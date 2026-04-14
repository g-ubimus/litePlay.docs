# Bem-vindo! 

## O que é o litePlay.js?
O litePlay.js é um módulo JavaScript (JS) para codificação leve (*lite coding*)
de música. Ele pode ser usado em quaisquer aplicações web, mas pensamos que
seja particularmente útil para programação musical interativa, orientada à
performance ou à composição (ou a ambas). Este tutorial está focado nesses
casos de uso.

## Como usá-lo?
Existem várias maneiras de usar o litePlay.js, escolha a que você deseja testar
primeiro:

* [Editor online do litePlay.js](#editor-online-do-liteplay)
* [Outros ambientes online](#outros-ambientes-online)
* [Carregue-o em seu site](#carregue-o-em-seu-site)

### Editor online do litePlay
[Editor online do litePlay](https://g-ubimus.github.io/litePlay.js/): um editor
simples projetado para testar recursos experimentais com REPL, onde as funções
do litePlay podem ser chamadas sem o prefixo. Ele é capaz de salvar código e
gravar áudio. O upload de arquivos de mídia não é suportado no momento.

Caso escolha esta opção, vá em frente e [toque()](./play.md).

### Outros ambientes online
Há vários ambientes de codificação online disponíveis para o desenvolvimento em
JS com diferentes características e suporte para vários tipos de uso. Para o
presente tutorial, estamos procurando os seguintes requisitos:

- Permitir o acesso de edição tanto ao script JS quanto ao código da página
  HTML principal.
- Fornecer um meio de interação com o código, para que possa ser digitado e
  executado interativamente.

Nem todos os ambientes cumprem com isso. Os três a seguir sim:

- [JS Playground](https://www.jsplayground.dev/): uma plataforma leve para código JS
  interativa com REPL, permitindo a edição de HTML, JS e CSS. Não há suporte
  para salvar projetos até o presente momento.
- [Playcode](https://playcode.io): uma plataforma interativa de código JS com a
  facilidade de atualização/re-execução de código ao vivo, mas sem REPL. O
  suporte para salvar projetos em contas de usuários está disponível.
- [Editor p5.js](https://editor.p5js.org/): uma plataforma JS interativa,
  projetada para o desenvolvimento de aplicações multimídia p5.js, mas que
  suporta o litePlay.js muito bem. Permite a edição de código HTML e JS, bem
  como a importação de outros arquivos de mídia para o projeto. Fornece um
  REPL e suporta salvar projetos em contas de usuário.

### Carregue-o em seu site 
Para carregar o módulo litePlay.js em seu site, basta adicionar a seguinte tag
de script ao cabeçalho da página HTML principal:

```
<script  src="https://g-ubimus.github.io/litePlay.js/litePlay.constants.js"></script>
```

Quando a página for carregada, você terá acesso a várias constantes do sistema,
bem como à seguinte função:

```
lpRun();
```

que carrega o módulo, inicia o motor de áudio e avisa ao usuário que o sistema
está pronto para uso quando uma mensagem é impressa no console JS.
Alternativamente, uma função `f()` pode ser usada como ponto de partida da
aplicação litePlay.js. Ou pode conter todo o código que deve ser executado
quando o script for rodado:

```
function f() {
 // Código a ser executado aqui
}
lpRun(f);
```

Uma vez que o motor litePlay.js esteja rodando, podemos usar sua funcionalidade
em qualquer lugar prefixando suas funções etc com `lp.`, por exemplo:

```
lp.play();
```
