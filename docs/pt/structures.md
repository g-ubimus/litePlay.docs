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

Se o intervalo tiver decimais, microtons serão incluídos:
```javascript
let m = [()=>monótono(C4, 2.1), forte, 0, .1, clarineta]
ostinato(m, 100).toque()
```

### Transposição
```javascript
transpôr([melodia], intervalo)
```

Transpõe um array de notas por um intervalo:

```JavaScript
let notas = [C4, D4, E4, F4, G4]
let original = eventList.create()
let quando = 0
for (let i = 0; i < notas.length; i++) {
  original.add([notas[i], midLevel, i])
  quando = i
}
original.toque()

let semitons = 6
let notasTranspostas = transpôr(notas, semitons)
let transposto = eventList.create()
for (let i = 0; i < notasTranspostas.length; i++) {
  transposto.add([notasTranspostas[i], midLevel, i+quando+1])
}
transposto.toque()
```

### Afinações EDO
```javascript
edo(número de divisões)
```

Cria uma escala com um número de _divisões iguais de uma oitava_, informado
no seu parâmetro de entrada.

Como a saída varia de 0 a 12 (em valores MIDI), o usuário deve transpor para
a tessitura desejada:

```javascript
let escala = edo(19)
let escalaTransposta = transpôr(escala, A4)
let lista = eventList.create()
for (let i = 0; i < escalaTransposta.length; i++) {
  lista.add([escalaTransposta[i], loud, i*.2, .2, guitar])
}
lista.toque()
```

### Just intonation
```javascript
justIntonation(altura base, número de harmônicos)
```

De forma semelhante, a função `justIntonation()` cria uma escala ajustando
as classes de altura encontradas na série harmônica de uma altura dentro de
uma oitava.

