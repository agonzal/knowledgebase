# ⬢ ZUI – CGI+DHTML-like User Interface Library for Zsh / ZCurses

![Dharma logos](https://github.com/zdharma/zdharma.github.io/blob/master/static/img/logo_theme.png)

This is a RAD (Rapid Application Development) textual user interface library for Zsh. It in many aspects resembles typical CGI+(D)HTML setup. There are:

* generators ran on "server" side (basic Zshell-code that is just generating text!),
* event loop that turns the generated text into document with active elements (buttons, anchors, toggle buttons, text fields, list boxes),
* mechanism to regenerate document parts from the original generators.

So, a Zshell code generates text. It is then turned into document with hyperlinks. DHTML-like calls are possible that will regenerate document parts on the fly. Page can be also reloaded with input data, just like an HTML page.

# Learning Zsh

ZUI will allow you to learn Zsh at advanced level. The library uses Zshell as e.g. Ruby. To write a
functional program in Ruby, you need to know the language. To write a command or alias in Zsh, you
can spend years not learning anything new. With ZUI you will learn how to use `coproc`, patterns
with `(#b)` flag, Zstyles, arrays, hashes and various substitutions. That said, examples are there
to make the process easy, and problems have easy and advanced way of solving.

# API

The API consists of [Standard Library](https://github.com/zdharma/zui/wiki/Standard-Library),
[Utilities Library](https://github.com/zdharma/zui/wiki/Utilities-Library) and
[Callbacks](https://github.com/zdharma/zui/wiki/Callbacks). You normally want few calls from
Standard Library – to create buttons and regenerate document parts, and one or two callbacks. Fastest
way to learn ZUI is to look at
[Hello World example](https://github.com/zdharma/zui/blob/master/demos/zui-demo-hello-world) and
other [example codes](https://github.com/zdharma/zui/tree/master/demos) like the
[timeout example](https://github.com/zdharma/zui/blob/master/demos/zui-demo-timeout).
