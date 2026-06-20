# Musical structures
## Pitch & harmony
### Get note names
```JavaScript
console.log(midiToName(60)) // returns "C4"
console.log(midiToName(31)) // returns "G1"
```

### Tuning
We can have microtonal pitches in different formats. The simplest one would be
to add fractional parts to a midi value:

```JavaScript
play(60.25)
```

However, for it to work, we should know the pitch values! One way to do it is to
print it to the console:

```JavaScript
console.log(C4) // returns 60
```

A different way, which doesn't require specific midi knowledge, is to take the
advantage of some constants:

```JavaScript
play(C4+quarterTone) // equivalent to 60.5
play(C4+thirdTone) // equivalent to 60.33
play(C4+eighthTone) // equivalent to 60.25
play(C4+tenCent) // equivalent to 60.1
play(C4+oneCent) // equivalent to 60.01
```

### Transposition
```javascript
transpose([melody], interval)
```

Transpose an array of notes by an interval:

```JavaScript
let m = [C4, D4, E4, F4, G4]
let s = 2
let transposed = transpose(m, s)

console.log(midiToName(transposed)) // returns [D4, E4, Fs4/Gb4, G4, A4]
```

### EDO tunings
```javascript
edo(number of divisions)
```

Creates a scale with a number of _equal divisions of an octave_, informed to
its input parameter.

As the output ranges from 0 to 12 (in midi values), the user has to transpose
it to desired range:

```javascript
let scale = edo(19)
let transposedScale = transpose(scale, A4) //19 note scale starting on A4
```

### Just intonation
```javascript
justIntonation(base pitch, number of harmonics)
```

In a similar fashion, the `justIntonation()` function creates a scale by
fitting the pitch classes found on the harmonic series of a pitch inside an
octave.

