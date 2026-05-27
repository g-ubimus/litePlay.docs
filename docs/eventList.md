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

as can be seen, there is an outer lists which holds two events, which are
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

insert one or more events into the object, after position `pos`.

```javascript
sequence.add(event, [C4,0.9,3]);    
sequence.remove(0);    
sequence.insert(1, [D4, 0.8, 0.5]);
```

## eventList.repeat()

We can also repeat an `eventList` any number of `times`, `when` seconds later
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
