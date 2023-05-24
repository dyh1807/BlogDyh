---
title: Use Vi Style Keybinding In Command Line
date: 2023-04-19 01:53:08
tags: vim
---
I've got used to Vi style editing mode for long, but there are always some cases where it's not straightforward to config vim style editing. For instance, in the bash command line.

To edit using the style of vi in command line (before entering Vim/Vi), I add the following content in ~/.bashrc file:

```bash
# set to commandline editing-mode setting
$ set -o vi
$ set editing-mode vi
$ set keymap vi
```

After reopening a bash shell, we are now current in the insert mode of vim. We can type ESC to normal mode, and use "B" "I" "dd" and some easy command in vi, though some complex commands like "/" don's work hitherto.

More Info: [StackExchange](https://unix.stackexchange.com/questions/4870/is-it-possible-to-have-vim-key-bindings-in-terminal)
