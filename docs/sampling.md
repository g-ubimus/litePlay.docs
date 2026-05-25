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
