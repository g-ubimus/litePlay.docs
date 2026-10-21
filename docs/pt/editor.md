# Editor do litePlay.js

O [editor online do litePlay.js](https://g-ubimus.github.io/litePlay.js/) é um
ambiente pequeno, no estilo REPL, para executar código litePlay. Ele carrega a
biblioteca, oferece autocompletar e uma paleta de snippets, além de gravar o
resultado em WAV e MIDI.

## Iniciando o motor

O motor de áudio inicia no primeiro clique em qualquer lugar da página. O botão
Run fica verde e o console imprime `litePlay is ready!` quando ele está
disponível. Se você tentar tocar antes disso, nada será ouvido.

Depois que o motor estiver pronto, todas as exportações do litePlay e do
`extra.js` ficam disponíveis como globais, então você pode escrever
`play(C4)` em vez de `lp.play(C4)`.

## Barra de ferramentas

| Botão | O que faz | Atalho |
| --- | --- | --- |
| **RUN** | Avalia todo o conteúdo do editor | `Ctrl+Enter` / `Cmd+Enter` |
| **STOP** | Chama `reset()`, parando a reprodução e limpando os efeitos | `Ctrl+.` / `Cmd+.` |
| **SAVE CODE** | Baixa o conteúdo atual como um arquivo `.js` | `Ctrl+S` / `Cmd+S` |
| **REC** | Inicia a gravação em WAV e MIDI | |
| **STOP REC** | Para a gravação e baixa os arquivos | |
| **ADD SAMPLE** | Carrega um arquivo de áudio no sistema de arquivos do Csound | |
| **SNIPPETS** | Mostra ou esconde a paleta de snippets | |

Run avalia todo o buffer a cada execução. Mantenha a preparação repetida dentro
de uma função e chame-a no final, para poder executar o take como uma unidade
de novo:

```javascript
function take() {
  setBpm(120);
  sequenciador.limpar();
  sequenciador.adicionar(bateria, [kick, snare, kick, snare], .8, 1);
  sequenciador.toque();
}
take();
```

## Snippets

A paleta de snippets contém exemplos prontos, agrupados por tema (básico,
altura e harmonia, ritmo, sampling, efeitos etc.). Você pode:

* pesquisar a paleta por nome ou descrição;
* clicar em um cartão para inserir o código no cursor;
* pressionar o botão de reprodução do cartão para executá-lo imediatamente.

Os snippets são definidos em `src/core/snippets.js` no repositório do
litePlay.js.

## Enviando amostras

Clique em **ADD SAMPLE** e escolha um arquivo de áudio. O editor o escreve no
sistema de arquivos do Csound e cria uma variável global a partir do nome do
arquivo: tudo antes do primeiro ponto, com caracteres não alfanuméricos
substituídos por `_`. Por exemplo, `my-drums.wav` vira `my_drums`, e o console
imprime o nome:

```javascript
my_drums.toque();
```

O motor precisa estar rodando antes do upload. Veja [Sampling](./sampling.md)
para a API de amostras.

## Gravação

**REC** inicia duas gravações ao mesmo tempo:

* um arquivo WAV da saída de áudio;
* um arquivo MIDI padrão com as notas agendadas e as mudanças de andamento.

**STOP REC** para as duas e baixa `litePlay_<data>.wav` e
`litePlay_<data>.mid`. Se nenhuma nota foi capturada, o arquivo MIDI é omitido.

O gravador MIDI também pode ser controlado pelo código:

```javascript
midiRecorder.start();

// ... toque algo ...

midiRecorder.stop();
midiRecorder.buildAndDownload("take.mid");
```

O arquivo MIDI usa uma faixa por canal de instrumento do litePlay, com a bateria
no canal de percussão General MIDI (canal 10).

## Enviando mensagens MIDI

Dois auxiliares de baixo nível enviam MIDI diretamente ao Csound:

```javascript
midi(status, data1, data2)
midiProgram(program, channel = 1)
```

Por exemplo, para tocar e parar o Dó central no canal 1:

```javascript
midi(144, 60, 100); // note on, status 0x90
midi(128, 60, 0);   // note off, status 0x80
```

`midiProgram(0, 1)` seleciona o primeiro programa General MIDI no canal 1.
