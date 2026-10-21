# Audio effects
Some useful audio effects to manipulate each instrument are available.

## Amplitude Envelope 
We can define an amplitude envelope by setting the duration for a ADSR function:

```javascript
ampEnvelope(attack time, decay time, sustain level, release time)
```

For example:

```javascript
strings.reverb(1);
strings.ampEnvelope(.5,1,1,10);
strings.play([C2, 1, 0], [Bb2, .9, .5], [Ab3, .8, 1], [Gb4, .7, 1.5], [E5, .6, 2], [D6, .5, 2.5]); // Alien sound
```

## Bend
We can bend the pitch of an instrument using the `bend` method. The amount is given in semitones.

```javascript
synth.bend(1); // Bends up by a semitone
```

### Glissando
```javascript
glissando([event], targetPitch)
```

Generates a smooth pitch bend from the initial event's pitch to the `targetPitch` over the duration of the event.

```javascript
let e = [C4, loud, 0, 2, fx2]
glissando(e, C5).play()
```


## Cutoff
We control the filter cutoff amount with a 0 to 1 argument.

```javascript
synth.cutoff(0.5);
```

## Delay
```javascript
delay(delay time, feedback amount)
```

A delay line can be added to an instrument with:
For example:
```javascript
xylophone.delay(.1, .9)
```

!!! note 
    Notice that the delay time is set in seconds and has a limit of 2 seconds.
    The feedback amount is a number from 0 to .99. Call `delay(0, 0)` to
    disable the delay line.

## Filter Envelope
We can define a filter envelope by setting the amount and the duration for an ADSR function:

```javascript
filterEnvelope(amount, attack, decay, sustain, release)
```

For example:

```javascript
synth.filterEnvelope(0.8, 1, 2, 3, 4);
```

## Frequency shift
Inharmonic sounds can be created from harmonic sounds by shifting frequencies
up or down:
```javascript
piano.shift(10)
piano.play()
```

We can do the same with samples:
```javascript
pianoc2.instr.shift(-10)
pianoc2.play()
```

## Pan 
Panning values go from 0 (totally on the left) to 1 (totally on the right).

```javascript
drums2.pan(0);
drums2.play([snare, .5, 0]);
```

### Auto-panning
```javascript
instrument.autoPan(hertz)
```

This method moves a sound around the stereo field from left to right and
back, in _n_ times per second. Use `instrument.autoPan(0)` to stop it.

For example:
```javascript
flute.autoPan(1)
flute.play()
```

## Resonance
We control the filter resonance with a 0 to 1 argument.

```javascript
synth.resonance(0.8);
```

## Reverb
We control the amount of reverb of an instrument with a 0 to 1 argument.

```javascript
drums.reverb(1);
drums.play([kick, .9, 2]);
```

## Volume
The overall volume of an instrument is set with a value from 0 to 1:

```javascript
piano.volume(.5);
piano.play(C4);
```

## Signal modifiers
In addition to the effects above, every instrument has four Csound signal
modifiers. Amounts are clamped to the ranges shown here; a value of `0` is a
bypass.

### Distortion
```javascript
instrument.distortion(amount)
```

Waveshapes the signal. `amount` goes from 0 to 1:

```javascript
guitar.distortion(.6);
guitar.play(E2);
```

### Highpass
```javascript
instrument.highpass(cutoff)
```

A highpass filter. `cutoff` goes from 0 (bypass) to 1 (brightest):

```javascript
piano.highpass(.4);
piano.play(C4);
```

### Tremolo
```javascript
instrument.tremolo(rate, depth)
```

Amplitude modulation at `rate` Hz with a `depth` from 0 to 1. Use a rate below
10 Hz; larger values are treated as 0:

```javascript
organ.tremolo(6, .6);
organ.play(G3);
```

### Chorus
```javascript
instrument.chorus(rate, depth)
```

A chorus/flanger effect. `rate` is in Hz and `depth` is in seconds, clamped to
0.025. Calling `chorus(0, 0)` disables it:

```javascript
strings.chorus(.25, .02);
strings.play([C4, 1, 0, 4]);
```

!!! note
    `delay()` and `chorus()` keep a per-instrument effect line running. Call
    `delay(0, 0)` or `chorus(0, 0)` to disable one, or `reset()` to stop
    everything and clear all effect state.
