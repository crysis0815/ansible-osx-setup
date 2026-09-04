By default, your shell uses Emacs-style keybindings, but you can change to Vim-like bindings if you prefer.  The following are the commands to change to Vi-bindings (and back to Emacs-bindings).
``
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
