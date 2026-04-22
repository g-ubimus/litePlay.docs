# Sequenciador

A classe `sequencer` atua como uma grade de loop automático, lendo dados de
arrays passados para ela e agendando-os no tempo de acordo com uma função de _batidas por
minuto_ (BPM), que diz ao sequenciador quanto tempo cada item no
array ocupa.

## Tempo
Antes de iniciar um sequenciador, precisamos definir um BPM para rodar:

```javascript
setBpm(120);
```

Agora podemos definir grades (grids) para estabelecer diferentes subdivisões. A lógica é a seguinte:

```text
1: Cada item é uma semínima (1 tempo).
0.5: Cada item é uma colcheia (1/2 tempo).
0.25: Cada item é uma semicolcheia (1/4 de tempo).
1/3: Cada item é uma tercina.
etc.
```

Considerando isso, vamos definir algumas subdivisões diferentes:

```javascript
const grid4 = 1;     // Semínimas
const grid8 = 0.5;   // Colcheias
const grid16 = 0.25; // Semicolcheias
```

## Padrões

No núcleo do sequenciador, temos padrões que serão repetidos na
subdivisão de grade definida.

Dado isso, vamos criá-los:

```javascript
let backbeatPat = [kick, snare, kick, snare];
let bassPat = [[C3, 1, 0, 0.2], [C3, 1, 0, 0.2], [C4, 1, 0, 0.2], O, [Eb3, 1, 0, 0.2], O, [G3, 1, 0, 0.2], O];
let hatPat = [[closedHiHat, .2], [closedHiHat, 0.5], [openHiHat, .2], [closedHiHat, 0.5]];
let chordPat = [
      [[C4, 1, 0, 3.75], [Eb4, 1, 0, 3.75], [G4, 1, 0, 3.75], [Bb4, 1, 0, 3.75]], // Cm7
      O, O, O,
      [[Ab3, 1, 0, 3.75], [C4, 1, 0, 3.75], [Eb4, 1, 0, 3.75], [G4, 1, 0, 3.75]], // Abmaj7
      O, O, O,
  ];
```

!!! note "Nota"
    Note que estamos usando o O maiúsculo para eventos silenciosos no baixo.

    Também note que para fazer as notas tocarem na mesma parte no padrão de acordes,
    usamos colchetes aninhados.

### Subdivisões locais
O método especial `sub()` nos permite ter subdivisões em um padrão sem
ter que reescrevê-lo para uma nova grade. Por exemplo:

```javascript
let backbeatPat = [kick, snare, kick, sub(snare, snare)];
```

Dará uma colcheia para as duas últimas caixas (snares), ocupando o último tempo.
Qualquer número de subdivisões é possível, assim como silêncios (`O`) dentro de uma
subdivisão.

!!! warning "Aviso"
    Subdivisões aninhadas (ex. `sub(snare, sub(snare, snare), snare)`) ainda não estão
    implementadas e retornarão um evento silencioso. Para este caso, recomendamos 
    descer mais na grade de subdivisão para ter mais controle de cada batida.

## Faixas

O sequenciador pode ter múltiplas faixas (tracks) tocando ao mesmo tempo, cada uma com
durações diferentes e tocando em subdivisões diferentes, mas todas sincronizadas com
a mesma configuração de BPM.

Os parâmetros para uma nova faixa do sequenciador são os seguintes:

```javascript
sequencer.add(instrumento, padrao, amplitude, grade);
```

Assim, podemos continuar e adicionar nossos instrumentos:

```javascript
let backbeatTrack = sequencer.add(bateria, kckPat, 0.8, grid16);
let hatTrack = sequencer.add(bateria, hatPat, 0.3, grid16);
let bassTrack = sequencer.add(baixoSintetizado1, bassPat, 0.6, grid8);
let padTrack = sequencer.add(pad1, chordPat, 0.4, grid4);
```

Podemos colocar tudo dentro de uma função:

```javascript
function synthWave() {

  sequencer.clear();
  setBpm(120);

  const grid4 = 1;     
  const grid8 = 0.5;   
  const grid16 = 0.25; 

  let backbeatPat = [kick, snare, kick, snare];
  let bassPat = [[C3, 1, 0, 0.2], [C3, 1, 0, 0.2], [C4, 1, 0, 0.2], O, [Eb3, 1, 0, 0.2], O, [G3, 1, 0, 0.2], O];
  let hatPat = [[closedHiHat, .2], [closedHiHat, 0.5], [openHiHat, .2], [closedHiHat, 0.5]];
  let chordPat = [
        [[C4, 1, 0, 3.75], [Eb4, 1, 0, 3.75], [G4, 1, 0, 3.75], [Bb4, 1, 0, 3.75]], // Cm7
        O, O, O,
        [[Ab3, 1, 0, 3.75], [C4, 1, 0, 3.75], [Eb4, 1, 0, 3.75], [G4, 1, 0, 3.75]], // Abmaj7
        O, O, O,
    ];
  let backbeatTrack = sequencer.add(bateria, backbeatPat, 0.8, grid4);
  let hatTrack = sequencer.add(bateria, hatPat, 0.3, grid16);
  let bassTrack = sequencer.add(baixoSintetizado1, bassPat, 0.6, grid8);
  let padTrack = sequencer.add(pad1, chordPat, 0.4, grid4);

  sequencer.play();
}

synthWave();
```

!!! note "Nota"
    Usamos `sequencer.clear()` no início da nossa função para podermos
    modificar seus padrões e chamá-la instantaneamente de novo!

## Padrões dinâmicos
Um problema com o exemplo acima é que ele sempre se repetirá da mesma
maneira, sem variações, o que não soa muito musical...
Para corrigir isso, podemos envolver uma função `escolha()` dentro de um padrão, fazendo-a 
escolher entre algumas opções a cada vez.

```javascript
let Cm7 = [[C4, 1, 0, 3.75], [Eb4, 1, 0, 3.75], [G4, 1, 0, 3.75], [Bb4, 1, 0, 3.75]];
let Abmaj7 = [[Ab3, 1, 0, 3.75], [C4, 1, 0, 3.75], [Eb4, 1, 0, 3.75], [G4, 1, 0, 3.75]];
let Ebmaj7 = [[Eb3, 1, 0, 3.75], [D4, 1, 0, 3.75], [G4, 1, 0, 3.75], [Bb4, 1, 0, 3.75]];
let Fm7 = [[F3, 1, 0, 3.75], [Eb3, 1, 0, 3.75], [C4, 1, 0, 3.75], [Ab4, 1, 0, 3.75]];

let chordPat = [
  () => escolha(Cm7, Ebmaj7), O, O, O, 
  () => escolha(Abmaj7, Fm7), O, O, O
];
```
