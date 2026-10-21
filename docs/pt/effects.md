# Efeitos de áudio
Alguns efeitos de áudio úteis para manipular cada instrumento estão disponíveis.

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

## Envelope de amplitude 
Podemos definir um envelope de amplitude ajustando a duração para uma função ADSR:

```javascript
ampEnvelope(duração ataque, duração decaimento, nível sustentação, duração repouso)
```

Por exemplo:

```javascript
cordas.reverb(1);
cordas.ampEnvelope(.5,1,1,10);
cordas.toque([C2, 1, 0], [Bb2, .9, .5], [Ab3, .8, 1], [Gb4, .7, 1.5], [E5, .6, 2], [D6, .5, 2.5]); // Som alienígena
```

## Envelope de filtro
Podemos definir um envelope de filtro (banda baixa) ajustando a quantidade e a
duração para uma função ADSR:

```javascript
filterEnvelope(quantidade, ataque, decaimento, sustentação, repouso)
```

Por exemplo:

```javascript
sintetizador.filterEnvelope(0.8, 1, 1, 1, 4);
```

## Deslocamento de frequência
Sons inarmônicos podem ser criados a partir de sons harmônicos pelo processo de
deslocamento de suas frequências (frequency shift), para cima ou para baixo:
```javascript
piano.shift(10)
piano.toque()
```

Podemos fazer o mesmo com samples:
```javascript
pianoc2.instr.shift(-10)
pianoc2.toque()
```

## Pan 
Valores de panning vão de 0 (totalmente à esquerda) a 1 (totalmente à
direita).

```javascript
bateria2.pan(.2);
bateria2.toque([snare, .5, 0]);
```

### Espacialização automática (Auto-panning)
```javascript
instrumento.panAutomático(hertz)
```

Esse método move um som ao redor do campo estéreo da esquerda para a direita e
de volta, em uma taxa de _hertz_. Use `instrumento.panAutomático(0)` para
parar.

Por exemplo:
```javascript
flauta.panAutomático(1)
flauta.toque()
```

## Ressonância
Controlamos a ressonância do filtro com um argumento de 0 a 1.

```javascript
sintetizador.resonance(0.8);
```

## Delay
Podemos adicionar um delay com um tempo em segundos e uma quantidade de
realimentação:

```javascript
bateria.delay(.1, .9);
bateria.toque([kick, .9, 0]);
```

O tempo máximo é de 2 segundos e a realimentação vai de 0 a .99. Use
`delay(0, 0)` para desativar a linha de delay.

## Reverb
Controlamos a quantidade de reverb de um instrumento com um argumento de 0 a 1.

```javascript
bateria.reverb(1);
bateria.toque([kick, .9, 2]);
```

## Volume
O volume geral de um instrumento é definido com um valor de 0 a 1:

```javascript
piano.volume(.5);
piano.toque(C4);
```

## Modificadores de sinal
Além dos efeitos acima, todo instrumento tem quatro modificadores de sinal do
Csound. Os valores são limitados às faixas indicadas; `0` desativa o efeito.

### Distortion
```javascript
instrumento.distortion(quantidade)
```

Modela a onda (waveshaping). `quantidade` vai de 0 a 1:

```javascript
guitarra.distortion(.6);
guitarra.toque(E2);
```

### Highpass
```javascript
instrumento.highpass(corte)
```

Filtro passa-alta. `corte` vai de 0 (desativado) a 1 (mais brilhante):

```javascript
piano.highpass(.4);
piano.toque(C4);
```

### Tremolo
```javascript
instrumento.tremolo(taxa, profundidade)
```

Modulação de amplitude em `taxa` Hz, com `profundidade` de 0 a 1. Use uma taxa
abaixo de 10 Hz; valores maiores são tratados como 0:

```javascript
órgão.tremolo(6, .6);
órgão.toque(G3);
```

### Chorus
```javascript
instrumento.chorus(taxa, profundidade)
```

Efeito de chorus/flanger. `taxa` é em Hz e `profundidade` em segundos,
limitada a 0.025. `chorus(0, 0)` desativa o efeito:

```javascript
cordas.chorus(.25, .02);
cordas.toque([C4, 1, 0, 4]);
```

!!! note
    `delay()` e `chorus()` mantêm uma linha de efeito por instrumento em
    execução. Use `delay(0, 0)` ou `chorus(0, 0)` para desativá-la, ou
    `reset()` para parar tudo e limpar o estado dos efeitos.
