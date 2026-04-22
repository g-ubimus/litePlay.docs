# Sequencer

The `sequencer` class acts as an automatic looping grid, reading data from
arrays passed on to it and scheduling them in time according to a _beats per
minute_ function, which tells the sequencer how much time each item in the
array takes up.

## Timing
Before starting a sequencer, we need to set a BPM running:

```JavaScript
setBpm(120);
```

Now we can define grids to set different subdivisions. The logic is as follows:

```
1: Each item is a quarter note (1 beat).
0.5: Each item is an eighth note (1/2 beat).
0.25: Each item is a sixteenth note (1/4 beat).
1/3: Each item is a triplet.
etc.
```

Considering this, let's define some different subdivisions:

```JavaScript
const grid4 = 1;     // Quarter notes
const grid8 = 0.5;   // 8th notes
const grid46 = 0.25; // 16th notes
```

## Patterns

At the core of the sequencer, we have patterns that will be repeated at the
defined grid subdivision.

Given that, let's create them:

```JavaScript
let backbeatPat = [kick, snare, kick, snare];
let bassPat = [[C3, 1, 0, 0.2], [C3, 1, 0, 0.2], [C4, 1, 0, 0.2], O, [Eb3, 1, 0, 0.2], O, [G3, 1, 0, 0.2], O];
let hatPat = [[closedHiHat, .2], [closedHiHat, 0.5], [openHiHat, .2], [closedHiHat, 0.5]];
let chordPat = [
      [[C4, 1, 0, 3.75], [Eb4, 1, 0, 3.75], [G4, 1, 0, 3.75], [Bb4, 1, 0, 3.75]], // Cm7
      O, O, O,
      [[Ab3, 1, 0, 3.75], [C4, 1, 0, 3.75], [Eb4, 1, 0, 3.75], [G4, 1, 0, 3.75]], // Abmaj7
      O, O, O,
  ];
```

!!! note
	Notice that we are using the capital O for silent events in the bass.

    Also notice that to get notes to play at same part in the chord pattern,
    we use nested brackets.

## Tracks

The sequencer can have multiple tracks playing at the same time, each one of
different lengths and playing at different subdivisions, but all syncronized to
the same BPM setting.

The parameters for a new sequencer track is as follows:

```javascript
sequencer.add(instrument, pattern, amplitude, grid);
```

Thus, we can go on and add our instruments:

```javascript
let backbeatTrack = sequencer.add(drums, kckPat, 0.8, grid46);
let hatTrack = sequencer.add(drums, hatPat, 0.3, grid46);
let bassTrack = sequencer.add(synthBass1, bassPat, 0.6, grid8);
let padTrack = sequencer.add(pad1, chordPat, 0.4, grid4);
```

We can wrap everything into a function:

```javascript
function synthWave() {

  sequencer.clear();
  setBpm(120);

  const grid4 = 1;     
  const grid8 = 0.5;   
  const grid46 = 0.25; 

  let backbeatPat = [kick, snare, kick, snare];
  let bassPat = [[C3, 1, 0, 0.2], [C3, 1, 0, 0.2], [C4, 1, 0, 0.2], O, [Eb3, 1, 0, 0.2], O, [G3, 1, 0, 0.2], O];
  let hatPat = [[closedHiHat, .2], [closedHiHat, 0.5], [openHiHat, .2], [closedHiHat, 0.5]];
  let chordPat = [
        [[C4, 1, 0, 3.75], [Eb4, 1, 0, 3.75], [G4, 1, 0, 3.75], [Bb4, 1, 0, 3.75]], // Cm7
        O, O, O,
        [[Ab3, 1, 0, 3.75], [C4, 1, 0, 3.75], [Eb4, 1, 0, 3.75], [G4, 1, 0, 3.75]], // Abmaj7
        O, O, O,
    ];
  let backbeatTrack = sequencer.add(drums, backbeatPat, 0.8, grid4);
  let hatTrack = sequencer.add(drums, hatPat, 0.3, grid46);
  let bassTrack = sequencer.add(synthBass1, bassPat, 0.6, grid8);
  let padTrack = sequencer.add(pad1, chordPat, 0.4, grid4);

  sequencer.play();
}

synthWave();
```

!!! note
    We use `sequencer.clear()` in the start of our function so we're able to
    modify its patterns and instantly call it again!

## Dynamic patterns
One problem with the example above is that it will always repeat in the same
way, without variations, which doesn't sound very musical...
To fix this, we can wrap a `choose()` function inside a pattern, making it 
choose between some options each time.

```javascript
let Cm7 = [[C4, 1, 0, 3.75], [Eb4, 1, 0, 3.75], [G4, 1, 0, 3.75], [Bb4, 1, 0, 3.75]];
let Abmaj7 = [[Ab3, 1, 0, 3.75], [C4, 1, 0, 3.75], [Eb4, 1, 0, 3.75], [G4, 1, 0, 3.75]];
let Ebmaj7 = [[Eb3, 1, 0, 3.75], [D4, 1, 0, 3.75], [G4, 1, 0, 3.75], [Bb4, 1, 0, 3.75]];
let Fm7 = [[F3, 1, 0, 3.75], [Eb3, 1, 0, 3.75], [C4, 1, 0, 3.75], [Ab4, 1, 0, 3.75]];

let chordPat = [
  () => choose(Cm7, Ebmaj7), O, O, O, 
  () => choose(Abmaj7, Fm7), O, O, O
];
```
