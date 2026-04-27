# Instruments

Sound generation is managed by instrument objects. In general, we can access
these by using one of the names defined in the JS module, like `piano`,
`organ`, `synth`, etc. These are constants provided by litePlay.js, allowing an
easy selection of sound generators (to fulfill the _onSomething_ attribute of
an event).

This is the complete set of instruments available in litePlay. There are 128
different instruments plus six special drum kits that can be accessed via their 
name or their individual numbers.

## Struck
Pitched sounds create by striking something like a string, bar, etc:

```javascript
grandPiano
piano = grandPiano
brightPiano
electricGrand
honkyPiano
electricPiano
electricPiano2
harpsichord
clavinet
celesta
glockenspiel
musicBox
vibraphone
marimba
xylophone
tubularBells
dulcimer
tinkleBell
```

## Sustained
Pitched sounds created by sustaining tones:

```javascript
drawbarOrgan
percussiveOrgan
rockOrgan
organ = rockOrgan
churchOrgan
reedOrgan
accordion
harmonic
tangoAccordion
```

## Plucked
Pitched sounds created by plucking strings:

```javascript
nylonAcousticGuitar
guitar = nylonAcousticGuitar
steelAcousticGuitar
jazzElectricGuitar
clearElectricGuitar
mutedElectricGuitar
overdrivenGuitar
distortionGuitar
guitarHarmonics
sitar
banjo
shamisen
koto
kalimba
pizzicatoStrings
orchestralHarp
harp = orchestralHarp
```

## Bass
Various types of pitched sounds for the bass range:

```javascript
acousticBass
fingerElectricBass
pickElectricBass
fretlessBass
bass = fretlessBass
slapBass1
slapBass2
synthBass1
synthBass2
```

## Bowed
Pitched sounds created by bowing strings:

```javascript
 violin
 viola
 cello
 contrabass
 tremoloStrings
```

## Ensemble
Ensemble-like pitched sounds:

```javascript
 stringEnsemble1
 strings = stringEnsemble1
 stringEnsemble2
 synthStrings1
 synthStrings2
```

## Voice
Pitched sounds created through singing:

```javascript
 choirAahs
 voiceOohs
 synthVoice
```

## Blow
Pitched sounds created by blowing on a open mouthpiece connected to a pipe:

```javascript
 trumpet
 trombone
 tuba
 mutedTrumpet
 frenchHorn
 horn = frenchHorn
 brassSection
 brass = brassSection
 synthBrass1
 synthBrass2
```

## Wind
Pitched sounds created by blowing on different types of reeds:

```javascript
sopranoSax
altoSax
tenorSax
baritoneSax
oboe
englishHorn
bassoon
clarinet
piccolo
flute
recorder
panFlute
blownBottle
shakuhachi
whistle
ocarina
```

## Lead 
Lead-type pitched synthesizer sounds:

```javascript
lead1
lead2
lead3
lead4
lead5
lead6
lead7
lead8
```
## Synth
Generic pitched synthesizer sounds:

```javascript
pad1
pad2
pad3
pad4
pad5
synth = pad5
pad6
pad7
pad8
```

## Effects
Mostly dystonic effects sounds:

```javascript
fx1
fx2
fx3
fx4
fx5
fx6
fx7
fx8
```
## Percussion
Mostly dystonic percussion sounds:

```javascript
agogo
steelDrums
woodblock
taikoDrum
melodicTom
synthDrum
reverseCymbal
guitarFretNoise
breathNoise
seaShore
birdTweet
telephoneRing
helicopter
applause
gunshot
```

## Drums
Unpitched drum/percussion kits, with specific sounds that can be selected
individually. For these, the instrument names are `drums`, `drums1`,`drums2`,
`drums3`,  `drums4`, `drums5`, and `drums6`. The sounds can be selected via a
_what_ attribute, for example:

```javascript
drums.play(kick)
```

The complete list of drums sounds is as follows:

```javascript
djScratch = 29
acousticBassDrum = 35
kick = acousticBassDrum
bassDrum1 = 36
sideStick = 37
acousticSnare = 38
handClap = 39
electricSnare = 40
snare = electricSnare
lowFloorTom = 41
closedHiHat = 42
highFloorTom = 43
pedalHiHat = 44
lowTom = 45
tom = lowTom
openHiHat = 46
lowMidTom = 47
hiMidTom = 48
crashCymbal = 49
crash = crashCymbal
hiTom = 50
rideCymbal1 = 51
cymbal = rideCymbal1
chineseCymbal = 52
rideBell = 53
tambourine = 54
splashCymbal = 55
cowbell = 56
crashCymbal2 = 57
vibraslap = 58
rideCymbal2 = 59
hiBongo = 60
lowBongo = 61
muteHiConga = 62
openHiConga = 63
lowConga = 64
hiTimbale= 65
lowTimbale = 66
hiAgogo = 67
lowAgogo = 68
cabasa = 69
maracas = 70
shortWhistle = 71
longWhistle = 72
shortGuiro = 73
longGuiro = 74
claves = 75
hiWoodBlock= 76
lowWoodBlock = 77
muteCuica = 78
openCuica = 79
muteTriangle = 80
openTriangle = 81
```
