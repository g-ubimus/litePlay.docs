# Sequencer

The `sequencer` object acts as a looper, reading data from arrays passed on to
it and scheduling them in time according to a _beats per minute_ function,
which tells the sequencer how much time each item in the array takes up.

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
const grid16 = 0.25; // 16th notes
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
      [[C4, 1, 0, 4], [Eb4, 1, 0, 4], [G4, 1, 0, 4], [Bb4, 1, 0, 4]], // Cm7
      O, O, O,
      [[Ab3, 1, 0, 4], [C4, 1, 0, 4], [Eb4, 1, 0, 4], [G4, 1, 0, 4]], // Abmaj7
      O, O, O,
  ];
```

!!! note
	We are using the capital O for silent events in the bass.

    Also, to get notes to play at same part in the chord pattern, we use nested
    brackets. The chord should go on for almost 4 beats (4), "overriding" the 
    moments of silence, which are counted anyway.

### Local subdivisions
The special `sub()` method allows us to have subdivisions in a pattern without
having to rewrite it in for new grid. For example:

```javascript
let backbeatPat = [kick, snare, kick, sub(snare, snare)];
```

Will give an eighth note for the last two snares, occupying the last beat.
Any number of subdivisions are possible, as well as silences (`O`) inside a
subdivision.

Nested divisions work just as expected:

```javascript
lat nestedPat = [kick, snare, kick, sub(snare, sub(snare, snare))];
```

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
let backbeatTrack = sequencer.add(drums, kckPat, 0.8, grid16);
let hatTrack = sequencer.add(drums, hatPat, 0.3, grid16);
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
  const grid16 = 0.25; 

  let backbeatPat = [kick, snare, kick, snare];
  let bassPat = [[C2, 1, 0, 0.2], [C2, 1, 0, 0.2], [C3, 1, 0, 0.2], O, [Eb2, 1, 0, 0.2], O, [G2, 1, 0, 0.2], O];
  let hatPat = [[closedHiHat, .2], [closedHiHat, 0.5], [openHiHat, .2], [closedHiHat, 0.5]];
  let chordPat = [
        [[C4, 1, 0, 4], [Eb4, 1, 0, 4], [G4, 1, 0, 4], [Bb4, 1, 0, 4]], // Cm7
        O, O, O,
        [[Ab3, 1, 0, 4], [C4, 1, 0, 4], [Eb4, 1, 0, 4], [G4, 1, 0, 4]], // Abmaj7
        O, O, O,
    ];
  let backbeatTrack = sequencer.add(drums, backbeatPat, 0.8, grid4);
  let hatTrack = sequencer.add(drums, hatPat, 0.3, grid16);
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
Replace the previous code for `chordPat` with the following:

```javascript
  let Cm7 = [[C4, 1, 0, 4], [Eb4, 1, 0, 4], [G4, 1, 0, 4], [Bb4, 1, 0, 4]];
  let Abmaj7 = [[Ab3, 1, 0, 4], [C4, 1, 0, 4], [Eb4, 1, 0, 4], [G4, 1, 0, 4]];
  let Ebmaj7 = [[Eb3, 1, 0, 4], [D4, 1, 0, 4], [G4, 1, 0, 4], [Bb4, 1, 0, 4]];
  let Fm7 = [[F3, 1, 0, 4], [Eb3, 1, 0, 4], [C4, 1, 0, 4], [Ab4, 1, 0, 4]];
  
  let chordPat = [
    () => choose(Cm7, Ebmaj7), O, O, O, 
    () => choose(Abmaj7, Fm7), O, O, O
  ];
```

## Another sequencer example
```javascript
// sequencer patterns
let riff, melody, shuf, kck, snr;
// sequencer tracks
let cymbals, kicks, snares, bassline, topline;
const MSECS = 1000;

// sequencer setup
function sequence() {
  sequencer.clear();
  // set beat division
  const beatDiv = 1 / 3; 
  // set the BPM
  setBpm(100);
  
  // eventLists for sequencer
  riff = [Eb2, [G2, 1, 0, 1], Bb2, [Db2, 2, 0.75, 0.2]];
  melody = [
    [[Eb4, 3, 0, 2, organ], [Bb4, 3, 0, 2, organ], [G5, 1, 0, 2]],
    [[F4, 3, 0, 2, organ], [Ab4, 3, 0, 2, organ], [Db5, 1, 0, 3.5]],
    O,
    [Bb4, 1, 0.5, 0.5],
    [[Eb4, 3, 0, 1, organ], [G4, 3, 0, 1, organ], [C5]],
    O,
    Bb4,
    Ab4,
  ];
  shuf = [[cymbal, 1, 0, 1 / 3], O, [cymbal, 0.9], [cymbal, 0.9]];
  kck = [[kick, 0.5], O, [kick, 1]];
  snr = [snare, O];

  // set up sequencer tracks
  // sequencer.add(instrument, events, amplitude, beatDiv)
  cymbals = sequencer.add(drums, shuf, 0.5, beatDiv);
  kicks = sequencer.add(drums, kck, 0.1, beatDiv);
  snares = sequencer.add(drums, snr, 0.1);
  bassline = sequencer.add(piano, riff, 0.5);
  topline = sequencer.add(synth, melody, 0.1);
  
  // run sequencer
  sequencer.play();
}

// arpeggio 
function arp() {
  // create an eventList with four events, each defined by
  // instrument.event(what, howLoud, when, howLong)
  const eList = eventList.create(
     piano.event(Eb7, 0.1, 0, 1),
     piano.event(Bb6, 0.1, 0.25, 1),
     piano.event(G6, 0.1, 0.75, 1),
     piano.event(Eb6, 0.1, 1, 1)
  );
  // set a function to play this in the litePlay sequencer
  sequencer.addCallback((t) => {
    eList.play(t);
  });
}
sequence();
arp();
```
