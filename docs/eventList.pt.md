# eventList 
Listas de eventos podem ser transformadas em um objeto JS que pode ser manipulado. 

## eventList.create()
É usado para criar um evento, geralmente atribuído a uma variável:

```javascript
let lista = eventList.create([C3, 0.1, 0], [C4, 0.2, 1], [C5, 0.4, 2]);
```

então podemos tocá-lo repetidamente (chamando `toque()` nele):

```javascript
lista.toque();
```

O `toque()` pode receber dois parâmetros opcionais,

```javascript
eventList.toque(quanto, eventos);
```

o primeiro é um _quando_ para a lista completa, obtido a partir do
momento da ação, para que possamos posicionar a performance no futuro. O
segundo é uma lista de eventos, basicamente uma lista JS de listas, como:

```
[[C3, 0.1, 0, 1], [C4, 0.2, 1, 1], [C5, 0.4, 2, 1]]
```

como pode ser visto, há uma lista externa que contém dois eventos, que são
eles
próprios listas (de atributos). Um aspecto fundamental é que a lista de eventos
passada para `toque()` substitui os eventos existentes (se algum tiver sido
adicionado) no objeto.

## eventList.add()

```
eventList.add(event, ...)
```

Adiciona eventos ao final da lista. Pode receber qualquer número de eventos (assim como o `create()`).

## eventList.remove() 

```
eventList.remove(índice)
```

Remove um evento com um determinado índice da lista, ou o último evento (se
nenhum `índice` for fornecido).

## eventList.insert() 

```
eventList.insert(posição, event, ...)
```

insere um ou mais eventos no objeto, após o índice `posição`.

```
lista.add(event, [C4,0.9,3]);    
lista.remove(0);    
lista.insert(1, [D4, 0.8, 0.5]);
```

## eventList.repeat()

Também podemos repetir um `eventList` qualquer número de `vezes`, em `quando`
segundos após a ação,

```
lista.repeat(vezes, quando);
```

Tanto `eventList.toque()` quanto `eventList.repeat()` retornam o tempo de
término da reprodução. Portanto, podemos usar essa informação para agendar
outros eventos. Por exemplo, este código toca uma lista de eventos, adiciona
dois eventos a ela e, em seguida, agenda a repetição dessa sequência mais longa
por três vezes após o término da primeira ação `toque()`,

```
let lista = eventList.create([C4, 0.1, 0, 1],
                              [E4, 0.2, 1, 1], 
                              [G4, 0.4, 2, 1]);
let end = lista.toque();
lista.add([C1, 0.1, 3, 3], [C1, 0.1, 0, 4]);
lista.repeat(3, end);
```

Uma consequência de todas essas ideias é que introduzimos a ideia de que uma
ação `toque()` pode ter formas diferentes, dependendo do contexto em que está
sendo invocada, que no momento pode ser

- O contexto global do litePlay.js: `toque()`
- Um objeto de instrumento: `órgão.toque()`
- Uma _eventList_: `eventList.toque()`

Com eventos e eventLists, podemos construir composições e performances com
tempo relativo preciso entre os eventos, o que é algo desejável em atividades
musicais.
