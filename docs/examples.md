# Examples
 
This page lists useful musical examples and extra functions available in the
system to give a grasp of how litePlay.js works.

## Pitch & harmony
### Get note names
```JavaScript
midiToName(60)); // returns "C4"
midiToName(31)); // returns "G1"
```

### Tunning
We can have microtonal pitches in different formats. The simplest one would be
to add fractional parts to a midi value:

```JavaScript
play(60.25);
```

However, for it to work, we should know the pitch values! One way to do it is to
print it to the console:

```JavaScript
console.log(C4); // returns 60
```

A different way, which doesn't require specific midi knowledge, is to take the
advantage of some constants:

```JavaScript
play(C4+quarterTone); // equivalent to 60.5
play(C4+thirdTone); // equivalent to 60.33
play(C4+eightTone); // equivalent to 60.25
play(C4+tenCent); // equivalent to 60.1
play(C4+oneCent); // equivalent to 60.01
```

### Transposition
```javascript
transpose(melody, semitones);
```

Transpose an array of notes by a number of semitones:

```JavaScript
let m = [C4, D4, E4, F4, G4];
let s = 2;
let transposed = transpose(m, s);

console.log(transposed) // returns [D4, E4, Fs4/Gb4, G4, A4]
```

### Random chord
```javascript
randomChord(size, range, microtonal = false);
```

Generates a chord (4 notes by default, in `midPitch` range) with random
pitches:
```javascript
let a = randomChord();
console.log(a);
```

Change the size of the chord with the first parameter:
```javascript
let fiveNoteChord = randomChord(5);
console.log(fiveNoteChord);
```

Change the range with the second parameter, using litePlay's pitch generators:
```javascript
let highPitchChord = randomChord(4, highPitch);
let lowPitchChord = randomChord(4, lowPitch);
console.log(highPitchChord);
console.log(lowPitchChord);
```

Make microtonal chords:
```javascript
let microtonalChord = randomChord(4, midPitch, true);
console.log(microtonalChord);
```

### Arpeggiator
```javascript
arpeggio([event], [chord], repeats, direction = "upAndDown")
```
By default, the `arpeggio` function plays an "upAndDown" arpeggio of a
_random chord_, one time:
```JavaScript
arpeggio().play();
```

You can pass an event to define note parameters, but it still plays a random
chord:
```javascript
let e = [C3, .5, 0, .1, xylophone];
arpeggio(e).play();
```

Define the chord by passing it as the second parameter:
```javascript
let e = [C3, .5, 0, .1, xylophone];
let cmaj7 = [C4, E4, G4, B4];
arpeggio(e, cmaj7).play();
```

Define a different number of repetitions:
```javascript
let e = [C3, .5, 0, .1, xylophone];
let cmaj7 = [C4, E4, G4, B4];
arpeggio(e, cmaj7, 10).play();
```

Change the direction ("up", "down", or "upAndDown"):
```javascript
let e = [C3, .5, 0, .1, xylophone];
let cmaj7 = [C4, E4, G4, B4];
arpeggio(e, cmaj7, 10, "up").play();
```

Concatenate arpeggios!
```javascript
let a = arpeggio([C4, .8, 0, .2, harpsichord], [C4, E4, G4, C5, E5, G4, C5, E5], 2, "up").play();
let b = arpeggio([D4, .8, a, .2, harpsichord], [C4, D4, A4, D5, F5, A4, D5, F5], 2, "up").play();
let c = arpeggio([G4, .8, b, .2, harpsichord], [B4, D4, G4, D5, F5, G4, D5, F5], 2, "up").play();
let d = arpeggio([C4, .8, c, .2, harpsichord], [C4, E4, G4, C5, E5, G4, C5, E5], 2, "up").play();
```

### Interval sequence
```JavaScript
intervalSequence([event], interval, repetitions, up?)
```

A different type of arpeggio can be constructed using an interval sequence.

By default, it starts on a random note and goes up or down four notes,
following a random interval:
```javascript
intervalSequence().play();
```

Create an event and use it as the basis for the function:
```javascript
let e = [E2, .8, 0, .5, guitar];
intervalSequence(e).play();
```

Select a specific interval for the sequence:
```javascript
let e = [E2, .8, 0, .5, guitar];
intervalSequence(e, 3).play();
```

Change the number of times it will transpose:
```javascript
let e = [E2, .8, 0, .5, guitar];
intervalSequence(e, 3, 10).play();
```

Make it only go up:
```javascript
let e = [E2, .8, 0, .1, piano];
intervalSequence(e, 3, 10, true).play();
```

Force it to go down by changing the last parameter to false:
```javascript
let e = [E6, .8, 0, .1, piano];
intervalSequence(e, 3, 10, false).play();
```

