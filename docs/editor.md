# litePlay.js editor

The [litePlay.js online editor](https://g-ubimus.github.io/litePlay.js/) is a
small REPL-style environment for running litePlay code. It loads the library,
provides autocompletion and a snippet palette, and can record the result as WAV
and MIDI.

## Starting the engine

The audio engine starts on the first click anywhere in the page. The Run button
turns green and the console prints `litePlay is ready!` when it is available.
If you try to play before that, nothing will be heard.

After the engine is ready, all litePlay and `extra.js` exports are available as
globals, so you can write `play(C4)` instead of `lp.play(C4)`.

## Toolbar

| Button | What it does | Shortcut |
| --- | --- | --- |
| **RUN** | Evaluates the whole editor buffer | `Ctrl+Enter` / `Cmd+Enter` |
| **STOP** | Calls `reset()`, stopping playback and clearing effect state | `Ctrl+.` / `Cmd+.` |
| **SAVE CODE** | Downloads the current buffer as a `.js` file | `Ctrl+S` / `Cmd+S` |
| **REC** | Starts WAV and MIDI recording | |
| **STOP REC** | Stops recording and downloads the files | |
| **ADD SAMPLE** | Loads an audio file into the Csound filesystem | |
| **SNIPPETS** | Shows or hides the snippet palette | |

Run evaluates the whole buffer each time. Keep repeated setup in a function and
call it at the end, so the take can be re-run as a unit:

```javascript
function take() {
  setBpm(120);
  sequencer.clear();
  sequencer.add(drums, [kick, snare, kick, snare], .8, 1);
  sequencer.play();
}
take();
```

## Snippets

The snippet palette contains ready-to-run examples grouped by topic (basics,
pitch and harmony, rhythm, sampling, effects, and so on). You can:

* search the palette by name or description;
* click a card to insert its code at the cursor;
* press the play button on a card to run it immediately.

The snippets are defined in `src/core/snippets.js` in the litePlay.js
repository.

## Uploading samples

Click **ADD SAMPLE** and choose an audio file. The editor writes it into the
Csound filesystem and creates a global variable from the filename: everything
before the first dot, with non-alphanumeric characters replaced by `_`. For
example, `my-drums.wav` becomes `my_drums`, and the console prints the name:

```javascript
my_drums.play();
```

The engine must be running before uploading. See [Sampling](./sampling.md) for
the sample API.

## Recording

**REC** starts two recordings at once:

* a WAV file of the audio output;
* a Standard MIDI File with the scheduled notes and tempo changes.

**STOP REC** stops both and downloads `litePlay_<date>.wav` and
`litePlay_<date>.mid`. If no notes were captured, the MIDI file is skipped.

The MIDI recorder can also be controlled from code:

```javascript
midiRecorder.start();

// ... play something ...

midiRecorder.stop();
midiRecorder.buildAndDownload("take.mid");
```

The MIDI file uses one track per litePlay instrument channel, with drums on the
General MIDI drum channel (channel 10).

## Sending MIDI messages

Two low-level helpers send MIDI directly to Csound:

```javascript
midi(status, data1, data2)
midiProgram(program, channel = 1)
```

For example, to play and stop middle C on channel 1:

```javascript
midi(144, 60, 100); // note on, status 0x90
midi(128, 60, 0);   // note off, status 0x80
```

`midiProgram(0, 1)` selects the first General MIDI program on channel 1.
