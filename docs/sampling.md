# Sampling
You can work with audio samples, you can use the "ADD SAMPLE" button on
[litePlay.js editor](https://g-ubimus.github.io/litePlay.js/). Start the engine
first (click anywhere in the editor), then choose an audio file.

For example, if we load the file "pianoc2.wav", available in litePlay.js
repository, it becomes an object accessible with the same name minus the
extension:

```javascript
pianoc2.play()
```

!!! note
    The editor builds the variable name from the part of the filename before
    the first dot, replacing every character that is not a letter or digit
    with `_`. For example, `my-file.wav` becomes `my_file`; the editor also
    prints the generated name in the console.

But we can rename it however we want by assigning a variable to it:

```javascript
let a = pianoc2
```

We can play it in another pitch by passing it as a parameter to `play()`:

```javascript
a.play(highPitch)
```

And, of course, use the other parameters: 

```javascript
a.play([highPitch, loud, soon, longDur])
```

## Loading samples in code

The `sample` object can also load files without the editor button:

```javascript
let s = sample.create("assets/audio/pianoc2.wav");
silently(500).then(() => s.play());
```

!!! note
    `sample.load()` fetches and decodes the file asynchronously. Wait for it
    before playing; `silently(ms)` is a convenient promise-based timer.

`sample.create(what, fo, bpm)` creates a sample object and optionally loads
`what`. `fo` is the reference pitch of the file (default 60) and `bpm` is the
tempo the file was recorded at (0 means no tempo sync). `sample.load(url, fo,
bpm)` loads a file into an existing sample, and `sample.instrument()` returns
its `Sampler`:

```javascript
let s = sample.create();
s.load("assets/audio/pianoc2.wav", 60, 120);
s.instrument().reverb(.4);
silently(500).then(() => s.play());
```

When `bpm` is greater than 0, `Sampler.play()` follows the current litePlay BPM
(`setBpm()`); this keeps loops and recorded samples in time. `sample.stop()`
stops the sample, and `sample.speed(value)` changes its playback speed.

A sample object can be passed directly as the instrument of an event or a
sequencer track:

```javascript
play([C4, 1, 0, 2, s]);
sequencer.add(s, [C4, O, E4, O], .8, .5);
```

## Changing playback speed
To change playback speed, we can do:
```javascript
a.speed(10)
a.play()
```

Values above 1 will play it faster, while values below 1 will play it slowly. 
Combining it with other methods we can create interesting textures:

```javascript
for (let i = 0; i < 1000; i++) {
  a.speed(.1)
  a.instr.ampEnvelope(2,0,0,1)
  a.play([hiPitch, loud, i*.1])  
}
```

## Looping samples
We can loop a sound sample in defined boundaries in seconds:
```javascript
a.loop(1,2)
a.play()
```