Por padrão, retorna a escala construída a partir dos primeiros treze
harmônicos em C4, lembrando a [escala acústica](https://en.wikipedia.org/wiki/Acoustic_scale):

```javascript
let afinado = justIntonation(C4, 13)
let lista = eventList.create()
for (let i = 0; i < afinado.length; i++) {
  lista.add([afinado[i], loud, i*.3, .3, clarineta])
}
lista.toque()
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

Tocando um acorde aleatório:

```javascript
let r = acordeAleatório()
let lista = eventList.create()
for (let i = 0; i < r.length; i++) {
  lista.add([r[i], loud, i*.1, .1, piano])
}
lista.toque()
```

Altere o tamanho do acorde e toque:

```javascript
let cincoNotas = acordeAleatório(5)
let lista = eventList.create()
for (let i = 0; i < cincoNotas.length; i++) {
  lista.add([cincoNotas[i], loud, i*.1, .1, piano])
}
lista.toque()
```

Crie acordes microtonais e toque:

```javascript
let acordeMicrotonal = acordeAleatório(4, médio, true)
let lista = eventList.create()
for (let i = 0; i < acordeMicrotonal.length; i++) {
  lista.add([acordeMicrotonal[i], loud, i*.1, .1, piano])
}
lista.toque()
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

Se nenhum acorde for passado, ele gera um _acorde aleatório_ com quatro notas:

```javascript
blockChord().toque()
```

Você pode torná-lo microtonal com:

```javascript
blockChord(qualquer, acordeAleatório({microtonal:true})).toque()
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

### Inversão
```JavaScript
inverter([melodia], eixo)
```

Inverte um array de alturas em torno de um eixo:
```javascript
let original = [C4, D4, E4, F4]
arpejo(qualquer, {acorde: original, direcao:"normal"}).toque()
let invertida = inverter(original, C4)
arpejo(qualquer, {acorde: invertida, direcao:"normal"}).toque()
```

### Glissando
```javascript
glissando([evento], alturaAlvo)
```

Gera um _pitch bend_ suave a partir da altura do evento inicial até a `alturaAlvo` durante a duração do evento.

```javascript
let e = [C4, forte, 0, 2, efeito2]
glissando(e, C5).toque()
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

### Ostinato
```javascript
ostinato([evento], repetições, [ritmo])
```

Para criar rapidamente um ostinato a partir de um evento, passe-o ao método `ostinato`:
```javascript
ostinato([snare, forte, 0, 1, bateria5], 10).toque()
```

Alternativamente, repita ritmos mais complexos passando um array de durações ao terceiro parâmetro:
```javascript
ostinato([snare, forte, 0, 1, bateria5], 10, [1, .5, .5, .25, .25, .25, .25]).toque()
```

### Ritmos euclidianos
```javascript
euclidiano([evento], repetições, pulsações, golpes)
```

É possível gerar _ritmos euclidianos_ com esta função dedicada,
que distribui um número definido de golpes ativos por um número total de
pulsações _da forma mais uniforme possível_. Esta distribuição cria padrões
rítmicos comuns na música tradicional de todo o mundo ([mais
informações](https://archive.bridgesmathart.org/2005/bridges2005-47.pdf)).

Por exemplo, para gerar um ritmo Tresillo (3 golpes em 8 pulsações), que se
repete 4 vezes:
```javascript
euclidiano({oQue: caixa, porQuantoTempo: .2, emAlgo: bateria}, 4, 8, 3).toque()
// a saída é [1, 0, 0, 1, 0, 0, 1, 0], ou 3+3+2
```

Este ritmo pode ser rotacionado com o (opcional) último parâmetro:
```javascript
euclidiano({oQue: caixa, porQuantoTempo: .2, emAlgo: bateria}, 4, 8, 3, 1).toque()
// a saída é [0, 1, 0, 0, 1, 0, 0, 1]
```

!!! warning
    O número de golpes (quarto parâmetro) tem de ser menor que o número de pulsações
    (terceiro parâmetro). Caso contrário, é lançado um `RangeError`.

### Sequência de rotação
```javascript
sequenciaRotacao([evento], [ritmo])
```

Gera uma lista de eventos repetindo um padrão rítmico enquanto rotaciona seus elementos a cada iteração.

```javascript
let e = [C4, forte, 0, 1, piano]
let ritmo = [0.5, 0.25, 0.25]
sequenciaRotacao(e, ritmo).play()
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

Inverte a ordem dos elementos de uma lista.

```JavaScript
let minhaLista = [1, 2, 3, 4];
let retro = retrogradar(minhaLista);
console.log(retro) // retorna [4, 3, 2, 1]
```

Um exemplo musical tocando uma melodia ao contrário:

```javascript
let melodia = [C4, D4, E4, F4, G4]
let retro = retrogradar(melodia)
let lista = eventList.create()
for (let i = 0; i < retro.length; i++) {
  lista.add([retro[i], loud, i * 0.5, 0.5, piano])
}
lista.toque()
```

### Rotação
```JavaScript
rotacionar([lista], passos)
```

Rotaciona os elementos de uma lista de acordo com o número de passos.

```JavaScript
let minhaLista = [1, 2, 3, 4];
let rotacionada = rotacionar(minhaLista, 2);
console.log(rotacionada) // retorna [3,4,1,2]
```

Um exemplo musical tocando uma melodia rotacionada:

```javascript
let melodia = [C4, D4, E4, F4, G4]
let rotacionada = rotacionar(melodia, 2)
let lista = eventList.create()
for (let i = 0; i < rotacionada.length; i++) {
  lista.add([rotacionada[i], loud, i * 0.5, 0.5, piano])
}
lista.toque()
```

### Misturar
```JavaScript
misturar([listaA], [listaB])
```

Mistura duas listas, elemento por elemento. Caso uma das listas seja maior do
que a outra, elementos da lista menor passam a se repetir.

```javascript
let listaA = [1,2,3];
let listaB = [4,5,6,7,8];
console.log(misturar(listaA,listaB)); // retorna [1, 4, 2, 5, 3, 6, 1, 7, 2, 8]
```

Um exemplo musical intercalando notas graves com uma melodia:

```javascript
let notasGraves = [C2, G2, D2]
let notasMelodia = [C4, E4, G4, B4, D5]
let misturada = misturar(notasGraves, notasMelodia)
let lista = eventList.create()
for (let i = 0; i < misturada.length; i++) {
  lista.add([misturada[i], loud, i * 0.5, 0.5, piano])
}
lista.toque()
```

### Embaralhar
```JavaScript
embaralhar([lista])
```

Embaralha a ordem dos elementos em um array.

```JavaScript
let melodia = [1, 2, 3, 4]
let embaralhada = embaralhar(melodia)
console.log(embaralhada)
```

Um exemplo musical tocando uma escala embaralhada:

```javascript
let melodia = [C4, D4, E4, F4, G4, A4, B4]
let embaralhada = embaralhar(melodia)
let lista = eventList.create()
for (let i = 0; i < embaralhada.length; i++) {
  lista.add([embaralhada[i], loud, i * 0.5, 0.5, piano])
}
lista.toque()
```
