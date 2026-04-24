# Audio effects

Some useful audio effects to manipulate each instrument are available.

## Reverb
We control the amount of reverb of an instrument with a 0 to 1 argument.

```javascript
drums.reverb(1);
drums.play([kick, .9, 2]);
```
## Pan 
Panning values go from -1 (totally on the left) to 1 (totally on the right).

```javascript
drums2.pan(-1);
drums2.play([snare, .5, 0]);
```

## Amplitude Envelope 
We can define an amplitude envelope by setting the duration for a ADSR function:

```javascript
(attack , decay , sustain, release)
```

For example:

```javascript
strings.reverb(1);
strings.ampEnvelope(10,10,10,10);
strings.play([C2, 1, 0], [Bb2, .9, .5], [Ab3, .8, 1], [Gb4, .7, 1.5], [E5, .6, 2], [D6, .5, 2.5]); // Alien sound
```
