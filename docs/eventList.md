# eventList 
Lists of events can be made into an object that can be manipulated. 

## eventList.create()
Is used to create an event, usually assigned to a variable:

```javascript
let sequence = eventList.create([C3, 0.1, 0], [C4, 0.2, 1], [C5, 0.4, 2]);
```

then we can play it repeatedly (by calling `play()` on it):

```javascript
sequence.play();
```

`create()`, `add()` and `insert()` also accept object events, with the same
keys used by `play()`:

```javascript
let sequence = eventList.create(
  { what: C3, howLoud: 0.1, when: 0, howLong: 1 },
  { what: E4, howLoud: 0.2, when: 1, howLong: 1 },
);
```

Portuguese keys (`oQue`, `quãoForte`, `quando`, `quãoLongo`, `noQue`) work too.

The stored events are available in the `events` array. For lower-level use,
`score(when, events)` returns an object with the Csound score text and a
`play()` method, without changing the stored events.

```javascript
console.log(sequence.events.length);
sequence.score(0).play();
```

The `play()` can take two optional parameters,

```javascript
eventList.play(when, events);
```

the first one is a _when_ for the complete list, taken from the time of the
action, so we can place the performance in the future. The second is a list of
events, basically a JS list of lists, such as:

```javascript
[[C3, 0.1, 0, 1], [C4, 0.2, 1, 1], [C5, 0.4, 2, 1]]
```

as can be seen, there is an outer list which holds three events, which are
themselves lists (of attributes). One fundamental aspect is that the event list
passed to `play()` replaces the existing events (if any have been added) in the
object.

## eventList.add()

```javascript
eventList.add(event, ...)
```

events to the end of the list. This can take any number of events (as
`create()` did).

## eventList.remove()

```javascript
eventList.remove(index)
```

remove an event with a given `index` from the list, or the last event (if no
`index` is given).

## eventList.insert()

```javascript
eventList.insert(pos, event, ...)
```

insert one or more events into the object at position `pos`.

```javascript
sequence.add(event, [C4,0.9,3]);    
sequence.remove(0);    
sequence.insert(1, [D4, 0.8, 0.5]);
```

## eventList.repeat()

We can also repeat an `eventList` any number of `times`, `when` beats later
from the action,

```javascript
sequence.repeat(times, when);
```

Both `eventList.play()` and `eventList.repeat()` return the end time of the
playback. So we can use that information to schedule other events. For example,
this code plays an event list, adds two events to it and then schedule to
repeat that longer sequence for three times after the end of the first `play()`
action,

```javascript
  let sequence = eventList.create([C4, 0.1, 0, 1],
                                [E4, 0.2, 1, 1], 
                                [G4, 0.4, 2, 1]);
  let end = sequence.play();
  sequence.add([C1, 0.1, 3, 3], [C1, 0.1, 0, 4]);
  sequence.repeat(3, end);
```

One consequence of all these ideas is that we have introduced the idea that a
`play()` action may have different forms, depending on which context it is
being invoked, which at the moment can be

- The global litePlay.js context: `play()`

- An instrument object : `organ.play()`

- An _eventList_: `eventList.play()`

With events and eventLists we can construct compositions and performances with
precise relative timing between events, which is something desirable in musical
activities.

## A groove in A minor

An `eventList` can be scheduled in the future by passing a _when_ value (in
beats) to `play()`. This allows us to layer one-shot phrases on top of a
looping `sequencer`. In the example below, a drum/bass/piano loop starts at
beat 0, a flute melody enters at beat 8 and a harp arpeggio at beat 16:

```javascript
function aMinorGroove() {
  sequencer.clear();
  setBpm(120);
  let grid = 1;

  sequencer.add(drums, [
    sub(crash, O, O, O), sub(O, O, snare, O),
    sub(kick, O, O, O),  sub(O, O, snare, O),
    sub(kick, O, kick, O), sub(O, O, snare, O),
    sub(kick, O, O, O),  sub(O, O, snare, O),
  ], 1, grid);

  sequencer.add(drums, [sub(closedHiHat, closedHiHat, closedHiHat, closedHiHat)], 0.35, grid);

  sequencer.add(bass, [
    sub(A2, O, A2, O), sub(F2, O, F2, O),
    sub(C3, O, C3, O), sub(G2, O, G2, O),
    sub(A2, O, A2, O), sub(F2, O, F2, O),
    sub(C3, O, C3, O), sub(G2, O, G2, O),
  ], 0.8, grid);

  sequencer.add(piano, [
    [[C4],[E4],[G4]], [[C4],[E4],[G4]],
    [[A3],[C4],[E4]], [[A3],[C4],[E4]],
    [[F3],[A3],[C4]], [[F3],[A3],[C4]],
    [[G3],[B3],[D4]], [[G3],[B3],[D4]],
  ], 0.5, grid);

  sequencer.play();

  let melody = eventList.create(
    [E5, 0.7, 0, 0.5, flute],
    [G5, 0.7, 0.5, 0.5, flute],
    [A5, 0.7, 1, 1, flute],
    [G5, 0.7, 2, 0.5, flute],
    [E5, 0.7, 2.5, 1, flute],
    [D5, 0.7, 3.5, 0.5, flute],
    [C5, 0.7, 4, 2, flute],
  );
  melody.play(8);

  arpeggio({ howLoud: 0.6, howLong: 0.25, onSomething: harp },
           { noteList: [C5, E5, G5, A5], repetitions: 2 }).play(16);
}
aMinorGroove();
```

The _when_ passed to `play()` is added to the `when` of every event in the
list, so the melody events above actually sound at the following beats:

| event | `when` | sounds at beat |
| --- | --- | --- |
| E5 | 0 | 8 |
| G5 | 0.5 | 8.5 |
| A5 | 1 | 9 |
| G5 | 2 | 10 |
| E5 | 2.5 | 10.5 |
| D5 | 3.5 | 11.5 |
| C5 | 4 | 12 |

In the same way, `arpeggio()` returns an `eventList`: with its default
_backAndForth_ direction it plays C5, E5, G5, A5, G5, E5 in each repetition,
spacing the notes by _howLong_ (0.25 beats). Two repetitions give 12 notes,
spanning beats 16 to 19. All these phrases stay in sync with the `sequencer`
because every timing value is in beats, converted to seconds by the same BPM
(`secs(b) = b * 60 / BPM`).

## Portuguese interface

The object is exported as `listaEventos`. Its methods have Portuguese aliases:
`toque`, `criar`, `repetir`, `limpar`, `inserir`, `adicionar` and `remover`.

```javascript
let lista = listaEventos.criar([C4, 0.1, 0, 1], [E4, 0.2, 1, 1]);
lista.toque();
lista.adicionar([G4, 0.4, 2, 1]);
```