By default, it returns the scale constructed from the first thirteen harmonics
on C3, resembling the [acoustic scale](https://en.wikipedia.org/wiki/Acoustic_scale),
or the same as:

```javascript
justIntonation(C3, 13)
```

### Monotone
```javascript
monotone(initial pitch, interval)
```

Get _mostly_ the same pitch (50% chance), but also pitches that can vary in the
range passed to the second parameter, which is, by default, one semitone up or
down:

For example:
```javascript
let e = [()=>monotone(C4), loud, 0, .1, clarinet]
ostinato(e, 100).play()
```

If the interval has decimals, microtones would be included:
```javascript
let m = [()=>monotone(C4, 2.1), loud, 0, .1, clarinet]
ostinato(m, 100).play()
```

### Random chord
```javascript
randomChord(size, range, microtonal = false)
```

Generates a chord (4 notes by default, in `midPitch` range) with random
pitches:
```javascript
let a = randomChord()
console.log(a)
```

Change the size of the chord with the first parameter:
```javascript
let fiveNoteChord = randomChord(5)
console.log(fiveNoteChord)
```

Change the range with the second parameter, using litePlay's pitch generators:
```javascript
let highPitchChord = randomChord(4, highPitch)
let lowPitchChord = randomChord(4, lowPitch)
console.log(highPitchChord)
console.log(lowPitchChord)
```

Make microtonal chords:
```javascript
let microtonalChord = randomChord(4, midPitch, true)
console.log(microtonalChord)
```

### Arpeggiator
```javascript
arpeggio([event], [chord], repetitions, direction = "backAndForth")
```
By default, the `arpeggio` function plays an "backAndForth" arpeggio of a
_random chord_, one time:
```JavaScript
arpeggio().play()
```

You can pass an event to define note parameters, but it still plays a random
chord:
```javascript
let e = [C3, .5, 0, .1, xylophone]
arpeggio(e).play()
```

Define the chord by passing it as the second parameter:
```javascript
let e = [C3, .5, 0, .1, xylophone]
let cmaj7 = [C4, E4, G4, B4]
arpeggio(e, cmaj7).play()
```

Define a different number of repetitions:
```javascript
let e = [C3, .5, 0, .1, xylophone]
let cmaj7 = [C4, E4, G4, B4]
arpeggio(e, cmaj7, 10).play()
```

Change the direction ("forward", "backward", or "backAndForth"):
```javascript
let e = [C3, .5, 0, .1, xylophone]
let cmaj7 = [C4, E4, G4, B4]
arpeggio(e, cmaj7, 10, "up").play()
```

### Interval sequence
```JavaScript
intervalSequence([event], interval, repetitions, direction)
```

A different type of arpeggio can be constructed using an interval sequence.

By default, it starts on a random note and goes up or down four notes,
following a random interval:
```javascript
intervalSequence().play()
```

Create an event and use it as the basis for the function:
```javascript
let e = [E2, .8, 0, .5, guitar]
intervalSequence(e).play()
```

Select a specific interval for the sequence:
```javascript
let e = [E2, .8, 0, .5, guitar]
intervalSequence(e, 3).play()
```

Change the number of times it will transpose:
```javascript
let e = [E2, .8, 0, .5, guitar]
intervalSequence(e, 3, 10).play()
```

Make it only go up:
```javascript
let e = [E2, .8, 0, .1, piano]
intervalSequence(e, 3, 10, "up").play()
```

Force it to go down:
```javascript
let e = [E6, .8, 0, .1, piano]
intervalSequence(e, 3, 10, "down").play()
```

### Pitch inversion
```JavaScript
invert([melody], axis)
```

Invert an array of pitches around an axis:
```javascript
let melody = [C4, D4, E4, F4]
let inverted = invert(melody, C4)
console.log(inverted) // returns ["D4", "E4", "Bb4","Ab4","G4"]
```

## Clock & timing
### Get current audio clock time
```JavaScript
console.log(audioClock())
```

### Set/get beats per minute
```JavaScript
setBpm(value)
console.log(getBpm())
```

### Timing 
```JavaScript
secs(beats)
// return duration of a beat in seconds

beats(seconds)
// return number of beats from an interval in seconds
```

### Tempo variation (accelerando & rallentando)
```JavaScript
faster([event], steps, ratio)
slower([event], steps, ratio)
```

Generate an accelerando with `faster` with the `ratio` set to less than 1:
```javascript
let e = [membranophone, .1, 0, 1, drums2]
faster(e, 20, .9).play()
```

Generate a rallentando with the `ratio` set to more than 1:
```javascript
let e = [idiophone, .1, 0, .1, drums4]
slower(e, 20, 1.1).play()
```

### Ostinato
```javascript
ostinato([event], repetitions, [rhythm])
```

To quickly create an ostinato from an event, pass it to the `ostinato` method:
```javascript
ostinato([snare, loud, 0, 1, drums4], 10).play()
```

Alternatively, repeat more complex rhythms by passing an array of durations to the third parameter:
```javascript
ostinato([snare, loud, 0, 1, drums5], 10, [1, .5, .5, .25, .25, .25, .25]).play()
```

### Euclidean rhythms
```javascript
euclidean([event], repetitions, steps, hits)
```

It is possible to generate _euclidean rhythms_ with this dedicated function,
which balances a set number of active musical hits across a total number of
pulses _as evenly as possible_. This distribution creates rhythmic patterns
common in traditional music across the globe ([more
info](https://archive.bridgesmathart.org/2005/bridges2005-47.pdf)).

For example, to generate a Tresillo rhythm (3 hits over 8 pulses), that repeats
4 times:
```javascript
euclidean({what: snare, howLong: .2, onSomething: drums}, 4, 8, 3).play()
// the output is [1, 0, 0, 1, 0, 0, 1, 0], or 3+3+2
```

This rhythm can be rotated with the (optional) last parameter:
```javascript
euclidean({what: snare, howLong: .2, onSomething: drums}, 4, 8, 3, 1).play()
// now the output is [0, 1, 0, 0, 1, 0, 0, 1]
```

!!! warning
    The number of hits (fourth parameter) has to be less than the number of pulses
    (third parameter). If it isn't, it will throw an error on the console!

## Dynamics
### Crescendo and decrescendo
```JavaScript
louder([event], lastAmp, steps)
softer([event], lastAmp, steps)
```
Similar to the accelerando/rallentando functions.

Do a crescendo with a soft event going, for example, to maximum amplitude (1):
```javascript
let a = [C4, .1, 0, 1, piano]
louder(a, 1, 10).play()
```

Do a decrescendo with the opposite:
```javascript
let a = [C4, 1, 0, 1, piano]
softer(a, .1, 10).play()
```

## List manipulation
### Retrograde
```JavaScript
retrograde([list])
```

```JavaScript
let myList = [1, 2, 3, 4]
let retro = retrograde(myList)
console.log(retro) // returns [4, 3, 2, 1]
```

### Rotation
```JavaScript
rotate([list], steps)
```

```JavaScript
let melody = [1, 2, 3, 4]
let rotated = rotate(melody, 2)
console.log(rotated) // returns [3,4,1,2]
```

### Blend 
```JavaScript
blend([listA], [listB])
```

```javascript
let listA = [1,2,3]
let listB = [4,5,6,7,8]
console.log(blend(listA,listB)) // returns [1, 4, 2, 5, 3, 6, 1, 7, 2, 8]
```

