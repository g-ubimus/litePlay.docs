# Estruturas musicais
Essa página apresenta algumas estruturas musicais recorrentes (além de funções
úteis) já disponíveis no sistema.

## Altura & harmonia
### Obter nomes das notas
```JavaScript
console.log(midiParaNome(60)); // returns "C4"
console.log(midiPataNome(31)); // returns "G1"
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

### Monótono
```javascript
monótono(altura inicial)
```

Retorna quase sempre a mesma altura, que pode variar um semitom acima ou abaixo.

Por exemplo, teste:

```javascript
let e = [()=>monótono(C4), forte, 0, .1, clarineta]
ostinato(e, 100).toque()
```

### Transposição
```javascript
transpôr([melodia], semitons);
```

Transpõe uma lista de notas em um número de semitons:

```JavaScript
let m = [C4, D4, E4, F4, G4];
let s = 2;
let transposto = transpôr(m, s);

console.log(transposto) // returns [D4, E4, Fs4/Gb4, G4, A4]
```

### Acorde aleatório
```javascript
acordeAleatório(tamanho, tessitura, microtonal = false);
```

Gera um acorde (4 notas por padrão, na tessitura `médio`) com alturas aleatórias:
```javascript
let a = acordeAleatório();
console.log(a);
```

Altere o tamanho do acorde com o primeiro parâmetro:
```javascript
let cincoNotas = acordeAleatório(5);
console.log(cincoNotas);
```

Altere a tessitura com o segundo parâmetro, usando os geradores de altura do litePlay:
```javascript
let acordeAgudo = acordeAleatório(4, agudo);
let acordeGrave = acordeAleatório(4, grave);
console.log(acordeAgudo);
console.log(acordeGrave);
```

Crie acordes microtonais:
```javascript
let acordeMicrotonal = acordeAleatório(4, médio, true);
console.log(acordeMicrotonal);
```

### Acorde em bloco
```javascript
blockChord([evento], [acorde])
```

Gera uma lista de eventos tocando todas as notas de um acorde simultaneamente.

```javascript
let e = [C4, forte, 0, 1, piano]
let cmaj7 = [C4, E4, G4, B4]
blockChord(e, cmaj7).toque()
```

### Arpejador
```javascript
arpejo([evento], [acorde], repetições, direção = "vaiVolta")
```

Por padrão, a função `arpejo` toca um arpejo "vaiVolta" de um *acorde aleatório*, uma única vez:
```JavaScript
arpejo().toque();
```

Você pode passar um evento para definir os parâmetros do som, mas ele ainda tocará um acorde aleatório:
```javascript
let e = [C3, .5, 0, .1, xilofone];
arpejo(e).toque();
```

Defina o acorde passando-o como o segundo parâmetro:
```javascript
let e = [C3, .5, 0, .1, xilofone];
let cmaj7 = [C4, E4, G4, B4];
arpejo(e, cmaj7).toque();
```

Defina um número diferente de repetições:
```javascript
let e = [C3, .5, 0, .1, xilofone];
let cmaj7 = [C4, E4, G4, B4];
arpejo(e, cmaj7, 10).toque();
```

Altere a direção de leitura da lista ("normal" ->, "inversa" <- ou "vaiVolta" -><-):
```javascript
let e = [C3, .5, 0, .1, xilofone];
let cmaj7 = [C4, E4, G4, B4];
arpejo(e, cmaj7, 10, "normal").toque();
```

### Sequência intervalar
```JavaScript
sequênciaIntervalar([evento], intervalo, repetições, para cima?)
```

Um tipo diferente de arpejo pode ser construído usando uma sequência de
intervalos.

Por padrão, começa em uma nota aleatória e sobe ou desce quatro notas, seguindo
um intervalo aleatório:
```javascript
sequênciaIntervalar().toque();
```

Crie um evento e use-o como base para a função:
```javascript
let e = [E4, .8, 0, .5, guitar];
sequênciaIntervalar(e).toque();
```

Selecione um intervalo específico para a sequência:
```javascript
let e = [E4, .8, 0, .5, guitar];
sequênciaIntervalar(e, 3).toque();
```

Altere o número de vezes que ele será transposto:
```javascript
let e = [E4, .8, 0, .5, guitar];
sequênciaIntervalar(e, 3, 10).toque();
```

Faça com que apenas suba:
```javascript
let e = [E4, .8, 0, .1, piano];
sequênciaIntervalar(e, 3, 10, true).toque();
```

Force-o a descer alterando o último parâmetro para `false`:
```javascript
let e = [E6, .8, 0, .1, piano];
sequênciaIntervalar(e, 3, 10, false).toque();
```

### Sequência de rotação
```javascript
sequenciaRotacao([evento], [ritmo])
```

Gera uma lista de eventos repetindo um padrão rítmico enquanto rotaciona seus elementos a cada iteração.

```javascript
let e = [C4, forte, 0, 1, noSintetizador]
let ritmo = [0.5, 0.25, 0.25]
sequenciaRotacao(e, ritmo).toque()
```

### Inversão
```JavaScript
inverter([melodia], eixo)
```

Inverte um array de alturas em torno de um eixo:
```javascript
let melodia = [C4, D4, E4, F4];
let invertida = invert(melodia, C4);
console.log(invertida); // retorna ["C4","Bb4","Ab4","G4"]
```

### Glissando
```javascript
glissando([evento], alturaAlvo)
```

Gera um _pitch bend_ suave a partir da altura do evento inicial até a `alturaAlvo` durante a duração do evento.

```javascript
let e = [C4, forte, 0, 2, noSintetizador]
glissando(e, C5).play()
```

## Ritmo & tempo
### Definir/obter batidas por minuto
```JavaScript
setBpm(value);
console.log(getBpm());
```

### Conversões
```JavaScript
secs(beats); // retorna a duração em segundos de um pulso

