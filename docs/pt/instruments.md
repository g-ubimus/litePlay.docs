# `instrumento()`

A geração de som é gerenciada por objetos de instrumento. Em geral, podemos
acessá-los usando um dos nomes definidos no módulo JS, como `piano`, `órgão`,
`synth`, etc. Estas são constantes fornecidas pelo litePlay.js, permitindo uma
seleção fácil de geradores de som (para preencher o atributo _onSomething_ de
um evento).

Este é o conjunto de constantes de instrumentos disponíveis no litePlay. Ele
cobre os programas General MIDI, além de seis kits de bateria especiais que
podem ser acessados por seus nomes ou por seus números individuais.

## Teclas
Sons afinados criados ao percutir algo como uma corda, barra, etc:

```javascript
pianoDeCauda
pianoBrilhante
deCaudaElétrico
pianoRachado
pianoElétrico
pianoElétrico2
cravo
clavinete
cravoElétrico
caixaDeMúsica
vibrafone
xilofone
carrilhão
sininho
sino

```

## Sustentados
Sons afinados criados através da sustentação de tons:

```javascript
órgãoElétrico
órgãoPercussivo
órgãoDeRock
órgão
órgãoLitúrgico
órgãoDePalhetas
acordeão
gaita
acordeãoDeTango
```

## Dedilhados
Sons afinados criados ao dedilhar ou puxar cordas:

```javascript
violão
vilãoDeNáilon
violãoDeAço
guitarraDeJazz
guitarraLimpa
guitarraAbafada
guitarraOverdrive
guitarraDistorcida
guitarraHarmônicos
samisém
cordasPizzicato
harpaOrquestral
harpa
```

## Baixo
Vários tipos de sons afinados para a região dos graves:

```javascript
baixoAcústico
baixoElétrico
baixoElétricoPalhetado
baixoFretless
baixo
baixoSlap1
baixoSlap2
baixoSintetizado1
baixoSintetizado2
```

## Friccionados/Arco
Sons afinados criados ao passar o arco em cordas:

```javascript
violino
violoncelo
celo
contrabaixo
cordasTremolo
fiddle
```

## Conjunto
Sons afinados semelhantes aos de um conjunto ou orquestra:

```javascript
cordas
cordas2
cordasSintetizadas1
cordasSintetizadas2
golpeOrquestral
golpe
```

## Voz
Sons afinados criados através do canto:

```javascript
coralAahs
coralOohs
vozSintetizada
```

## Metais
Sons afinados criados ao soprar em um bocal aberto conectado a um tubo:

```javascript
trompete
trompeteAbafado
trompeteComSurdina
trompaFrancesa
trompa
seçãoDeMetais
metais
metalSintetizado1
metalSintetizado2
```

## Palhetas
Sons afinados criados ao soprar em diferentes tipos de palhetas:

```javascript
saxSoprando
saxAlto
saxTenor
saxBarítono
oboé
corneInglês
fagote
clarineta
clarinete
pícolo
flautaTransversal
flauta
flautaDoce
flautaDePã
garrafaSoprada
garrafa
assobio
assovio
gaitaDeFole
```

## Solo
Sons de sintetizador afinados do tipo lead (solo):


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

## Sintetizador
Sons genéricos afinados de sintetizador:

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

## Efeitos
Principalmente sons de efeitos distônicos:

```javascript
efeito1
efeito2
efeito3
efeito4
efeito5
efeito6
efeito7
efeito8
```

## Percussão
Principalmente sons de percussão distônicos:

```javascript
agogô
tamborDeAço
blocoDeMadeira
bloco
taiko
tomTom
bateriaSintetizada
pratoReverso
trasteDeGuitarra
traste
respiração
ondaDoMar
pássaro
telefone
helicóptero
aplausos
tiro
tímpano
```

## Bateria
Kits de bateria/percussão sem altura definida, com sons específicos que podem
ser selecionados individualmente. Para estes, os nomes dos instrumentos são
`bateria`, `bateria1`, `bateria2`, `bateria3`, `bateria4`, `bateria5` e
`bateria6`. Os sons
podem ser selecionados através de um atributo _what_ (o que), por exemplo:

```javascript
bateria.toque(kick)
```

A lista completa de sons de bateria é a seguinte:

```javascript
djScratch = 29
acousticBassDrum = 35
kick = acousticBassDrum
bassDrum1 = 36
bassDrum = bassDrum1
sideStick = 37
acousticSnare = 38
handClap = 39
clap = handClap
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
longWhistle = 71
shortWhistle = 72
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
