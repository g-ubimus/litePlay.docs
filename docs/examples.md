# Examples
 
This page lists useful musical examples to give a grasp of how litePlay.js works.

## Pitch & harmony
### Tunning
We can have microtonal pitches in different formats. The simplest one would be
to add fractional parts to a midi value:

```JavaScript
play(60.25);
```

However, for it to work, we should now the note values! One way to do it is to
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

### Arpeggiator
```JavaScript
function arpeggiate(chord, stepTime, repeats, l = eventList.create(), time = 0) {
 if (repeats <= 0) {
   l.play();
 } else {
   for (let i = 0; i < chord.length; i++) {
     l.add([chord[i], 1, time, stepTime, xylophone]);
     time += stepTime; 
   }
   arpeggiate(chord, stepTime, repeats - 1, l, time);
 }
}
let Cmaj7 = [C4, E4, G4, B4];
arpeggiate(Cmaj7, 0.15, 5);
```

#### Up-and-down arpeggio
```JavaScript
function upAndDown(chord, stepTime, repeats, l = eventList.create(), time = 0) {
  if (!repeats) {
    l.play(); 
  } else {    
    for (let i = 0; i < chord.length; i++) {
      l.add([chord[i], 1, time, stepTime, xylophone]);
      time += stepTime; 
    }    
    for (let i = chord.length - 2; i > 0; i--) {
      l.add([chord[i], 1, time, stepTime, xylophone]);
      time += stepTime; 
    }    
    upAndDown(chord, stepTime, repeats - 1, l, time);
  }
}
let Cmaj7 = [C4, E4, G4, B4];
upAndDown(Cmaj7, 0.15, 10);
```

### Interval sequence
```JavaScript
function sequence(note, interval, duration, delta, repeats) {
      if (!repeats) return;
      play([note, 1, delta, duration, vibraphone]);
      sequence(note + interval, interval, duration, delta + duration, repeats - 1);
}

sequence(C4, 3, .5, 0.1, 10);
```

### Inversion
```JavaScript
function invert(melody, axis) {
  return melody.map(note => axis + (((axis - note) % 12) + 12) % 12);
}
let melody = [C4, D4, E4, F4];
let inverted = invert(melody, C4);
console.log(inverted); // returns [C4, Bb4, Ab4, G4]
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

## List manipulation
### Transposition
```JavaScript
function transpose(melody, semitones) {
  return melody.map(note => note + semitones);
}

let melody = [C4, D4, E4, F4, G4];
let transposed = transpose(melody, 2);
console.log(transposed) // returns [D4, E4, Fs4, G4, A4]
```

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

