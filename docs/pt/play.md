# `toque()`

No núcleo do litePlay.js temos o `play()`. Ele pode ser simplesmente executado
como

```javascript
toque();
```

para tocar um som, e `pare()` para pará-lo. Estas linhas de código podem ser
digitadas diretamente em um console REPL interativo ou adicionadas ao script.

Podemos fazer alterações neste som definindo o instrumento padrão que o reproduz,

```javascript
instrumento(órgão);
```

o padrão foi definido como piano no início, mas agora mudamos para organ. Uma
introdução aos instrumentos é dada no final do tutorial.

## Outras formas de `toque()`

Além de notas e listas de eventos, `toque()` aceita um instrumento ou uma
função:

```javascript
toque(órgão);          // chama órgão.toque()
toque(() => E4);       // chama a função e toca o resultado
```

`pare()` para apenas o instrumento padrão. Use `instrumento.pare()` para parar
um instrumento específico, ou `reset()` para parar o instrumento padrão, o
sequenciador e limpar o estado dos efeitos:

```javascript
órgão.pare();
reset();
```

Também podemos dizer o que tocar. Se o som for de um instrumento afinado
(pitch), podemos pedir para tocar uma determinada nota,

```javascript
toque(E4);
```

Os nomes simbólicos das notas variam de Cm1 (=C<sub>-1</sub>) a G8
(=G<sub>8</sub>), de tal forma que o Dó central de um teclado de piano é
definido como C4. O som é tocado imediatamente e dura indefinidamente (embora
alguns possam decair em intensidade ao longo do tempo).

Acidentes são representados pela letra minúscula `s` para _sustenidos_ (sharp)
e `b` para notas _bemóis_ (flat):

```javascript
toque(Eb4);
toque(Fs4);
```

## Events (Eventos)

No litePlay.js podemos pensar na ação resultante do `play()` desta forma como
um _evento_ musical. Podemos defini-lo com cinco atributos:

* _what_ (o que): a coisa que tocamos, que pode variar, mas no caso mostrado
  anteriormente, é a altura (pitch) do som.
* _howLoud_ (quão forte): o volume do som, que pode ser definido por um valor
  numérico em uma escala de 0 a 1.
* _when_ (quando): o tempo do evento, quando ele deve acontecer, em batidas.
* _howLong_ (quanto tempo): por quanto tempo o som deve durar, em batidas.
* _onSomething_ (em algo): a coisa que fará o som, o instrumento, como `organ`,
  `violino` etc. Existem vários destes para escolher. Dependendo do tipo de
  instrumento, o que (_what_) pode ser tocado pode variar. Por exemplo, no caso
  da `bateria`, não temos altura, mas sons de percussão
  diferentes como `caixa`, `bumbo`, etc.

Os atributos (ou parâmetros) do evento são opcionais, como já vimos. Se os
passarmos, os padrões serão usados. É possível passar apenas alguns parâmetros,
por exemplo, apenas _what_; _what_ e _howLoud_; _what_, _howLoud_ e _when_; bem
como _what_, _howLoud_, _when_ e _howLong_.

Os eventos são passados usando uma lista JS (ou array) com atributos na ordem
listada anteriormente:

```javascript
[o quê, quão forte, quando, quanto tempo, em algo]
```

Por exemplo,

```javascript
toque([C4, 0.5, 0, 2, violino])
```

A ação `toque()` pode receber vários eventos como argumentos, como

```javascript
toque([C4, 0.1, 0, 3], [E4, 0.2, 0.5, 0.5], [G4, 0.4, 2, 0.1])
```

### Eventos como objetos

Um evento também pode ser escrito como um objeto. As chaves em inglês e em
português são aceitas:

```javascript
toque({ oQue: C4, quãoForte: 0.5, quando: 0, quãoLongo: 2, emAlgo: violino });
```

Os nomes equivalentes em inglês são `what`, `howLoud`, `when`, `howLong` e
`onSomething`.

`any` (também exportado como `algum`) é simplesmente `undefined`. Ele serve como
argumento de evento quando uma função espera um evento, mas você quer os
valores padrão:

```javascript
arpejo(any, [C4, E4, G4]).toque();
acordeEmBloco(any, [C4, E4, G4]).toque();
```

`resolveEvent()` preenche altura, intensidade, tempo, duração e instrumento
padrão para `any`. Ele não é um curinga dentro de um array de evento; para
eventos parciais use o formato de objeto, ou omita os valores finais:

```javascript
toque({oQue: E4, quando: 0, quãoLongo: 1});
toque([E4, 1, 0]);
```

!!! note
    Os tempos dos eventos são medidos em batidas, não em segundos. No BPM
    padrão de 60, uma batida equivale a um segundo; a 120 BPM, uma batida
    equivale a meio segundo. Use `secs()` e `beats()` para converter.

Agora aprendemos que podemos trabalhar com listas de eventos, em vez de apenas
enviar eventos individuais para o `toque()`. Há um aspecto particular disso que
devemos notar, os atributos _when_ de cada evento serão interpretados de uma
certa maneira.

### Uma lista simples de _what_ (o que)

```javascript
toque(C3, C4, C5);
```

Neste caso, os eventos serão separados pelo _howLong_ padrão para o
_onSomething_ sendo tocado (uma batida, que equivale a um segundo no BPM
padrão de 60). Assim, ouvimos os sons em sequência.

### Uma lista de eventos incompletos

```javascript
toque([C3], [C4], [C5]);
```

Neste caso, o padrão para _when_ é 0, imediatamente, para todos os eventos na
lista. Ouvimos os sons começando ao mesmo tempo, misturados.

### Uma lista de eventos com atributos _when_ definidos explicitamente

```javascript
toque([C3, 0.1, 0], [C4, 0.2, 1], [C5, 0.4, 2]);
```

Neste caso, o tempo dos eventos é relativo ao momento em que pedimos para a
lista de eventos ser tocada, com talvez um atraso muito curto. Todos os eventos
são cronometrados com precisão em relação a isso. Podemos decidir quando eles
devem entrar com um tempo exato.

E se uma lista de eventos se tornar um [objeto](./eventList.md)?
