# Play

At the core of litePlay.js we have `play()`. This can be simply run as

```javascript
play();
```

to play a sound, and `stop()` to stop it.

We can make changes to this sound by setting the default instrument playing it,

```javascript
instrument(organ);
play();
```

the default was set to piano at the start, but we now changed it to
organ. You can see all instruments available [here](./instruments.md).

```javascript
stop();
lead1.play();
```

!!! note
	See? We can select a new instrument and play it in a single line!
	However, to stop it, now we should do: `lead1.stop();`.
	
## Other forms of `play()`

Besides playing notes and event lists, `play()` accepts an instrument or a
function:

```javascript
play(organ);          // call organ.play()
play(() => E4);       // call the function, then play its result
```

`stop()` only stops the default instrument. Use `instrument.stop()` for a
specific instrument, or `reset()` to stop the default instrument, the
sequencer, and clear the effect state:

```javascript
lead1.stop();
reset();
```

We can also, of course, tell litePlay _what_ to play. If the sound is coming
from a pitched instrument, we can ask it to play a given pitch,

```javascript
play(E4);
```

The pitch symbolic names range from Cm1 (C<sub>-1</sub>) to G8 (G<sub>8</sub>),
in such a way that the middle C of a piano keyboard is set to C4. The
sound is played immediately and lasts indefinitely (although some
may decay in intensity over time).

Accidentals are represented by lowercase `s` for _sharp_ and `b` for flat _notes_:

```
play(Eb4);
play(Fs4);
```

But the system also supports microtonal tunings set from midi values. To get
them, just add fractional parts to it. For example:

```
play(60); // C4
play(60.5); // between C4 and C#4
play(60.55); // a bit higher than that!
```

See more details in [here](./structures/#tuning).

## Events

In litePlay.js we can think of the resulting action of `play()` in this form is
a musical _event_. We can define it with five attributes:

- _what_: the thing that we play, which may vary, but in the case shown
  earlier, is the pitch of the sound.

- _howLoud_: the level of the sound, which can be set by a numeric value in a
  scale from 0 to 1.

- _when_: the event time, when it is supposed to be happening, in beats.

- _howLong_: how long the sound is to last for, in beats.

- _onSomething_: the thing that will be making the sound, the instrument, such
  as `organ`, `violin` etc. There are several of these to choose from.
  Depending on the type of instrument, _what_ can be played may vary. For
  example, in the case of `drums`, we do not have pitch, but different
  percussion sounds like `snare`, `kick`,  etc.

!!! note
    Event times are in beats, not seconds. At the default 60 BPM one beat is
    one second; at 120 BPM one beat is half a second. Use `secs()` and
    `beats()` to convert between the two.

Event attributes (or parameters) are optional, as we have seen. If we do pass
them, defaults are used. It is possible to pass only a few parameters, for
example just _what_; _what_ and _howLoud_; _what_, _howLoud_, and _when_; as
well as _what_, _howLoud_,_when_, and _howLong_.

Events are passed using a JS list (or array) with attributes in the order
listed earlier:

```javascript
[what,  howLoud, when, howLong, onSomething]
```

For example,

```javascript
play([C4, 0.5, 0, 2, violin])
```

## Unordered parameters 
Instead of using an array, events can also be defined by using an object, in
which ordering of parameters do not matter, for example:

```javascript
play({when: 0, what: C4, howLoud: 0.5, onSomething: violin, howLong: 2})
```

Is the same as the previous example.

## More than one event
The top-level `play()` action can take several events as arguments, such
as

```javascript
play([C4, 0.1, 0, 3], [E4, 0.2, 0.5, 0.5], [G4, 0.4, 2, 0.1])
```

Now we learned that can we work with lists of events, instead of only sending
individual events to `play()`. There is a particular aspect of this that we
should note, the _when_ attributes of each event will be interpreted in a
certain way.

### A simple list of _what_

```javascript
play(C3, C4, C5);
```

In this case, the events will be separated by the default _howLong_ for the
_onSomething_ being played (one beat, which is one second at the default
60 BPM). So we hear the sounds in sequence.

### A list of incomplete events

```javascript
play([C3], [C4], [C5]);
```

In this case, the default for _when_ is 0, immediately, for all events in the
list. We hear the sounds starting at the same time, mixed up.

### A list of events with _when_ attributes explicitly defined

```javascript
play([C3, 0.1, 0], [C4, 0.2, 1], [C5, 0.4, 2]);
```

In this case, the timing of events is relative to the time we asked for the
event list to be played, with perhaps a very short delay. All events are
precisely timed in relation to that. We can decide when these should come in
with an exact time.

## Silent events

For any instrument, we get a silent event by passing a capital O to the _what_
parameter:

```javascript
trumpet.play([O, 1, 0, 1])
```

!!! note 
	Silent events could also be generated by setting the amplitude at zero,
	however, the intention would not be so clear in the code, as you're 
	asking an instrument to play _something_.

## `any`

`any` (also exported as `algum`) is simply `undefined`. It is useful as a
placeholder event argument when a function expects an event but you want the
defaults, for example:

```javascript
arpeggio(any, [C4, E4, G4]).play();
blockChord(any, [C4, E4, G4]).play();
```

`resolveEvent()` fills the default pitch, loudness, time, duration and
instrument for `any`. It is not a wildcard inside an event array; for partial
events use the object form, or omit trailing values:

```javascript
play({what: E4, when: 0, howLong: 1});
play([E4, 1, 0]);
```

What if a list of events become an [object](./eventList.md)?
