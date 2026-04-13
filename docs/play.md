#`play()`

At the core of litePlay.js we have `play()`. This can be simply run as

```
play();
```

to play a sound, and `lp.stop()` to stop it. These code lines can be typed directly on an interactive REPL console or added to the script.

We can make changes to this sound by setting
the default instrument playing it,

```
instrument(organ);
```

the default was set to piano at the start, but we now changed it to
organ. An introduction to instruments is given at the end of the tutorial.

We can also tell what to play. If the sound is from a pitched instrument,
we can ask it to play a given pitch,

```
play(E4);
```

The pitch symbolic names range from Cm1 (=C<sub>-1</sub>) to G8 (=G<sub>8</sub>),
in such a way that the middle C of a piano keyboard is set to C4. The
sound is played immediately and lasts indefinitely (although some
may decay in intensity over time).

Accidentals are represented by lowercase `s` for _sharp_ and `b` for flat _notes_:
```
play(Eb4);
play(Fs4);
```

Got it? Now lets see how to do more complex [events](./events.md).
