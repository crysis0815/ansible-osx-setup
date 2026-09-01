# zshell basics

1. .zshenv - Always loaded first, for every ZSH invocation (interactive, non-interactive, login, non-login)
1. .zprofile - Loaded for login shells only (when you first open Terminal or SSH into a machine)
1. .zshrc - Loaded for interactive shells (when you can type commands)


**Load order (here interactive and login):**
1. ~/.zshenv
2. /etc/zprofile → macOS path_helper rebuilds PATH
3. ~/.zprofile
4. /etc/zshrc
5. ~/.zshrc
   

| Shell type                                             | Reads .zprofile | Reads .zshrc |
| ------------------------                               | ---               | --- |
| Interactive and login (Terminal.app / iTerm tab)        | yes               | yes |
| Interactive, not login (many IDE terminals)            | no               | yes |
| Login, not interactive (some automation, zsh -l script) | yes             | no |
| Plain script                                            | no               | no |


## What to put where
The best .zprofile contains only global environment variables and login-specific initialization that should run once per session

**Rule of thumb:**
- if a non-interactive command should still see it, it is environment => .zprofile. 
- if it only matters while you type, it is interactive => .zshrc.
  

| File        | When it runs | Put here |
| ----------- | ----------- | ----------- |
| ~/.zshenv   | Interactive shells | only tiny, silent env vars. Not PATH on macOS |
| ~/.zprofile | Login shells       | PATH (homebrew, SDKs,...), global environment variables (LANG, EDITOR, XDG directories) and login-specific initialization |
| ~/.zshrc    | Interactive shells | aliases, prompt, completions, setopt, keybindings, plugins |


## my environment

Good copy of what is described in  
- https://towardsthecloud.com/blog/zsh-config#prerequisites
- https://towardsthecloud.com/blog/zsh-aliases

zsh-specific dotfiles:  
- [.zshenv](https://github.com/crysis0815/dotfiles/blob/master/.zshenv)  
- [.zprofile](https://github.com/crysis0815/dotfiles/blob/master/.zprofile)  
- [.zshrc](https://github.com/crysis0815/dotfiles/blob/master/.zshrc)  
- [aliases.zsh](https://github.com/crysis0815/dotfiles/blob/master/.config/zsh/functions/aliases.zsh)  

