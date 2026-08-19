# Audio effects
Some useful audio effects to manipulate each instrument are available.

## Amplitude Envelope 
We can define an amplitude envelope by setting the duration for a ADSR function:

```javascript
ampEnvelope(attack , decay , sustain, release)
```

For example:

```javascript
strings.reverb(1);
strings.ampEnvelope(1,2,3,4);
strings.play([C2, 1, 0], [Bb2, .9, .5], [Ab3, .8, 1], [Gb4, .7, 1.5], [E5, .6, 2], [D6, .5, 2.5]); // Alien sound
```

## Bend
We can bend the pitch of an instrument using the `bend` method. The amount is given in semitones.

```javascript
synth.bend(1); // Bends up by a semitone
```

## Cutoff
We control the filter cutoff amount with a 0 to 1 argument.

```javascript
synth.cutoff(0.5);
```

## Filter Envelope
We can define a filter envelope by setting the amount and the duration for an ADSR function:

```javascript
filterEnvelope(amount, attack, decay, sustain, release)
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
Panning values go from -1 (totally on the left) to 1 (totally on the right).

```javascript
drums2.pan(-1);
drums2.play([snare, .5, 0]);
```

### Auto-panning
```javascript
autoPan(instrument, times per seconds)
```

This function moves a sound around the stereo field from left to right and
back, in _n_ times per seconds. 

For example:

```javascript
synth.filterEnvelope(0.8, 1, 2, 3, 4);
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
