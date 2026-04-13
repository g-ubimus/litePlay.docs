# Welcome! 
## What is litePlay.js?
litePlay.js is a JavaScript (JS) module for music lite coding. It can be used
in any web applications, but typically we anticipate it to be helpful for
interactive music programming. This may be oriented to performance or
composition (or both). This tutorial is focused on such use cases.

## How to use it?
There are several ways to use litePlay.js, choose what you want to try first:

* [litePlay.js online editor](#litePlay.js-online-editor)

* [Other online environments](#other-online-environments)

* [Load it to your website](#load-it-to-your-website)

### litePlay.js online editor
[litePlay online editor](https://g-ubimus.github.io/litePlay.js/): a simple
editor designed to test experimental features with REPL, where litePlay's
functions can be called without the prefix. It is able to save code and 
record audio. Uploading media files is currently not supported.

If you're choosing this, go ahead and [play()](./play.md).

### Other online environments
There are various online coding environments available for JS development with
different characteristics and support for various kinds of uses. For the
present tutorial, we are looking at the following requirements:

- Allow editing access both to the JS script and the main HTML page code.
- Provide a means of interaction with the code, so it can be typed and executed
  interactively

Not all environments fulfill these. The following three do:

- [JS Playground](https://www.jsplayground.dev/): a lightweight interactive JS
  code platform with a read-eval-print loop (REPL), allowing editing of HTML
  and JS (as well as CSS). No support for saving projects at the time of
  writing.

- [Playcode](https://playcode.io): an interactive JS code platform with the
  facility for live refreshing/re-running of code, but no REPL. Support for
  saving projects in user accounts is available.

- [p5.js editor](https://editor.p5js.org/): an interactive JS platform, which
  is designed for developing p5.js multimedia applications, but supports
  litePlay.js very well. It allows editing of HTML and JS code as well as
  import other media files into the project. It provides a REPL, and it also
  supports saving projects in user accounts.

### Load it to your website 
To load the litePlay.js module into your website, simply add the following
script tag to the main HTML page header:

```
<script  src="https://vlazzarini.github.io/litePlay.js/litePlay.constants.js"></script>
```

When the page is loaded, you will have access to various system constants, as
well as the following function:

```
lpRun();
```

which loads the module, starts the audio engine and tells the user that the
system is ready to use when a message is printed to the JS console.
Alternatively, a function `f()` can be used as the starting point of the
litePlay.js application. Or it can simply contain all the code that is to be
executed when the script is run:

```
function f() {
 // we start executing from here ...
}
lpRun(f);
```

Once the litePlay.js engine is running, we can use its functionality anywhere
by prefixing its functions etc with `lp.`.
