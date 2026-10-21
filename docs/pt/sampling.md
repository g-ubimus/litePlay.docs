# Sampling
É possível trabalhar diretamente com amostras de áudio. Para isso, inicie o
motor (clique em qualquer lugar do editor) e selecione o botão "ADD SAMPLE" na
interface do [editor do litePlay.js](https://g-ubimus.github.io/litePlay.js/).

Por exemplo, se carregarmos o arquivo "pianoc2.wav", disponível na pasta
"assets" do repositório do litePlay.js, ele vira um objeto acessível pelo mesmo
nome, mas sem a extensão de áudio:

```javascript
pianoc2.toque()
```

!!! note
    O editor monta o nome da variável a partir da parte do nome do arquivo
    antes do primeiro ponto, substituindo todo caractere que não seja letra ou
    número por `_`. Por exemplo, `my-file.wav` vira `my_file`; o editor também
    imprime o nome gerado no console.

Mas podemos renomear essa _sample_ da forma que quisermos, assinalando-a a uma
variável:

```javascript
let a = pianoc2
```

Podemos tocá-la em uma altura diferente: 

```javascript
a.toque(agudo)
```

E claro, utilizar os demais parâmetros: 

```javascript
a.toque([agudo, forte, logo, longa])
```

## Carregando amostras no código

O objeto `amostra` também pode carregar arquivos sem o botão do editor:

```javascript
let s = amostra.criar("assets/audio/pianoc2.wav");
quieto(500).then(() => s.toque());
```

!!! note
    `amostra.carregar()` busca e decodifica o arquivo de forma assíncrona.
    Espere antes de tocar; `quieto(ms)` é um temporizador baseado em promise.

`amostra.criar(what, fo, bpm)` cria um objeto de sample e, opcionalmente, carrega
`what`. `fo` é a altura de referência do arquivo (padrão 60) e `bpm` é o
andamento em que ele foi gravado (0 significa sem sincronia de tempo).
`amostra.carregar(url, fo, bpm)` carrega um arquivo em uma amostra existente, e
`instrument()` retorna seu `Sampler`:

```javascript
let s = amostra.criar();
s.carregar("assets/audio/pianoc2.wav", 60, 120);
s.instrument().reverb(.4);
quieto(500).then(() => s.toque());
```

Quando `bpm` é maior que 0, `Sampler.play()` acompanha o BPM atual do litePlay
(`setBpm()`); isso mantém loops e amostras gravadas em tempo. `amostra.pare()`
para a amostra, e `amostra.velocidade(valor)` muda sua velocidade de reprodução.

Um objeto de sample pode ser passado diretamente como instrumento de um evento
ou de uma faixa do sequenciador:

```javascript
toque([C4, 1, 0, 2, s]);
sequenciador.adicionar(s, [C4, O, E4, O], .8, .5);
```

## Mudando a velocidade de reprodução
Para mudar a velocidade de reprodução, podemos fazer:
```javascript
a.velocidade(10)
a.toque()
```

Valores acima de 1 reproduzem mais rápido, enquanto valores abaixo de 1
reproduzem lentamente. Combinando com outros métodos, podemos criar texturas
interessantes:

```javascript
for (let i = 0; i < 1000; i++) {
  a.velocidade(.1)
  a.instr.ampEnvelope(2,0,0,1)
  a.toque([agudo, forte, i*.1])
}
```

## Repetindo samples em loop
Podemos repetir uma amostra de som em loop dentro de limites definidos em
segundos:
```javascript
a.repetir(1,2)
a.toque()
```
