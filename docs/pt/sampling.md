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
a.toque([agudo, forte, emBreve, longa])
```
