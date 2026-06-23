# Efeitos de áudio
Alguns efeitos de áudio úteis para manipular cada instrumento estão disponíveis.

## Reverb
Controlamos a quantidade de reverb de um instrumento com um argumento de 0 a 1.

```javascript
bateria.reverb(1);
bateria.toque([kick, .9, 2]);
```
## Pan 
Valores de panning vão de -1 (totalmente à esquerda) a 1 (totalmente à direita).

```javascript
bateria2.pan(-1);
bateria2.toque([snare, .5, 0]);
```

### Espacialização automática (Auto-panning)
```javascript
panAutomático(instrumento, hertz)
```

Essa função move um som ao redor do campo estéreo da esquerda para a direita e
de volta, em uma taxa de _hertz_. 

## Envelope de Amplitude 
Podemos definir um envelope de amplitude ajustando a duração para uma função ADSR:

```javascript
ampEnvelope(ataque, decaimento, sustentação, repouso)
```

Por exemplo:

```javascript
cordas.reverb(1);
cordas.ampEnvelope(1,2,3,4);
cordas.toque([C2, 1, 0], [Bb2, .9, .5], [Ab3, .8, 1], [Gb4, .7, 1.5], [E5, .6, 2], [D6, .5, 2.5]); // Som alienígena
```

## Bend
Podemos alterar a altura de um instrumento usando o método `bend`. A quantidade é dada em semitons.

```javascript
sintetizador.bend(1); // Sobe a altura em um semitom
```

## Cutoff
Controlamos a quantidade do filtro de corte (cutoff) com um argumento de 0 a 1.

```javascript
sintetizador.cutoff(0.5);
```

## Ressonância
Controlamos a ressonância do filtro com um argumento de 0 a 1.

```javascript
sintetizador.resonance(0.8);
```

## Envelope de Filtro
Podemos definir um envelope de filtro ajustando a quantidade e a duração para uma função ADSR:

```javascript
filterEnvelope(quantidade, ataque, decaimento, sustentação, repouso)
```

Por exemplo:

```javascript
sintetizador.filterEnvelope(0.8, 1, 2, 3, 4);
```
