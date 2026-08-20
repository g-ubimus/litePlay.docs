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
let notes = [C4, D4, E4, F4, G4]
let original = eventList.create()
let when = 0
for (let i = 0; i < notes.length; i++) {
  original.add([notes[i], midLevel, i])
  when = i
}
original.play()

let semitones = 6
let transposedNotes = transpose(notes, semitones)
let transposed = eventList.create()
for (let i = 0; i < transposedNotes.length; i++) {
  transposed.add([transposedNotes[i], midLevel, i+when+1])
}
transposed.play()
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
let transposedScale = transpose(scale, A4)
let list = eventList.create()
for (let i = 0; i < transposedScale.length; i++) {
  list.add([transposedScale[i], loud, i*.2, .2, guitar])
}
list.play()
```

### Just intonation
```javascript
justIntonation(base pitch, number of harmonics)
```

In a similar fashion, the `justIntonation()` function creates a scale by
fitting the pitch classes found on the harmonic series of a pitch inside an
octave.

By default, it returns the scale constructed from the first thirteen harmonics
on C4, resembling the [acoustic scale](https://en.wikipedia.org/wiki/Acoustic_scale),
or the same as:

```javascript
let inTune = justIntonation(C4, 13)
let list = eventList.create()
for (let i = 0; i < inTune.length; i++) {
  list.add([inTune[i], loud, i*.3, .3, clarinet])
}
list.play()
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
let r = randomChord()
console.log(r)
let list = eventList.create()
for (let i = 0; i < r.length; i++) {
  list.add([r[i], loud, i*.1, .1, piano])
}
list.play()
```

Change the size of the chord with the first parameter:
```javascript
let fiveNoteChord = randomChord(5)
let list = eventList.create()
for (let i = 0; i < fiveNoteChord.length; i++) {
  list.add([fiveNoteChord[i], loud, i*.1, .1, piano])
}
list.play()
```

Change the range with the second parameter, using litePlay's pitch generators:
```javascript
let highPitchChord = randomChord(4, highPitch)
let lowPitchChord = randomChord(4, lowPitch)
```

Make microtonal chords:
```javascript
let microtonalChord = randomChord(4, midPitch, true)
let list = eventList.create()
for (let i = 0; i < microtonalChord.length; i++) {
  list.add([microtonalChord[i], loud, i*.1, .1, piano])
}
list.play()
```

### Block chord
```javascript
blockChord([event], [chord])
```

Generates an event list playing all notes of a chord simultaneously.

```javascript
let e = [C4, loud, 0, 1, piano]
let cmaj7 = [C4, E4, G4, B4]
blockChord(e, cmaj7).play()
```

If no chord is passed, it outputs a _random chord_ with four pitches:

```javascript
blockChord().play()
```

You can make this chord microtonal with:

```javascript
blockChord(any, randomChord({microtonal:true})).play()
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
let original = [C4, D4, E4, F4]
arpeggio(any, {chord: original, direction:"forward"}).play()
let inverted = invert(original, C4)
arpeggio(any, {chord: inverted, direction:"forward"}).play()
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
faster([event], lastDuration, steps)
slower([event], lastDuration, steps)
```

Generate an accelerando with `faster` by setting the last duration shorter than the first:
```javascript
let e = [membranophone, loud, 0, 1, drums2]
faster(e, .1, 9).play()
```

Generate a rallentando with the last duration faster than the first:
```javascript
let e = [idiophone, .1, 0, .1, drums4]
slower(e, 2, 20).play()
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
    (third parameter). If it isn't, it throws a `RangeError`.

### Rotation sequence
```javascript
rotationSequence([event], [rhythm])
```

Generates an event list by repeating a rhythmic pattern while rotating its elements each iteration.

```javascript
let e = [C4, loud, 0, 1, synth]
let rhythm = [0.5, 0.25, 0.25]
rotationSequence(e, rhythm).play()
```


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

Return the elements of a list in inverse order.

```JavaScript
let myList = [1, 2, 3, 4]
let retro = retrograde(myList)
```

A musical example playing a melody in reverse:

```javascript
let melody = [C4, D4, E4, F4, G4]
let retro = retrograde(melody)
let list = eventList.create()
for (let i = 0; i < retro.length; i++) {
  list.add([retro[i], loud, i * 0.5, 0.5, piano])
}
list.play()
```

### Rotation
```JavaScript
rotate([list], steps)
```

Rotate the elements of a list according to a number of steps.

```JavaScript
let melody = [1, 2, 3, 4]
let rotated = rotate(melody, 2)
console.log(rotated) // returns [3,4,1,2]
```

A musical example playing a rotated melody:

```javascript
let melody = [C4, D4, E4, F4, G4]
let rotated = rotate(melody, 2)
let list = eventList.create()
for (let i = 0; i < rotated.length; i++) {
  list.add([rotated[i], loud, i * 0.5, 0.5, piano])
}
list.play()
```

### Blend 
```JavaScript
blend([listA], [listB])
```

Mix elements of a list into one, element by element. If one of the lists is
longer than the other, it starts to repeat elements of the shorter.

```javascript
let listA = [1,2,3]
let listB = [4,5,6,7,8]
console.log(blend(listA,listB)) // returns [1, 4, 2, 5, 3, 6, 1, 7, 2, 8]
```

A musical example interleaving bass notes with a melody:

```javascript
let bassNotes = [C2, G2, D2]
let melodyNotes = [C4, E4, G4, B4, D5]
let blended = blend(bassNotes, melodyNotes)
let list = eventList.create()
for (let i = 0; i < blended.length; i++) {
  list.add([blended[i], loud, i * 0.5, 0.5, piano])
}
list.play()
```

### Shuffle
```JavaScript
shuffle([list])
```

Randomizes the order of elements in an array.

```JavaScript
let melody = [1, 2, 3, 4]
let shuffled = shuffle(melody)
console.log(shuffled)
```

A musical example playing a shuffled scale:

```javascript
let melody = [C4, D4, E4, F4, G4, A4, B4]
let shuffled = shuffle(melody)
let list = eventList.create()
for (let i = 0; i < shuffled.length; i++) {
  list.add([shuffled[i], loud, i * 0.5, 0.5, piano])
}
list.play()
```