beats(seconds) // retorna o números de pulsos de um intervalo em segundos
```

### Obter o tempo atual do relógio de áudio
```JavaScript
console.log(audioClock())
```

### Variação de andamento (accelerando & rallentando)
```JavaScript
maisRápido([evento], últimaDuração, passos)
maisLento([evento], últimaDuração, passos)
```

Gera um _accelerando_ com `maisRápido` definindo a última duração mais curta que a primeira:
```javascript
let e = [membranofone, forte, 0, 1, bateria2];
maisRápido(e, .1, 9).toque();
```

Gera um _rallentando_ com a última duração mais longa que a primeira:
```javascript
let e = [idiofone, .1, 0, .1, bateria4];
maisLento(e, 2, 20).toque();
```

## Dinâmica
### Crescendo e decrescendo
```JavaScript
maisForte([evento], últimaAmplitude, passos)
maisSuave([evento], últimaAmplitude, passos)
```

Semelhante às funções de accelerando/rallentando.

Faça um crescendo com um evento suave indo, por exemplo, até a amplitude máxima (1):
```javascript
let a = [C4, .1, 0, 1, violino];
maisForte(a, 1, 10).toque();
```

Faça um decrescendo com o oposto:
```javascript
let a = [C4, 1, 0, 1, violoncelo];
maisSuave(a, .1, 10).toque();
```

## Manipulando efeitos de áudio
### Espacialização automática
```javascript
espacializador(instrumento, hertz)
```

## Manipulação de listas
### Retrogradar
```JavaScript
retrogradar([lista])
```

```JavaScript
let minhaLista = [1, 2, 3, 4];
let retro = retrogradar(minhaLista);
console.log(retro) // retorna [4, 3, 2, 1]
```

### Rotação
```JavaScript
rotacionar([lista], índices)
```

```JavaScript
let minhaLista = [1, 2, 3, 4];
let rotacionada = rotacionar(minhaLista, 2);
console.log(rotacionada) // retorna [3,4,1,2]
```

### Mistura
```JavaScript
misturar([listaA], [listaB])
```

```javascript
let listaA = [1,2,3];
let listaB = [4,5,6,7,8];
console.log(misturar(listaA,listaB)); // retorna [1, 4, 2, 5, 3, 6, 1, 7, 2, 8]
```

### Embaralhar
```JavaScript
embaralhar([lista])
```

Randomiza a ordem dos elementos em um array.

```JavaScript
let melodia = [1, 2, 3, 4]
let embaralhada = embaralhar(melodia)
console.log(embaralhada)
```
