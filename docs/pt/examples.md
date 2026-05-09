# Exemplos
Esta página lista exemplos musicais úteis e funções extras disponíveis no
sistema para dar uma ideia de como o litePlay.js funciona.

## Altura & harmonia

### Obter nomes das notas
```JavaScript
midiToName(60)); // returns "C4"
midiToName(31)); // returns "G1"
```

### Afinação
Podemos ter alturas microtonais em diferentes formatos. O mais simples seria
adicionar partes fracionárias a um valor MIDI:

```JavaScript
toque(60.25);
```

No entanto, para que isso funcione, devemos conhecer os valores das alturas!
Uma maneira de fazer isso é imprimi-los no console:

```JavaScript
console.log(C4); // returns 60
```

Uma maneira diferente, que não requer conhecimento específico de MIDI, é tirar
proveito de algumas constantes:

```JavaScript
toque(C4+quartoTom); // equivalente a 60.5
toque(C4+terçoTom); // equivalente a 60.33
toque(C4+oitavoTOm); // equivalente a 60.25
toque(C4+dezCents); // equivalente a 60.1
toque(C4+umCent); // equivalente a 60.01
```

### Transposição
```javascript
transpose(melody, semitones);
```

Transpõe um array de notas em um número de semitons:

```JavaScript
let m = [C4, D4, E4, F4, G4];
let s = 2;
let transposed = transpose(m, s);

console.log(transposed) // returns [D4, E4, Fs4/Gb4, G4, A4]
```

### Acorde aleatório
```javascript
randomChord(size, range, microtonal = false);
```

Gera um acorde (4 notas por padrão, na faixa de `midPitch`) com alturas aleatórias:
```javascript
let a = randomChord();
console.log(a);
```

Altere o tamanho do acorde com o primeiro parâmetro:
```javascript
let fiveNoteChord = randomChord(5);
console.log(fiveNoteChord);
```

Altere o alcance com o segundo parâmetro, usando os geradores de altura do litePlay:
```javascript
let highPitchChord = randomChord(4, highPitch);
let lowPitchChord = randomChord(4, lowPitch);
console.log(highPitchChord);
console.log(lowPitchChord);
```

Crie acordes microtonais:
```javascript
let microtonalChord = randomChord(4, midPitch, true);
console.log(microtonalChord);
```

### Arpejador
```javascript
arpejo([event], [chord], repeats, direction = "upAndDown")
```
Por padrão, a função `arpejo` toca um arpejo "upAndDown" (sobe e desce) de um *acorde aleatório*, uma vez:
```JavaScript
arpejo().toque();
```

Você pode passar um evento para definir os parâmetros da nota, mas ele ainda tocará um acorde aleatório:
```javascript
let e = [C3, .5, 0, .1, xylophone];
arpejo(e).toque();
```

Defina o acorde passando-o como o segundo parâmetro:
```javascript
let e = [C3, .5, 0, .1, xylophone];
let cmaj7 = [C4, E4, G4, B4];
arpejo(e, cmaj7).toque();
```

Defina um número diferente de repetições:
```javascript
let e = [C3, .5, 0, .1, xylophone];
let cmaj7 = [C4, E4, G4, B4];
arpejo(e, cmaj7, 10).toque();
```

Altere a direção ("up" [cima], "down" [baixo] ou "upAndDown" [cima e baixo]):
```javascript
let e = [C3, .5, 0, .1, xylophone];
let cmaj7 = [C4, E4, G4, B4];
arpejo(e, cmaj7, 10, "up").toque();
```

Concatene arpejos!
```javascript
let a = arpejo([C4, .8, 0, .2, cravo], [C4, E4, G4, C5, E5, G4, C5, E5], 2, "up").toque();
let b = arpejo([D4, .8, a, .2, cravo], [C4, D4, A4, D5, F5, A4, D5, F5], 2, "up").toque();
let c = arpejo([G4, .8, b, .2, cravo], [B3, D4, G4, D5, F5, G4, D5, F5], 2, "up").toque();
let d = arpejo([C4, .8, c, .2, cravo], [C4, E4, G4, C5, E5, G4, C5, E5], 2, "up").toque();
```

### Sequência de intervalos
```JavaScript
intervalSequence([event], interval, repetitions, up?)
```

Um tipo diferente de arpejo pode ser construído usando uma sequência de intervalos.

Por padrão, começa em uma nota aleatória e sobe ou desce quatro notas, seguindo um intervalo aleatório:
```javascript
intervalSequence().toque();
```

Crie um evento e use-o como base para a função:
```javascript
let e = [E2, .8, 0, .5, guitar];
intervalSequence(e).toque();
```

Selecione um intervalo específico para a sequência:
```javascript
let e = [E2, .8, 0, .5, guitar];
intervalSequence(e, 3).toque();
```

Altere o número de vezes que ele será transposto:
```javascript
let e = [E2, .8, 0, .5, guitar];
intervalSequence(e, 3, 10).toque();
```

Faça com que apenas suba:
```javascript
let e = [E2, .8, 0, .1, piano];
intervalSequence(e, 3, 10, true).toque();
```

