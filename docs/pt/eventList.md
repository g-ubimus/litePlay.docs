# listaEventos
Listas de eventos podem ser transformadas em um objeto que pode ser manipulado.

## listaEventos.criar()
É usado para criar uma lista, geralmente atribuída a uma variável:

```javascript
let lista = listaEventos.criar([C3, 0.1, 0], [C4, 0.2, 1], [C5, 0.4, 2]);
```

Também aceita eventos como objetos, usando as mesmas chaves de `toque()`:

```javascript
let lista = listaEventos.criar(
  { oQue: C3, quãoForte: 0.1, quando: 0, quãoLongo: 1 },
  { oQue: E4, quãoForte: 0.2, quando: 1, quãoLongo: 1 },
);
```

Então podemos tocá-la repetidamente (chamando `toque()` nela):

```javascript
lista.toque();
```

O `toque()` pode receber dois parâmetros opcionais,

```javascript
listaEventos.toque(quando, eventos);
```

O primeiro é um _quando_ para a lista completa, obtido a partir do momento da
ação, para que possamos posicionar a performance no futuro. O segundo é uma
lista de eventos, basicamente uma lista JS de listas, como:

```javascript
[[C3, 0.1, 0, 1], [C4, 0.2, 1, 1], [C5, 0.4, 2, 1]]
```

Como pode ser visto, há uma lista externa que contém três eventos, que são eles
próprios listas (de atributos). Um aspecto fundamental é que a lista de eventos
passada para `toque()` substitui os eventos existentes (se algum tiver sido
adicionado) no objeto.

Os eventos armazenados ficam disponíveis no array `events`. Para uso de mais
baixo nível, `score(quando, eventos)` retorna um objeto com o texto da partitura
Csound e um método `play()`, sem alterar os eventos armazenados.

## listaEventos.adicionar()

```javascript
listaEventos.adicionar(event, ...)
```

Adiciona eventos ao final da lista. Pode receber qualquer número de eventos
(assim como o `criar()`).

## listaEventos.remover()

```javascript
listaEventos.remover(índice)
```

Remove um evento com um determinado índice da lista, ou o último evento (se
nenhum `índice` for fornecido).

## listaEventos.inserir()

```javascript
listaEventos.inserir(posição, event, ...)
```

Insere um ou mais eventos no objeto, na posição `posição`.

```javascript
lista.adicionar(event, [C4, 0.9, 3]);
lista.remover(0);
lista.inserir(1, [D4, 0.8, 0.5]);
```

## listaEventos.repetir()

Também podemos repetir uma `listaEventos` qualquer número de `vezes`, em
`quando` **batidas** após a ação,

```javascript
lista.repetir(vezes, quando);
```

Tanto `listaEventos.toque()` quanto `listaEventos.repetir()` retornam o tempo de
término da reprodução. Portanto, podemos usar essa informação para agendar
outros eventos. Por exemplo, este código toca uma lista de eventos, adiciona
dois eventos a ela e, em seguida, agenda a repetição dessa sequência mais longa
por três vezes após o término da primeira ação `toque()`,

```javascript
let lista = listaEventos.criar([C4, 0.1, 0, 1],
                               [E4, 0.2, 1, 1],
                               [G4, 0.4, 2, 1]);
let fim = lista.toque();
lista.adicionar([C1, 0.1, 3, 3], [C1, 0.1, 0, 4]);
lista.repetir(3, fim);
```

Uma consequência de todas essas ideias é que introduzimos a ideia de que uma
ação `toque()` pode ter formas diferentes, dependendo do contexto em que está
sendo invocada, que no momento pode ser

- O contexto global do litePlay.js: `toque()`
- Um objeto de instrumento: `órgão.toque()`
- Uma `listaEventos`: `listaEventos.toque()`

Com eventos e listas de eventos, podemos construir composições e performances
com tempo relativo preciso entre os eventos, o que é algo desejável em
atividades musicais.

## Interface em inglês

O objeto também é exportado como `eventList`. Os métodos equivalentes são
`play`, `create`, `repeat`, `clear`, `insert`, `add` e `remove`.
