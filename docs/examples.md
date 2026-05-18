# Examples
This page explores musical examples using litePlay.

## Prelude, BWV 846
```javascript
let a = arpeggio([C4, .8, 0, .2, harpsichord], [C4, E4, G4, C5, E5, G4, C5, E5], 2, "up").play()
let b = arpeggio([D4, .8, a, .2, harpsichord], [C4, D4, A4, D5, F5, A4, D5, F5], 2, "up").play()
let c = arpeggio([G4, .8, b, .2, harpsichord], [B3, D4, G4, D5, F5, G4, D5, F5], 2, "up").play()
let d = arpeggio([C4, .8, c, .2, harpsichord], [C4, E4, G4, C5, E5, G4, C5, E5], 2, "up").play()
//etc.
```

## Amen Break
```javascript
function amenBreak() {
  sequencer.clear()
  setBpm(130)
  let grid = .5

  let hhPat = [[openHiHat, .3]]
  let kkPat = [kick, kick, O, O, O, sub(kick, kick), O, O, kick, kick, O, O, O, sub(kick, kick), O, O, kick, kick, O, O, O, kick, O, O, O, sub(kick, kick), O, O, O, [kick, 1], O, O]
  let snPat = [O, O, [snare, 1], sub(O, [snare, .5]), sub(O, [snare, .5]), O, [snare, 1], sub(O, [snare, .5]), 
               O, O, [snare, 1], sub(O, [snare, .5]), sub(O, [snare, .5]), O, [snare, 1], sub(O, [snare, .5]),
               O, O, [snare, 1], sub(O, [snare, .5]), sub(O, [snare, .5]), O, O, [snare, .5],
               sub(O, [snare, .5]), O, [snare, 1], sub(O, [snare, .5]), sub(O,  [snare, .5]), O, O, [snare, .5]]

  sequencer.add(drums, hhPat, 1, grid)
  sequencer.add(drums, kkPat, 1, grid)
  sequencer.add(drums, snPat, 1, grid)

  sequencer.play()
}

amenBreak()
```

## Autechre vibes
```javascript
function montreal() {
  sequencer.clear()
  setBpm(100)

  let grid16 = .5
  let grid4 = 1

  let hhPat = [pedalHiHat, sub(pedalHiHat, pedalHiHat), pedalHiHat, sub(pedalHiHat, pedalHiHat), sub(pedalHiHat, pedalHiHat), pedalHiHat]  
  let bassPat = [O, sub(O, [Gb2, 1, 0, 1]), sub(O, [Gb2, 1, 0, 2]), O,
                 [A2, 1, 0, 2], sub(O, [A2, 1, 0, 2]), O, O]
  let kkPat = [kick, sub(O, kick), sub(O, kick), sub(O, [kick, 1, 0, 3], O, O)]
  
  autoPan(drums1, .1)
  synthBass1.reverb(.4)

  sequencer.add(drums1, hhPat, .4, grid16)
  sequencer.add(synthBass1, bassPat, .3, grid4)
  sequencer.add(drums2, kkPat, .9, grid4)

  sequencer.play()
}

montreal()
```

## Clapping music
```javascript
function clappingMusic() {
  setBpm(180);
  sequencer.clear()
  drums1.pan(-1)
  drums2.pan(1)
  let grid = .5
  let pattern = [[clap, loud, 0, .25], [clap, loud, 0, .25], [clap, loud, 0, .25], 
                 O, [clap, loud, 0, .25], [clap, loud, 0, .25], 
                 O, [clap, loud, 0, .25], O,
                [clap, loud, 0, .25], [clap, loud, 0, .25], O];
  pattern = pattern.concat(pattern)
  let pattern2 = pattern
  for (let i = 1; i < 12; i++) {
    pattern2 = pattern2.concat(rotate(pattern,i))
  }
  sequencer.add(drums, pattern, .5, grid)
  sequencer.add(drums2, pattern2, .5, grid)
  sequencer.play()
}

clappingMusic()
```

## Piano phase
```javascript
piano.reverb(.9)
piano.pan(-.5)
brightPiano.pan(.5)
brightPiano.reverb(.9)
arpeggio({howLoud: loud, when: 0, howLong:.125, onSomething:piano}, [E4, Fs4, B4, Cs5, D5, Fs4, E4, Cs5, B4, Fs4, D5, Cs5], 300, "forward").play()
arpeggio({howLoud: loud, when: 0, howLong:.1245, onSomething:brightPiano}, [E4, Fs4, B4, Cs5, D5, Fs4, E4, Cs5, B4, Fs4, D5, Cs5], 300, "forward").play()
```
