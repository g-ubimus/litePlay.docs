# `play()`

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

## Events

In litePlay.js we can think of the resulting action of `play()` in this form is
a musical _event_. We can define it with five attributes:

- _what_: the thing that we play, which may vary, but in the case shown
  earlier, is the pitch of the sound.

- _howLoud_: the level of the sound, which can be set by a numeric value in a
  scale from 0 to 1.

- _when_: the event time, when it is supposed to be happening, in the present
  case, set in seconds.

- _howLong_: how long the sound is to last for, in seconds.

- _onSomething_: the thing that will be making the sound, the instrument, such
  as `organ`, `violin` etc. There are several of these to choose from.
  Depending on the type of instrument, _what_ can be played may vary. For
  example, in the case of `drums`, we do not have pitch, but different
  percussion sounds like `snare`, `kick`,  etc.

Event attributes (or parameters) are optional, as we have seen. If we do pass
them, defaults are used. It is possible to pass only a few parameters, for
example just _what_; _what_ and _howLoud_; _what_, _howLoud_, and _when_; as
well as _what_, _howLoud_,_when_, and _howLong_.

Events are passed using a JS list (or array) with attributes in the order
listed earlier:

```
[what,  howLoud, when, howLong, onSomething]
```

For example,

```
play([C4, 0.5, 0, 2, violin])
```

The top-level `play()` action can take several events as arguments, such
as

```
play([C4, 0.1, 0, 3], [E4, 0.2, 0.5, 0.5], [G4, 0.4, 2, 0.1])
```

Now we learned that can we work with lists of events, instead of only sending
individual events to `play()`. There is a particular aspect of this that we
should note, the _when_ attributes of each event will be interpreted in a
certain way.

### A simple list of _what_

```
play(C3, C4, C5);
```

In this case, the events will be separated by the default _howLong_ for the
_onSomething_ being played (set to 1 sec). So we hear the sounds in sequence.

### A list of incomplete events

```
play([C3], [C4], [C5]);
```

In this case, the default for _when_ is 0, immediately, for all events in the
list. We hear the sounds starting at the same time, mixed up.

### A list of events with _when_ attributes explicitly defined

```
play([C3, 0.1, 0], [C4, 0.2, 1], [C5, 0.4, 2]);
```

In this case, the timing of events is relative to the time we asked for the
event list to be played, with perhaps a very short delay. All events are
precisely timed in relation to that. We can decide when these should come in
with an exact time.

What if a list of events become an [object](./eventList.md)?
