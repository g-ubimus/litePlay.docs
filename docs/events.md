# Events

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

