# Generators
An option available to make events more dynamic is to use special tokens to
define its parameters. In general, these generators define a particular scope
from which random values will be drawn.

## Pitch 
Pitch can be randomized in three ranges:

```
lowPitch = pitch between C0 and C3
midPitch = pitch between C3 and C5
highPitch = pitch between C5 and C7
```

For example:

```
piano.play([highPitch, 1, 0, 1])
```

## Loudness
Generators for loudness can be accessed from three tokens:

```
soft = amplitude between 0.01 and 0.1
midlevel = amplitude between 0.1 and 0.4
loud = amplitude between 0.4 and 0.9
```

An example, in context:

```
drums2.play([vibraslap, loud, 0, 1])
```

## Start time 
Generators for scheduling the start time in the future can be accessed from
three tokens:

```
now = start between 0.01 and 0.5 seconds
soon = start between 0.5 and 4 secons
later = start between 4 and 8 seconds
```

For example:

```
piano.play([Bb4, 0.5, later, 1])
```

## Durations
Durations can be randomized with these three tokens:

```
shortDur = durations between 0.05 and 0.2 seconds
midDur = durations between 0.2 and 2 secons
longDur = durations between 2 and 5 seconds
```

For example:

```
trombone.play([Eb2, 1, 0, longDur])
```

## Drums family
Two generators can be used to get different types of drums for the _what_ attribute:

```
membranophone = instruments that have a vibrating membrane 
idiophone = everything else
```

For example:

```
drums.play(membranophone)
drums.play([idiophone, .5, later, 1])
```

## Instrument groups
Additionally, random instruments that belong to a same group can be called in
the last attribute of `play()`:

```
onStruck
onSus
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

```
play([midPitch, soft, now, shortDur, onPerc])
```

Warning: notice that _sustained_ sounds will play forever if not called with a especified duration!

```
play(onSus)
```

Finally, any instrument can be randomly called with:

```
onSomething
```
