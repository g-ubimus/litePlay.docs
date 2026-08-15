# Sampling
É possível trabalhar diretamente com amostras de áudio. Para isso, selecione o botão "ADD SAMPLE" na 
interface do [editor do litePlay.js](https://g-ubimus.github.io/litePlay.js/).

Por exemplo, se carregarmos o arquivo "pianoc2.wav", disponível na pasta
"assets" do repositório do litePlay.js, ele vira um objeto acessível pelo menos
nome, mas sem a extensão de áudio:

```javascript
pianoc2.toque()
```

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

## Mudando a velocidade de reprodução
Para mudar a velocidade de reprodução, podemos fazer:
```javascript
a.instr.speed(10)
a.toque()
```

Valores acima de 1 reproduzem mais rápido, enquanto valores abaixo de 1
reproduzem lentamente. Combinando com outros métodos, podemos criar texturas
interessantes:

```javascript
for (let i = 0; i < 1000; i++) {
  a.instr.speed(.1)
  a.instr.ampEnvelope(2,0,0,1)
  a.toque([agudo, forte, i*.1])
}
```

## Repetindo samples em loop
Podemos repetir uma amostra de som em loop dentro de limites definidos em
segundos:
```javascript
a.loop(1,2)
a.toque()
```
