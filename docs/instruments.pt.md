# `instrumento()`

A geração de som é gerenciada por objetos de instrumento. Em geral, podemos
acessá-los usando um dos nomes definidos no módulo JS, como `piano`, `órgão`,
`synth`, etc. Estas são constantes fornecidas pelo litePlay.js, permitindo uma
seleção fácil de geradores de som (para preencher o atributo _onSomething_ de
um evento).

Este é o conjunto completo de instrumentos disponíveis no litePlay. Existem 128
instrumentos diferentes, além de seis kits de bateria especiais que podem ser
acessados por seus nomes ou por seus números individuais.

## Teclas
Sons afinados criados ao percutir algo como uma corda, barra, etc:

```
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

## Sustentados
Sons afinados criados através da sustentação de tons:

```
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

## Dedilhados
Sons afinados criados ao dedilhar ou puxar cordas:

```
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

## Baixo
Vários tipos de sons afinados para a região dos graves:

```
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

## Friccionados/Arco
Sons afinados criados ao passar o arco em cordas:

```
violin
viola
cello
contrabass
tremoloStrings
```

## Conjunto
Sons afinados semelhantes aos de um conjunto ou orquestra:

```
stringEnsemble1
strings = stringEnsemble1
stringEnsemble2
synthStrings1
synthStrings2
```

## Voz
Sons afinados criados através do canto:

```
choirAahs
voiceOohs
synthVoice
```

## Metais
Sons afinados criados ao soprar em um bocal aberto conectado a um tubo:

```
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

## Palhetas
Sons afinados criados ao soprar em diferentes tipos de palhetas:

```
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

## Solo
Sons de sintetizador afinados do tipo lead (solo):


```
lead1
lead2
lead3
lead4
lead5
lead6
lead7
lead8
```

## Sintetizador
Sons genéricos afinados de sintetizador:

```
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

## Efeitos
Principalmente sons de efeitos distônicos:

```
fx1
fx2
fx3
fx4
fx5
fx6
fx7
fx8
```

## Percussão
Principalmente sons de percussão distônicos:

```
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

## Bateria
Kits de bateria/percussão sem altura definida, com sons específicos que podem
ser selecionados individualmente. Para estes, os nomes dos instrumentos são
`drums`, `drums1`,`drums2`, `drums3`, `drums4`, `drums5` e `drums6`. Os sons
podem ser selecionados através de um atributo _what_ (o que), por exemplo:

```
drums.play(kick)
```

A lista completa de sons de bateria é a seguinte:

```
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
