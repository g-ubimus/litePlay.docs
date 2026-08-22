# Contributing
Thank you for considering contributing to litePlay.js! There are several ways
to help out:

* Report bugs and suggest ideas through
  [issues](https://github.com/g-ubimus/litePlay.js/issues).
* Implement new functionality
* Improve the documentation or add examples

## Before you code
If you want to add new functionality, please open an issue first so it can be
discussed. It is desirable that new features adapt to already established event
model (`[what, howLoud, when, howLong, onSomething]`), follow design patterns
already in place, and if possible, rely on already existing classes, objects
and functions (like Instrument, Sample, eventList, sequencer and play).

## Setting up a development environment
Clone the repository and install its dependencies:

```
git clone git@github.com:g-ubimus/litePlay.js.git
cd litePlay.js
npm install
```

To try your changes in the browser, serve the repository root:

```
npx serve
```

All litePlay exports are global in this environment, so functions can be
called without any prefix. 

## Where things live
Most of the module is found in `src/core/`:

* `litePlay.js`: the audio engine, `play()`, `stop()`, `Instrument` and
  `Sampler` class, eventList and sequencer.
* `extra.js`: generators and musical structures (ostinato, arpeggio, euclidean
  rhythm, etc.).
* `litePlay.constants.js`: note constants, drum sounds and the loader that
  makes everything global.
* `litePlay.csd`: the Csound engine, driven by the General MIDI SoundFont, a
  sampler and sound effects (reverb, envelopes, delay, etc.).
* `editor.js`: the online editor interface.

Machine listening utilities live in `src/listener/`. These are a WIP and can  
change at any time.

## Submitting your work
1. Create a feature branch with a short descriptive name .
2. Make your changes and add tests.
3. Run `npm test` and make sure everything passes.
4. Open a pull request against `main`, linking the issue or describing the
   functionality. Include a small runnable example if necessary.

## Adding an audio effect to Instrument class
Per-instrument parameters (volume, pan, reverb, delay, etc.) live in Csound
[function
tables](https://flossmanual.csound.com/csound-language/function-tables),
indexed by the instrument's channel number. The JS side writes values into
these tables with `csound.tableSet()`, and every note is scheduled as a Csound
score event carrying the channel as its seventh parameter (p7). The sound
itself is produced by two instruments in `src/core/litePlay.csd`: `instr 10`
plays General MIDI presets from the soundfont, and `instr 12` plays audio
samples. Both read the parameter tables using p7 as the index, so adding a new
functionality usually takes three things:

* a table,
* a class method 
* some signal processing inside instruments 10 and 12

To make this concrete, we walk through how the frequency shifter (`shift`)
was implemented.

### How a parameter flows
1. The user calls the method on an instrument instance, say
   `flute.shift(10)`.
2. The method writes the value into a table, using the instrument's channel
   as index: `csound.tableSet(28, this.chn, val)`.
3. When a note is played, `score()` builds an event like
   `i10.16006 0 2 60 63.5 73 16`: the fractional part of the instrument
   number encodes note and channel, p4 is the pitch, p5 the velocity, p6 the
   GM program and p7 the channel.
4. Inside `instr 10`, the value is read back at k-rate:
   `kshift table p7, 28`.
5. The effect is applied to the signal (`a1`, `a2`) before it is sent to the
   pan, delay, reverb and master busses.

### Step 1: declare a table
Tables are declared in the `<CsScore>` section of `litePlay.csd`. Parameter
tables hold 1024 points, one per possible channel, and are initialised with a
[GEN routine](https://csound.com/manual/score/genroutines/) (usually `7`, a
straight line). Pick the next free number (at the time of writing, tables up to
f33 are in use):

```
f34 0 1024 7 0 1024 0  /* my parameter table */
```

### Step 2: add the class method
Add a method to the `Instrument` class in `src/core/litePlay.js`. Clamp the
input so that out-of-range values cannot reach the engine, as the existing
methods do. This is how `resonance` was implemented:

```javascript
resonance(amount) {
  csound.tableSet(18, this.chn, amount < 1 ? (amount > 0 ? amount : 0) : 1);
}
```

Because `Sampler` extends `Instrument`, audio samples get the new
functionality for free.

### Step 3: apply the effect in instruments 10 and 12
Read the table inside both instruments and process the audio. In `instr 10`:

```
kshift table p7,28 
a1, a2 Shift a1, a2, kshift
```

The same lines appear in `instr 12`, so that samples are affected too. Place
the processing before the panning, delay and reverb sends, unless the effect
is meant to bypass them.

### When you need a custom opcode
If the processing does not fit in a couple of lines, define a [user-defined
opcode (UDO)](https://csound.com/docs/manual/OrchUDO.html) before its first use,
near the top of `<CsInstruments>`. This is the `Shift` opcode, which shifts the
spectrum with a Hilbert transform:

```
opcode Shift, aa, aak
	ain1, ain2, kval xin
	areal1, aimag1 hilbert ain1
	areal2, aimag2 hilbert ain2
	asin oscili 1, kval, 29
	acos oscili 1, kval, 29, .25
	aout1 = (areal1*acos - aimag1*asin)
	aout2 = (areal2*acos - aimag2*asin)
	xout aout1, aout2
endop
```

The types after the name declare its output and input arguments: here two
audio signals in (`aa`), a k-rate control, and two audio signals out (`aak`).
The opcode is then called identically in instrs 10 and 12.

### Notes
* **i-rate vs k-rate reads.** Values that shape the note once (envelope
  times, filter envelope amount) are read at i-time, like
  `iatt table p7,23`. Continuous controls are read every control cycle at
  k-rate, like `kbend table p7,14`.
* **Persistent processes.** Some functionalities need a running instrument
  rather than a table value. The delay works this way: `delay()` schedules a
  per-channel instance of `instr 105` on demand
  (`csound.inputMessage("i105." + this.chn + " 0 -1 " + this.chn)`), keeps
  track of active channels in a JS `Set`, and `noDelay()` turns the instance
  off by scheduling it with a negative p1 (`csound.inputMessage("i-105." +
  this.chn + " 0 0.1 " + this.chn)`). If you add something similar, remember to
  register your instrument numbers in `instr 200` and `instr 300`, so that
  `reset()` stops and restarts everything correctly.
* **Clamping conventions.** Amounts from 0 to 1 are clamped with the idiom
  `amount < 1 ? (amount > 0 ? amount : 0) : 1`; pans and volumes are
  multiplied by 127 to match MIDI controller ranges.

### Checklist
* Table declared in `<CsScore>`, with an initial value and a comment
* Method added to the `Instrument` class, with clamped input
* Effect applied in **both** `instr 10` and `instr 12`
* Persistent instances registered in the reset instruments, if any
* Tried by ear with a GM instrument and with a sample
* Documented in English and Portuguese, following the house style

## Adding functions to extra.js
The `extra.js` file hosts complex musical structures in litePlay.js. These are
pure JS functions built on top of the module primitives. Most of them produce
music by returning an **eventList**, so callers can chain a `.play()` directly:

```javascript
euclidean({what: snare, howLong: .2, onSomething: drums}, 4, 8, 3).play()
```

To walk through the conventions, we look at how `ostinato()` was implemented.

### Accepting events: [event] or {event}
Functions that generate music take an event as their first argument, either as
an array or as an object. Both forms are normalised by the internal
`resolveEvent(input)` helper into the canonical five-element event, filling
omitted attributes with defaults (60, 0.8, 0, 1, piano). Portuguese keys
(`oQue`, `quãoForte`, `quando`, `quãoLongo`, `noQue`) are accepted
automatically. Functions then destructure the result:

```javascript
const [what, howLoud, when, howLong, onSomething] = resolveEvent(eventInput);
```

All three call styles below end up in the same place:

```javascript
ostinato(C4,10).play                                             // bare pitch
ostinato([C4, .8, 0, 1, piano]).play()                           // event as array
ostinato({what: C4, howLong: 1, onSomething: piano}, 10).play()  // event as object
```

Attributes may hold generator tokens like `midPitch` or `loud`. Each attribute
is resolved only when the function needs it, with the idiom `typeof when ===
"function" ? when() : when`.

### Parameters as objects
Optional parameters follow a dual signature: they can be given positionally
or gathered in a single options object. Defaults are declared first, the
object branch reads named keys with `??` fallbacks, and the positional branch
only overwrites what was provided:

```javascript
export function ostinato(eventInput, arg2, arg3) {
  let repetitions = 1;
  let rhythm;
  const [what, howLoud, when, howLong, onSomething] = resolveEvent(eventInput);
  const resolvedWhen = typeof when === "function" ? when() : when;
  const resolvedHowLong = typeof howLong === "function" ? howLong() : howLong;

  if (typeof arg2 === "object" && arg2 !== null && !Array.isArray(arg2)) {
    repetitions = arg2.repetitions ?? repetitions;
    rhythm = arg2.rhythm ?? rhythm;
  } else {
    if (arg2 !== undefined) repetitions = arg2;
    if (arg3 !== undefined) rhythm = arg3;
  }
```

Both styles below are therefore valid:

```javascript
ostinato(any, 10).play()
ostinato(any, {repetitions: 10}).play()
```

Another example using `arpeggio()`:

```javascript
arpeggio(any, any, 10).play()
arpeggio(any, {repetitions: 10}).play()
```

Notice how defaults can be musical rather than neutral: `randomChord()`
generates a random pitch set, so `arpeggio().play()` already plays something
interesting. `randomRhythm()` is another already available function that can be
used for the same end.

### Return an eventList
Build the result with `eventList.create()`, adding one event at a time while
tracking time manually, and return the list:

```javascript
const durations = rhythm || [resolvedHowLong];
const resolvePitch = (val) => (typeof val === "function" ? val() : val);
const firstPitch = resolvePitch(what);
let l = eventList.create([
    what,
    howLoud,
    resolvedWhen,
    resolvedHowLong,
    onSomething,
]);

for (let i = 0, len = repetitions; i < len; i++) {
  for (let j of durations) {
    initialTime += j;
    const currentPitch = resolvePitch(what);
    l.add([currentPitch, howLoud, initialTime, resolvedHowLong, onSomething]);
  }
}
return l;
```

Usually, we would resolve `when` and `howLong` generators once, before the
loops, so that events are scheduled sequentially instead of drawing a new
random duration per note. Re-resolve inside the loop only when per-note
variation is intended, as `ostinato()` does with its pitch. The returned list
supports the usual methods, so users can write `ostinato().play()` or
`ostinato().repeat(3)`.

### When not to return an eventList
List manipulation functions take lists in and return plain arrays. Copy the
input instead of mutating it, and throw a `TypeError` when the argument is not
an array:

```javascript
export function retrograde(list) {
  if (!Array.isArray(list)) {
    throw new TypeError("retrograde(): argument must be an array.");
  }
  return [...list].reverse();
}
```

There is one exception to the rule: `glissando()` needs real-time control of
an instrument bend, so it returns a custom object exposing `play` and `toque`
instead of an eventList. Avoid this pattern unless the effect cannot be
expressed as scheduled events.

### Conventions
* Random defaults through generators are welcome, as in
  `euclidean()`: `let steps = rndInt(4, 12);`.
* Keep internal helpers private (`resolveEvent`, `changeTempo`,
  `changeLoudness` have no `export`).
* Export a Portuguese alias at the bottom of the file
  (`export const arpejo = arpeggio;`), and accept Portuguese values where it
  makes sense (as `direction: "inversa"` does). Again, ask for it in the PR if
  you can't do it.

### Checklist
* Event parsed with `resolveEvent()` — accepts `[event]`, `{event}` and bare
  pitch
* Optional parameters work positionally and as an `{options}` object
* Returns an `eventList` built with `create()` / `add()` (or a justified
  exception)
* Generators resolved once for `when` / `howLong`; re-resolved per note only
  for variation
* Inputs validated, errors prefixed with the function name
* Portuguese alias exported at the bottom of the file
* Unit tests added to `tests/extra.test.js`
* Documented under `structures.md` (+ pt)