Force-o a descer alterando o último parâmetro para `false`:
```javascript
let e = [E6, .8, 0, .1, piano];
intervalSequence(e, 3, 10, false).toque();
```

### Inversão
```JavaScript
invert(melody, axis)
```

Inverte um array de alturas em torno de um eixo:
```javascript
let melody = [C4, D4, E4, F4];
let inverted = invert(melody, C4);
console.log(inverted); // returns ["C4","As/Bb4","Gs/Ab4","G4"]
```

## Ritmo & tempo
### Definir/obter batidas por minuto
```JavaScript
setBpm(value);
console.log(getBpm());
```

### Conversões
```JavaScript
secs(beats);
// return duration of a beat in seconds

beats(seconds)
// return number of beats from an interval in seconds
```

### Obter o tempo atual do relógio de áudio
```JavaScript
console.log(audioClock())
```
### Variação de andamento (accelerando & rallentando)
```JavaScript
tempoVariation([event], steps, ratio)
```

Gera um accelerando quando a proporção (`ratio`) < 1:
```javascript
let e = [membranophone, .1, 0, 1, drums2];
tempoVariation(e, 20, .9).toque();
```

Ou um rallentando quando a proporção (`ratio`) > 1:
```javascript
let e = [idiophone, .1, 0, .1, drums4];
tempoVariation(e, 20, 1.1).toque();
```

### Amen Break
```javascript
function amenBreak() {
  sequencer.clear();
  setBpm(130);
  let grid = .5;

  let hhPat = [[openHiHat, .3]];
  let kkPat = [kick, kick, O, O, O, sub(kick, kick), O, O, kick, kick, O, O, O, sub(kick, kick), O, O, kick, kick, O, O, O, kick, O, O, O, sub(kick, kick), O, O, O, [kick, 1], O, O];
  let snPat = [O, O, [snare, 1], sub(O, [snare, .5]), sub(O, [snare, .5]), O, [snare, 1], sub(O, [snare, .5]), 
               O, O, [snare, 1], sub(O, [snare, .5]), sub(O, [snare, .5]), O, [snare, 1], sub(O, [snare, .5]),
               O, O, [snare, 1], sub(O, [snare, .5]), sub(O, [snare, .5]), O, O, [snare, .5],
               sub(O, [snare, .5]), O, [snare, 1], sub(O, [snare, .5]), sub(O,  [snare, .5]), O, O, [snare, .5]];

  sequencer.add(drums, hhPat, 1, grid);
  sequencer.add(drums, kkPat, 1, grid);
  sequencer.add(drums, snPat, 1, grid);

  sequencer.toque();
}

amenBreak()
```

## Dinâmica
### Crescendo e decrescendo
```JavaScript
ampVariation([event], lastAmp, steps)
```
Semelhante ao método de accelerando/rallentando.

Faça um crescendo com um evento suave indo, por exemplo, até a amplitude máxima (1):
```javascript
let a = [C4, .1, 0, 1, piano];
ampVariation(a, 1, 10).toque();
```

Faça um decrescendo com o oposto:
```javascript
let a = [C4, 1, 0, 1, piano];
ampVariation(a, .1, 10).toque();
```

## Manipulando efeitos de áudio
### Auto-panning
```javascript
autoPan(instrument, hertz)
```

Podemos usá-lo com um sequenciador:

```javascript
function autechreMontreal() {
  sequencer.clear();
  setBpm(100);

  let grid16 = .5;
  let grid4 = 1;

  let hhPat = [pedalHiHat, sub(pedalHiHat, pedalHiHat), pedalHiHat, sub(pedalHiHat, pedalHiHat), sub(pedalHiHat, pedalHiHat), pedalHiHat];  
  let bassPat = [O, sub(O, [Gb2, 1, 0, 1]), sub(O, [Gb2, 1, 0, 2]), O,
                 [A2, 1, 0, 2], sub(O, [A2, 1, 0, 2]), O, O];
  let kkPat = [kick, sub(O, kick), sub(O, kick), sub(O, [kick, 1, 0, 3], O, O)];
  
  autoPan(drums1, .1);
  synthBass1.reverb(.4);

  sequencer.add(drums1, hhPat, .4, grid16);
  sequencer.add(synthBass1, bassPat, .3, grid4);
  sequencer.add(drums2, kkPat, .9, grid4);

  sequencer.toque();
}

autechreMontreal()
```

## Manipulação de listas
### Retrogradar
```JavaScript
retrograde(list)
```

```JavaScript
let myList = [1, 2, 3, 4];
let retro = retrograde(myList);
console.log(retro) // returns [4, 3, 2, 1]
```

### Rotação
```JavaScript
rotate(list, steps)
```

```JavaScript
let melody = [1, 2, 3, 4];
let rotated = rotate(melody, 2);
console.log(rotated) // returns [3,4,1,2]
```

### Mistura
```JavaScript
tangle(listA, listB)
```

```javascript
let listA = [1,2,3];
let listB = [4,5,6,7,8];
console.log(tangle(listA,listB)); // returns [1, 4, 2, 5, 3, 6, 1, 7, 2, 8]
```
