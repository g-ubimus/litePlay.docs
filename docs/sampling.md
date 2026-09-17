# Sampling
You can work with audio samples, you can use the "ADD SAMPLE" button on
[litePlay.js editor](https://g-ubimus.github.io/litePlay.js/).

For example, if we load the file "pianoc2.wav", available in litePlay.js
repository, it becomes an object accessible with the same name minus the 
extension:

```javascript
pianoc2.play()
```

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
