By default, your shell uses Emacs-style keybindings, but you can change to Vim-like bindings if you prefer.  The following are the commands to change to Vi-bindings (and back to Emacs-bindings).
```
BASH: set -o vi (set -o emacs)
ZSH : bindkey -v (bindkey -e)
FISH : fish_vi_key_bindings (fish_default_key_bindings)
```


Listed are the Emacs-bindings followed by the Vim-bindings in parentheses:
```
CTRL-a (0 or ^) jump to beginning of line  
CTRL-e ($) jump to end of line  

CTRL-f (h) move forward one char  
CTRL-b (l) move backward one char  

ALT-f (w) move forward one word  
ALT-b (b) move backward one word  
2w, 2b...in Vi-mode  

CTRL-k (d$) delete from this point to the end of the line  
CTRL-u (d0) delete from beginning of the line to this point  
CTRL-y (u) undo  

CTRL-w (db) delete a word backwards  
ALT-d  (dw) delete a word forwards  

Alt-. to past last command argument  
Or just type !!, !$ in the command line  

CTRL-p (k) move to prev command in history  
CTRL-n (j) move to next command in history  

CTRL-l to clear terminal  
CTRL-d to exit the shell  
```

[Mac keyboard shortcuts](https://support.apple.com/en-us/102650)  
[Mac kurzreferenz](https://www.heise.de/tipps-tricks/Mac-Kurzbefehle-Die-wichtigsten-Shortcuts-fuer-macOS-3902321.html)  

[zsh reference](https://zsh.sourceforge.io/Doc/Release/Zsh-Line-Editor.html#Standard-Widgets)  




# Shell Keybindings (https://gist.github.com/2KAbhishek/9c6d607e160b0439a186d4fbd1bd81df)

## Navigation 🚀

| Keybinding                       | Action                                                       |
| -------------------------------- | ------------------------------------------------------------ |
| <kbd>Alt</kbd> + <kbd>f/b</kbd>  | Move cursor to previous/next word                            |
| <kbd>Ctrl</kbd> + <kbd>a/e</kbd> | Move cursor to beginning/end of command                      |
| <kbd>Ctrl</kbd> + <kbd>xx</kbd>  | Toggle between the start of line and current cursor position |

## Editing ✏️

| Keybinding                            | Action                                              |
| ------------------------------------- | --------------------------------------------------- |
| <kbd>Alt</kbd> + <kbd>.</kbd> or `!$` | Previous commands last argument                     |
| <kbd>Alt</kbd> + <kbd>c</kbd>         | Capitalize word                                     |
| <kbd>Alt</kbd> + <kbd>d</kbd>         | Delete next word                                    |
| <kbd>Alt</kbd> + <kbd>Del</kbd>       | Delete previous word                                |
| <kbd>Alt</kbd> + <kbd>l/u</kbd>       | Lowercase/Uppercase word                            |
| <kbd>Alt</kbd> + <kbd>r</kbd>         | Cancel the changes, revert                          |
| <kbd>Alt</kbd> + <kbd>t</kbd>         | Swap current word with previous                     |
| <kbd>Alt</kbd> + <kbd>w</kbd>         | Delete until beginning (zsh)                        |
| <kbd>Ctrl</kbd> + <kbd>\_</kbd>       | Undo                                                |
| <kbd>Ctrl</kbd> + <kbd>k</kbd>        | Cut till end                                        |
| <kbd>Ctrl</kbd> + <kbd>t</kbd>        | Swap the last two characters before the cursor      |
| <kbd>Ctrl</kbd> + <kbd>u</kbd>        | Delete whole line (zsh)/ cut until beginning (bash) |
| <kbd>Ctrl</kbd> + <kbd>w</kbd>        | Cut previous word                                   |
| <kbd>Ctrl</kbd> + <kbd>x,e</kbd>      | Open command in editor                              |
| <kbd>Ctrl</kbd> + <kbd>y</kbd>        | Paste                                               |
| <kbd>Esc</kbd> + <kbd>t</kbd>         | Swap the last two words before the cursor           |
| `!*`                                  | All arguments of previous command                   |

## Process 📊

| Keybinding                     | Action                    |
| ------------------------------ | ------------------------- |
| <kbd>Ctrl</kbd> + <kbd>c</kbd> | Interrupt/Kill            |
| <kbd>Ctrl</kbd> + <kbd>d</kbd> | Close current shell       |
| <kbd>Ctrl</kbd> + <kbd>l</kbd> | Clear screen              |
| <kbd>Ctrl</kbd> + <kbd>z</kbd> | Background/Foreground job |

## History ⏳

| Keybinding                     | Action                                       |
| ------------------------------ | -------------------------------------------- |
| <kbd>Ctrl</kbd> + <kbd>r</kbd> | History search                               |
| <kbd>Ctrl</kbd> + <kbd>s</kbd> | Go back to the next most recent command      |
| `^abc^def`                     | Run previous command, replacing abc with def |

## Modes 🕹️

| Keybinding                       | Action        |
| -------------------------------- | ------------- |
| <kbd>Ctrl</kbd> + <kbd>x,v</kbd> | vi mode (zsh) |
| `bindkey -e`                     | Emacs mode    |
| `set -o vi` / `bindkey -v`       | Vi mode       |

