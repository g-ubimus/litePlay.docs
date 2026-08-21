# Contribuindo
Obrigado por considerar contribuir com o litePlay.js! Há várias formas de
ajudar:

* Relatar bugs e sugerir ideias pelas
  [issues](https://github.com/g-ubimus/litePlay.js/issues).
* Implementar novas funcionalidades
* Melhorar a documentação ou adicionar exemplos

## Antes de programar
Se você quiser adicionar uma nova funcionalidade, abra primeiro uma issue
para discuti-la. Isso ajuda a manter o litePlay.js simples e acessível para
iniciantes. É desejável que novos recursos se adaptem ao modelo de eventos já
estabelecido (`[o que, quão forte, quando, quanto tempo, em algo]`), sigam os
padrões de design já existentes e, se possível, façam uso de classes, objetos
e funções já existentes (como Instrument, Sample, eventList e play).

## Preparando o ambiente de desenvolvimento
Clone o repositório e instale as dependências:

```
git clone git@github.com:g-ubimus/litePlay.js.git
cd litePlay.js
npm install
```

Para testar suas mudanças no navegador, sirva a raiz do repositório:

```
npx serve
```

Todas as exportações do litePlay são globais nesse ambiente, então as funções
podem ser chamadas sem prefixo.

## Onde ficam as coisas
A maior parte do módulo está em `src/core/`:

* `litePlay.js`: o motor de áudio, `toque()`, `pare()`, as classes
  `Instrument` e `Sampler`, listaEventos e sequenciador.
* `extra.js`: geradores e estruturas musicais (acordes, arpejos, ritmos
  euclidianos etc.).
* `litePlay.constants.js`: constantes de notas, sons de percussão e o
  carregador que torna tudo global.
* `litePlay.csd`: as definições de instrumentos do Csound, acionadas pela
  fonte sonora General MIDI, e efeitos sonoros (reverb, envelopes, delay
  etc.).
* `editor.js`: a interface do editor online.

Utilitários de escuta computacional (machine listening) ficam em
`src/listener/`. Eles estão em fase de desenvolvimento (WIP) e podem mudar a
qualquer momento.

## Convenções
Ao implementar novas funcionalidades:

* Adicione comportamentos por instrumento como métodos da classe
  `Instrument`, como foi feito para `pan`, `delay`, `shift` e `autoPan`. Essa
  classe é estendida pela classe Sampler, de modo que tudo o que for
  implementado nela fica disponível para amostras de áudio, sem necessidade
  de duplicar o código.
* Forneça apelidos em português para nomes voltados ao usuário (como `toque`
  para `play` e `pare` para `stop`). Se não puder fazê-lo, peça ajuda no PR.
* Siga o estilo de código existente. O repositório usa ESLint e Prettier;
  execute-os antes de enviar.

## Testes
Execute a suíte de testes com:

```
npm test
```

Os testes usam vitest e importam o módulo com `globalThis.window = {}`
simulado, verificando funções puras como `secs`, `beats`, os geradores e as
operações de listaEventos. Se a sua contribuição adiciona lógica que pode ser
testada sem áudio, por favor inclua testes para ela também.

## Enviando seu trabalho
1. Crie um branch de funcionalidade com um nome curto e descritivo (como nas
   contribuições anteriores `amp`, `delay`, `melody` e `panning`).
2. Faça suas mudanças e adicione testes.
3. Execute `npm test` e certifique-se de que tudo passa.
4. Abra um pull request contra a `main`, referenciando a issue ou descrevendo
   a funcionalidade. Inclua um pequeno exemplo executável, se necessário.

## Documentando a sua contribuição
Uma nova funcionalidade só é considerada completa quando está documentada.
Por favor, atualize este site de documentação:

1. Clone `g-ubimus/litePlay.docs` e prepare o ambiente (veja o README):
   crie um virtualenv, instale os requisitos e execute `zensical serve`.
2. Edite a página relevante sob `docs/`, seguindo o estilo da casa: uma breve
   introdução, a assinatura da função, um exemplo executável e um bloco
   `!!! note` para ressalvas.
3. Escreva a versão em português sob `docs/pt/`. Novamente, você pode pedir
   ajuda.
4. Se uma nova página foi criada, adicione-a às listas de navegação de ambos
   `zensical.toml` e `zensical-pt.toml`.
5. Confira o resultado localmente antes de abrir um pull request.

!!! note
    Pull requests de documentação são publicados automaticamente em
    [g-ubimus.github.io/litePlay.docs](https://g-ubimus.github.io/litePlay.docs/)
    após serem incorporados à `main`.