### Inversion
```JavaScript
invert(melody, axis)
```

Invert an array of pitches around an axis:
```javascript
let melody = [C4, D4, E4, F4];
let inverted = invert(melody, C4);
console.log(inverted); // returns ["D4", "E4", "Bb4","Ab4","G4"]
```

## Rhythm & timing
### Set/get beats per minute
```JavaScript
setBpm(value);
console.log(getBpm());
```

### Convertions
```JavaScript
secs(beats);
// return duration of a beat in seconds

beats(seconds)
// return number of beats from an interval in seconds
```

### Get current audio clock time
```JavaScript
console.log(audioClock())
```

### Tempo variation (accelerando & rallentando)
```JavaScript
tempoVariation([event], steps, ratio)
```

Generates an accelerando when the `ratio` < 1:
```javascript
let e = [membranophone, .1, 0, 1, drums2];
tempoVariation(e, 20, .9).play();
```

Or a rallentando when `ratio` > 1:
```javascript
let e = [idiophone, .1, 0, .1, drums4];
tempoVariation(e, 20, 1.1).play();
```

### Amen Break
```javascript
function amenBreak() {
  sequencer.clear();
  setBpm(130);
  let grid = .5;

  let hhPat = [[openHiHat, .3]];
  let kkPat = [kick, kick, O, O, O, sub(kick, kick), O, O, kick, kick, O, O, O, sub(kick, kick), O, O, kick, kick, O, O, O, kick, O, O, O, sub(kick, kick), O, O, O, [kick, 1], O, O];
  let snPat = [O, O, [snare, 1], sub(O, [snare, .5]), sub(O, [snare, .5]), O, [snare, 1], sub(O, [snare, .5]), 
               O, O, [snare, 1], sub(O, [snare, .5]), sub(O, [snare, .5]), O, [snare, 1], sub(O, [snare, .5]),
               O, O, [snare, 1], sub(O, [snare, .5]), sub(O, [snare, .5]), O, O, [snare, .5],
               sub(O, [snare, .5]), O, [snare, 1], sub(O, [snare, .5]), sub(O,  [snare, .5]), O, O, [snare, .5]];

  sequencer.add(drums, hhPat, 1, grid);
  sequencer.add(drums, kkPat, 1, grid);
  sequencer.add(drums, snPat, 1, grid);

  sequencer.play();
}

amenBreak()
```

## Dynamics
### Crescendo and decrescendo
```JavaScript
ampVariation([event], lastAmp, steps)
```
Similar to the accelerando/rallentando method.

Do a crescendo with a soft event going, for example, to maximum amplitude (1):
```javascript
let a = [C4, .1, 0, 1, piano];
ampVariation(a, 1, 10).play();
```

Do a decrescendo with the opposite:
```javascript
let a = [C4, 1, 0, 1, piano];
ampVariation(a, .1, 10).play();
```

## Manipulating audio effects
### Auto-panning
```javascript
autoPan(instrument, hertz)
```

We can use it with a sequencer:

```javascript
function autechreMontreal() {
  sequencer.clear();
  setBpm(100);

  let grid16 = .5;
  let grid4 = 1;

  let hhPat = [pedalHiHat, sub(pedalHiHat, pedalHiHat), pedalHiHat, sub(pedalHiHat, pedalHiHat), sub(pedalHiHat, pedalHiHat), pedalHiHat];  
  let bassPat = [O, sub(O, [Gb2, 1, 0, 1]), sub(O, [Gb2, 1, 0, 2]), O,
                 [A2, 1, 0, 2], sub(O, [A2, 1, 0, 2]), O, O];
  let kkPat = [kick, sub(O, kick), sub(O, kick), sub(O, [kick, 1, 0, 3], O, O)];
  
  autoPan(drums1, .1);
  synthBass1.reverb(.4);

  sequencer.add(drums1, hhPat, .4, grid16);
  sequencer.add(synthBass1, bassPat, .3, grid4);
  sequencer.add(drums2, kkPat, .9, grid4);

  sequencer.play();
}

autechreMontreal()
```

## List manipulation
### Retrograde
```JavaScript
retrograde(list)
```

```JavaScript
let myList = [1, 2, 3, 4];
let retro = retrograde(myList);
console.log(retro) // returns [4, 3, 2, 1]
```

### Rotation
```JavaScript
rotate(list, steps)
```

```JavaScript
let melody = [1, 2, 3, 4];
let rotated = rotate(melody, 2);
console.log(rotated) // returns [3,4,1,2]
```

### Tangle
```JavaScript
tangle(listA, listB)
```

```javascript
let listA = [1,2,3];
let listB = [4,5,6,7,8];
console.log(tangle(listA,listB)); // returns [1, 4, 2, 5, 3, 6, 1, 7, 2, 8]
```
