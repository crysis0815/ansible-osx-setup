# zshell basics

Rule of thumb: 
- if a non-interactive command should still see it, it is environment → .zprofile. 
- if it only matters while you type, it is interactive → .zshrc.
  

| File        | When it runs | Put here |
| ----------- | ----------- | ----------- |
| ~/.zprofile | Login shells       | "Environment: PATH, Homebrew, EDITOR, language runtimes" |
| ~/.zshrc    | Interactive shells | "Aliases, prompt, completions, setopt, keybindings, plugins" |
| ~/.zshenv   | Interactive shells | "Only tiny, silent env vars. Not PATH on macOS" |


Typical Terminal tab loads:
1. ~/.zshenv
2. /etc/zprofile → macOS path_helper rebuilds PATH
3. ~/.zprofile
4. /etc/zshrc
5. ~/.zshrc
Scripts (zsh script.zsh) usually read only .zshenv. They do not read .zshrc.


| Shell type               | Reads .zprofile | Reads .zshrc |
| ------------------------ | --- | --- |
| Terminal.app / iTerm tab | yes | yes |
| Interactive, not login (many IDE terminals) | no | yes |
| Login, not interactive (some automation, zsh -l script) | yes | no |
| Plain script | no | no |

