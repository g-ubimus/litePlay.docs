# Examples
 
This page lists useful musical examples and extra functions available in the
system to give a grasp of how litePlay.js works.

## Pitch & harmony
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
transposition(melody, semitones);
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
arpeggio(chord, repetitions, speed, direction, amp, instrument)
```
By default, the `arpeggio` function plays a _fast_ "upAndDown" arpeggio of a
_random chord_, one time:
```JavaScript
arpeggio();
```

You can pass a chord to the first parameter:
```javascript
let cmaj7 = [C3, B3, E4, G4];
arpeggio(cmaj7);
```

Define a different number of repetitions:
```javascript
let cmaj7 = [C3, B3, E4, G4];
arpeggio(cmaj7, 10);
```

Change the speed (`0.25` seconds per note by default):
```javascript
let cmaj7 = [C3, B3, E4, G4];
arpeggio(cmaj7, 5, .1);
```

Change the direction (`"upAndDown"` by default):
```javascript
let cmaj7 = [C3, B3, E4, G4];
arpeggio(cmaj7, 5, .1, "up");
```

```javascript
let g7 = [G3, D4, F4, B4];
arpeggio(g7, 5, .1, "down");
```

Change the amplitude (`1` by default):
```javascript
let cmaj7 = [C3, B3, E4, G4];
arpeggio(cmaj7, 5, .1, "upAndDown", .5);
```

And, of course, change the instrument (`piano` by default):
```javascript
let cmaj7 = [C3, B3, E4, G4];
arpeggio(cmaj7, 5, .1, "upAndDown", .5, xylophone);
```

### Interval sequence
```JavaScript
intervalSequence([event], interval, repetitions, up = true)
```

A different type of arpeggio can be constructed using an interval sequence.

By default, it starts on a random note and goes up four notes, following a
random interval:
```javascript
intervalSequence();
```

Create an event and use it as the basis for the function:
```javascript
let e = [E2, .8, 0, .5, guitar];
intervalSequence(e);
```

Select an specific interval for the sequence:
```javascript
let e = [E2, .8, 0, .5, guitar];
intervalSequence(e, 3);
```

Change the number of times it will transpose:
```javascript
let e = [E2, .8, 0, .5, guitar];
intervalSequence(e, 3, 10);
```

Go down by changing the last parameter to false:
```javascript
let p = [E6, .8, 0, .1, piano];
intervalSequence(e, 3, 10, false);
```

### Inversion
```JavaScript
invert(melody, axis)
```

Invert an array of pitches around an axis:
```javascript
let melody = [C4, D4, E4, F4];
let inverted = invert(melody, C4);
console.log(inverted); // returns ["C4","As/Bb4","Gs/Ab4","G4"]
```

### Get note names
```JavaScript
midiToName(60)); // returns "C4"
midiToName(31)); // returns "G1"
```

## Rhythm & timing
### Tempo variation (accelerando & rallentando)
```JavaScript
function tempoVariation(what, when, duration, steps, ratio) {
      if (steps <= 0) return;
      play([what, .9, when, duration, drums6]);
      let nextDuration = duration * ratio;
      tempoVariation(what, when + duration, nextDuration, steps - 1, ratio);
}

tempoVariation(membranofone, .1, 1, 20, .9); // accelerando
//tempoVariation(idiophone, 1, .1, 20, 1.1); // rallentando
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

## Dynamics
```JavaScript
function ampVariation(first, last, steps, duration, delta) {
  if (!steps) return;  
  let ampStep = (last - first) / steps; 
  play([midPitch, first, delta, duration]);
  ampVariation(first + ampStep, last, steps - 1, duration, delta + duration);
}

ampVariation(.1, 1, 10, 0.2, 0.5); // crescendo
// ampVariation(1, .1, 10, 0.2, 0.5); // invert values to get a decrescendo
```

## Manipulating audio effects
### Auto-panning
```javascript
function autoPan(instrument, cycleInSeconds) {
  if (instrument.panInterval) {
    clearInterval(instrument.panInterval);
  }
  
  instrument.panInterval = setInterval(() => {
    let timeInSeconds = Date.now() / 1000;    
    let panValue = Math.sin((timeInSeconds / cycleInSeconds) * Math.PI * 2);    
    instrument.pan(panValue);
  }, 30); 
}
```

We can use it with a sequencer:

```javascript
function autechreMontreal() {
  sequencer.clear();
  setBpm(90);

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
function retrograde(list) {
  return list.map((item, index, arr) => {let oppositeIndex = arr.length - 1 - index;    
    return arr[oppositeIndex];});
}

let myList = [1, 2, 3, 4];
let retro = retrograde(myList);
console.log(retro) // returns [4, 3, 2, 1]
```

### Rotation
```JavaScript
function rotate(list, steps) {
  return list.map((note, index, arr) => {
    let newIndex = (index + steps) % arr.length;
    if (newIndex < 0) newIndex += arr.length;
    return arr[newIndex];
  });
}

let melody = [1, 2, 3, 4];
let rotated = rotate(melody, 1);
console.log(rotated) // returns [2, 3, 4, 1]
```

### Tangle
```JavaScript
function tangle(listA, listB) {
  let listC = [];
  let size = Math.max(listA.length, listB.length);
  for (let i = 0; i < size; i++) {
    listC.push(listA[i % listA.length]);
    listC.push(listB[i % listB.length]);
  }
  return listC;
}

let listA = [1,2,3];
let listB = [4,5,6,7,8];
console.log(tangle(listA,listB)); // returns [1, 4, 2, 5, 3, 6, 1, 7, 2, 8]
```
