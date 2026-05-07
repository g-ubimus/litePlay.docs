# Exemplos
 
Esta página lista exemplos musicais úteis para dar uma noção de como o litePlay.js funciona.

## Altura e harmonia
### Afinação
Podemos ter alturas microtonais em diferentes formatos. O jeito mais simples seria
adicionar partes fracionárias a um valor MIDI:

```javascript
toque(60.25);
```

No entanto, para que isso funcione, devemos conhecer os valores das notas! Uma
maneira de fazer isso é imprimi-los no console:

```javascript
console.log(C4); // retorna 60
```

Uma maneira diferente, que não requer conhecimento específico de MIDI, é aproveitar
algumas constantes:

```javascript
toque(C4+quartoTom); // equivalente a 60.5
toque(C4+terçoTom); // equivalente a 60.33
toque(C4+oitavoTOm); // equivalente a 60.25
toque(C4+dezCents); // equivalente a 60.1
toque(C4+umCent); // equivalente a 60.01
```

### Transposição
```javascript
function transpor(melodia, semitons) {
  return melodia.map(nota => nota + semitons);
}

let melodia = [C4, D4, E4, F4, G4];
let transposta = transpor(melodia, 2);
console.log(transposta) // retorna [D4, E4, Fs4, G4, A4]
```

### Arpejador
```javascript
function arpejar(acorde, intervaloTempo, repeticoes, l = eventList.create(), tempo = 0) {
 if (!repeticoes) {
   l.play();
 } else {
   for (let i = 0; i < acorde.length; i++) {
     l.add([acorde[i], 1, tempo, intervaloTempo, xilofone]);
     tempo += intervaloTempo; 
   }
   arpejar(acorde, intervaloTempo, repeticoes - 1, l, tempo);
 }
}
let Cmaj7 = [C4, E4, G4, B4];
arpejar(Cmaj7, 0.15, 5);
```

#### Arpejo sobe-e-desce
```javascript
function sobeDesce(acorde, intervaloTempo, repeticoes, l = eventList.create(), tempo = 0) {
  if (!repeticoes) {
    l.play(); 
  } else {    
    for (let i = 0; i < acorde.length; i++) {
      l.add([acorde[i], 1, tempo, intervaloTempo, xilofone]);
      tempo += intervaloTempo; 
    }    
    for (let i = acorde.length - 2; i > 0; i--) {
      l.add([acorde[i], 1, tempo, intervaloTempo, xilofone]);
      tempo += intervaloTempo; 
    }    
    sobeDesce(acorde, intervaloTempo, repeticoes - 1, l, tempo);
  }
}
let Cmaj7 = [C4, E4, G4, B4];
sobeDesce(Cmaj7, 0.15, 10);
```

### Sequência de intervalos
```javascript
function sequencia(nota, intervalo, duracao, delta, repeticoes) {
      if (!repeticoes) return;
      toque([nota, 1, delta, duracao, vibrafone]);
      sequencia(nota + intervalo, intervalo, duracao, delta + duracao, repeticoes - 1);
}

sequencia(C4, 3, .5, 0.1, 10);
```

### Inversão
```javascript
function inverter(melodia, eixo) {
  return melodia.map(nota => eixo + (((eixo - nota) % 12) + 12) % 12);
}
let melodia = [C4, D4, E4, F4];
let invertida = inverter(melodia, C4);
console.log(invertida); // retorna [C4, Bb4, Ab4, G4]
```

### Obter nomes das notas
```javascript
midiToName(60)); // retorna "C4"
midiToName(31)); // retorna "G1"
```

## Ritmo e tempo
### Variação de tempo (accelerando e rallentando)
```javascript
function variacaoTempo(oQue, quando, duracao, repeticoes, razao) {
      if (!repeticoes) return;
      toque([oQue, .9, quando, duracao, bateria6]);
      let proximaDuracao = duracao * razao;
      variacaoTempo(oQue, quando + duracao, proximaDuracao, repeticoes - 1, razao);
}

variacaoTempo(membranofone, .1, 1, 20, .9); // accelerando
```

Invertendo os valores de início e fim, temos um _rallentando_:

```javascript
variacaoTempo(idiofone, 1, .1, 20, 1.1); // rallentando
```

### Definir/obter batidas por minuto
```javascript
setBpm(value);
console.log(getBpm());
```

### Conversões
```javascript
secs(beats);
// retorna a duração de uma batida em segundos

beats(seconds)
// retorna o número de batidas a partir de um intervalo em segundos
```

### Obter tempo atual do relógio de áudio
```javascript
console.log(audioClock())
```

## Dinâmica
```javascript
function variacaoAmp(primeiro, ultimo, passos, duracao, delta) {
  if (!passos) return;  
  let ampPasso = (ultimo - primeiro) / passos; 
  toque([médio, primeiro, delta, duracao]);
  variacaoAmp(primeiro + ampPasso, ultimo, passos - 1, duracao, delta + duracao);
}

variacaoAmp(.1, 1, 10, 0.2, 0.5); // crescendo
```

Invertendo os valores, obtemos um decrescendo:

```javascript
variacaoAmp(1, .1, 10, 0.2, 0.5); // decrescendo
```

## Manipulação de listas

### Retrogradação
```javascript
function retrogradar(lista) {
  return lista.map((item, index, arr) => {let indiceOposto = arr.length - 1 - index;    
    return arr[indiceOposto];});
}

let minhaLista = [1, 2, 3, 4];
let retro = retrogradar(minhaLista);
console.log(retro) // retorna [4, 3, 2, 1]
```

### Rotação
```javascript
function rotacionar(lista, passos) {
  return lista.map((nota, index, arr) => {
    let novoIndice = (index + passos) % arr.length;
    if (novoIndice < 0) novoIndice += arr.length;
    return arr[novoIndice];
  });
}

let melodiaRotacao = [1, 2, 3, 4];
let rotacionada = rotacionar(melodiaRotacao, 1);
console.log(rotacionada) // retorna [2, 3, 4, 1]
```

### Entrelaçar
```javascript
function entrelacar(listaA, listaB) {
  let listaC = [];
  let tamanho = Math.max(listaA.length, listaB.length);
  for (let i = 0; i < tamanho; i++) {
    listaC.push(listaA[i % listaA.length]);
    listaC.push(listaB[i % listaB.length]);
  }
  return listaC;
}

let listaA = [1,2,3];
let listaB = [4,5,6,7,8];
console.log(entrelacar(listaA,listaB)); // retorna [1, 4, 2, 5, 3, 6, 1, 7, 2, 8]
```
