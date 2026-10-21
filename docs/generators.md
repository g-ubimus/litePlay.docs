# Generators
An option available to make events more dynamic is to use special tokens to
define its parameters. In general, these generators define a particular scope
from which random values will be drawn.

## Pitch 
Pitch can be randomized in three ranges:

```javascript
lowPitch = pitch between C0 and C3
midPitch = pitch between C3 and C5
highPitch = pitch between C5 and C7
```

For example:

```javascript
piano.play([highPitch, 1, 0, 1])
```

The aliases `hiPitch`, `grave`, `médio` and `agudo` are also available.

## Loudness
Generators for loudness can be accessed from three tokens:

```javascript
soft = amplitude between 0.1 and 0.5
midlevel = amplitude between 0.5 and 0.8
loud = amplitude between 0.8 and 0.9
```

The longer aliases `softLevel`, `midLevel` and `loudLevel` work too. An example,
in context:

```javascript
drums2.play([vibraslap, loud, 0, 1])
```

## Start time 
Generators for scheduling the start time in the future can be accessed from
three tokens:

```javascript
now = start between 0.01 and 0.5 beats
soon = start between 0.5 and 4 beats
later = start between 4 and 8 beats
```

For example:

```javascript
piano.play([Bb4, 0.5, later, 1])
```

!!! note
    Event timing is measured in beats. At the default 60 BPM, one beat is one
    second; at 120 BPM, one beat is half a second. Use `secs()` and `beats()`
    to convert between the two.

## Durations
Durations can be randomized with these three tokens:

```javascript
shortDur = durations between 0.05 and 0.2 beats
midDur = durations between 0.2 and 2 beats
longDur = durations between 2 and 5 beats
```

For example:

```javascript
trombone.play([Eb2, 1, 0, longDur])
```

The aliases `shortDuration`, `midDuration` and `longDuration` are also
available.

## Drums family
Two generators can be used to get different types of drums for the _what_ attribute:

```javascript
membranophone = instruments that have a vibrating membrane 
idiophone = everything else
```

For example:

```javascript
drums.play(membranophone)
drums.play([idiophone, .5, later, 1])
```

## Instrument groups
Additionally, random instruments that belong to a same group can be called in
the last attribute of `play()`:

```javascript
onStruck
onSus
onSustained
onPluck
onBass
onBowed
onEnsemble
onVoice
onBlow
onWind
onLead
onSynth
onFx
onPerc
onDrums
```

For context, an example combining everything we learned here:

```javascript
play([midPitch, soft, now, shortDur, onPerc])
```

Warning: notice that _sustained_ sounds will play forever if not called with an specified duration!

```javascript
play(onSus)
```

Finally, any instrument can be randomly called with:

```javascript
onSomething
```

## Utility generators
These helpers return random values or options for use inside events:

```javascript
rnd(min, max)              // random float, max exclusive
random(min, max)           // alias of rnd
rndInt(min, max)           // random integer, max exclusive
randomInteger(min, max)    // alias of rndInt
choose(option1, option2, ...) // one option at random
silently(ms)               // promise that resolves after ms milliseconds
```

`choose()` can also take a single array:

```javascript
play(choose(C4, E4, G4));
play(choose([C4, E4, G4]));
```
