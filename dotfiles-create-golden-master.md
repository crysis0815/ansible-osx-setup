# create "golden master"

Step-by-step instructions on how to create a "golden master" of dotfiles, which can be used for the [setup on a new machine](mac-setup.md)).
the "golden master" is hosted in this repo: [dotfiles repo](https://github.com/crysis0815/dotfiles/) 
We create a git bare repo (git dir in $HOME/.dotfiles, working tree is $HOME)and seed it from this initial machine    
Later on we can setup another machine with this repo and keep them in sync 
later on MBP shall be the lead machine
**TODO: describe sync mechanism. allow differences?, top up with OS differentiation of config (add linux)**



### architecture for sync and drift avoidance

**TODO: what to do to keep synced**  
golden master ist welche maschine?  
da 1 person kein concurrency-problem

cron-skript das automatisch prüft ob nicht-commitete changes vorliegen bei bereits getrackten dateien. und ob es installationen gibt die nicht im Brewfile vorhanden sind. und: check ob der lokale und remote git stand abweichen um änderungen von einer maschine zu entdecken. als nightly check um etwas luft zu lassen? falls ja: alarm **TODO: ntfy oder anderen default-alarm-mechanismus an handy/whatsapp/signal whatever**


### Prerequisites  
1. we have a minimal installation and configuration of git, gh and ssh (see [dotfiles repo](https://github.com/crysis0815/dotfiles/) )
1. we have installed all applications (see [Brewfile](https://github.com/crysis0815/dotfiles/blob/main/.config/.lothar/Brewfile) plus description in [mac-setup](mac-setup.md))


# Step-by-step

### 1. create repo on github

```
gh auth login
gh repo create dotfiles --private
```

### 2. create a local git repo in $HOME
we create our local git repo, define an alias to use for dotfiles git operations from now on (will be defined in .zshrc) and add the first files we want to be tracked.
```
cd ~
git init --bare "$HOME/.dotfiles"

alias dot='git --git-dir=$HOME/.dotfiles --work-tree=$HOME'
dot config --local status.showUntrackedFiles no
```

### 3. add local files for the "golden master"
```
cd ~
dot add ~/.config/.lothar/Brewfile
dot add ~/.config/.lothar/bin/*
dot add ~/.config/.lothar/media.yaml
dot add ~/.config/starship.toml
dot add ~/.config/starship-gruvbox.toml
dot add ~/.config/sheldon/plugins.toml
dot add ~/.config/zsh/functions/alias.zsh
dot add ~/.gitignore
dot add ~/.gitconfig
dot add ~/.ssh/config
dot add ~/.vimrc
dot add ~/.zshenv
dot add ~/.zprofile
dot add ~/.zshrc

# check
dot status
dot diff --cached

dot commit -m "initial commit"
```

### 4. connect remote and local repo
push the local git into the github repo  
```
cd ~
dot remote add origin git@github.com:crysis0815/dotfiles.git
dot branch -M main
dot push -u origin main
```


## Sources / Links
- https://stegosaurusdormant.com/bare-git-repo/#initial-setup
- https://coffeeaddict.dev/how-to-manage-dotfiles-with-git-bare-repo/
