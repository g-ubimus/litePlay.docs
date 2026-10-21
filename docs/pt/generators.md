# Geradores
Uma opção disponível para tornar os eventos mais dinâmicos é usar _tokens_
especiais para definir seus parâmetros. Em geral, esses geradores definem um
escopo particular do qual valores aleatórios serão sorteados.

## Altura
A altura pode ser randomizada em três faixas:

```javascript
grave = altura entre C0 e C3
médio = altura entre C3 e C5
agudo = altura entre C5 e C7
```

Por exemplo:

```javascript
piano.toque([agudo, 1, 0, 1])
```

## Volume
Geradores para o volume podem ser acessados a partir de três tokens:

```javascript
fraco = amplitude entre 0.1 e 0.5
mezzo = amplitude entre 0.5 e 0.8
forte = amplitude entre 0.8 e 0.9
```

Um exemplo, no contexto:

```javascript
bateria2.toque([vibraslap, forte, 0, 1])
```

## Momento de início
Geradores para agendar o tempo de início no futuro podem ser acessados a partir
de três tokens:

```javascript
agora = início entre 0.01 e 0.5 batidas
logo = início entre 0.5 e 4 batidas
depois = início entre 4 e 8 batidas
```

Por exemplo:

```javascript
trompete.toque([Bb4, 0.5, depois, 1])
```

!!! note
    Os tempos dos eventos são medidos em batidas. No BPM padrão de 60, uma
    batida equivale a um segundo; a 120 BPM, uma batida equivale a meio
    segundo. Use `secs()` e `beats()` para converter.

## Durações
As durações podem ser randomizadas com estes três tokens:

```javascript
curta = durações entre 0.05 e 0.2 batidas
média = durações entre 0.2 e 2 batidas
longa = durações entre 2 e 5 batidas
```

Por exemplo:

```javascript
trombone.toque([Eb2, 1, 0, longa])
```

## Famílias de bateria
Dois geradores podem ser usados para obter diferentes tipos de peças de
bateria/percussão para o atributo _what_ (o que):

```javascript
membranofone = instrumentos que possuem uma membrana vibratória 
idiofone = todos os outros
```

Por exemplo:

```javascript
bateria.play(membranofone)
bateria.play([idiofone, .5, depois, 1])
```

## Grupos de instrumentos
Além disso, instrumentos aleatórios que pertencem a um mesmo grupo podem ser
chamados no último atributo de `play()`:

```javascript
emAlgo
naBateria
nasTeclas
nasCordas
nosArcos
nosBaixos
nosMetais
nosSopros
noSintetizador
nosEfeitos
naPercussão
naVoz
onEnsemble
onLead
```

Para contextualizar, um exemplo combinando tudo o que aprendemos aqui:

```javascript
toque([médio, fraco, agora, curta, naPercussão])
```

Aviso: note que, caso não seja definida a duração do evento, instrumentos
sustentados tocarão para sempre. Isso pode ocorrer, por exemplo em:

```javascript
toque(emAlgo)
```

## Geradores utilitários
Estes auxiliares retornam valores aleatórios ou opções para usar dentro de
eventos:

```javascript
númeroAleatório(min, max)       // float aleatório, max exclusivo
rnd(min, max)                   // equivalente em inglês
inteiroAleatório(min, max)      // inteiro aleatório, max exclusivo
rndInt(min, max)                // equivalente em inglês
escolha(opção1, opção2, ...)    // uma opção ao acaso
choose(...)                     // equivalente em inglês
quieto(ms)                      // promise resolvida após ms milissegundos
```

`escolha()` também aceita um único array:

```javascript
toque(escolha(C4, E4, G4));
toque(escolha([C4, E4, G4]));
```
