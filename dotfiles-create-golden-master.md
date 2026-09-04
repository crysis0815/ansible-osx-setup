# create "golden master"

Step-by-step instructions on how to create a "golden master" of dotfiles, which can be used for the setup on a new machine.  

We create a git bare repo (git dir in $HOME/.dotfiles, working tree is $HOME)and seed it from an initial machine.  
Later on we can setup a new machine with this repo and keep them in sync 
later on MBP shall be the lead machine
**TODO: describe sync mechanism. allow differences?, top up with OS differentiation of config (add linux)**

Prerequisites:  
1. we have a minimal installation and configuration of git, gh and ssh on the machine **TODO: xcode-select?, link setup-guide**
1. we have installed all applications **TODO: link setup-guide**


## create repo on github

```
gh auth login
gh repo create dotfiles --private
```


## create a local git repo in $HOME
we create our local git repo, define an alias to use for dotfiles git operations from now on (will be defined in .zshrc) and add the first files we want to be tracked.
```
cd ~
git init --bare "$HOME/.dotfiles"

alias dot='git --git-dir=$HOME/.dotfiles --work-tree=$HOME'
dot config --local status.showUntrackedFiles no
dot add ~/.gitconfig
dot add ~/.ssh/config
dot add ~/.zshenv
dot add ~/.zprofile
dot add ~/.zshrc
```

## install applications
**TODO: klappt das mit der minimalen installation?**
copy Brewfile and call brew bundle install --file Brewfile


## create git hooks

**TODO: hier bootstrap-skript das dateien wegsichert beim ersten lauf, rsync zum überschreiben und permissions fixen**  
**oder: alias der in dot auch den skriptaufruf enthält statt hooks?**

create the script [~/.config/.lothar/bin/git-dotfiles-fixpermissions.sh] and set its permissions: chmod +x (https://github.com/crysis0815/dotfiles/blob/master/.config/.lothar/bin/git-dotfiles-fixpermissions.sh)   

upon checkout of the dotfiles from the remote repo we have to run hook scripts to call this script to fix local file permissions  
```
cat > ~/.git/hooks/post-checkout <<'EOF'
#!/bin/zsh
exec "$HOME/.config/.lothar/bin/git-dotfiles-fixpermissions.sh"
EOF
cp ~/.git/hooks/post-checkout ~/.git/hooks/post-merge
chmod +x ~/.git/hooks/post-checkout ~/.git/hooks/post-merge
```

## add local files for the "golden master"
the "golden master" is hosted in this repo: [dotfiles repo](https://github.com/crysis0815/dotfiles/) 
```
cd ~
# TODO: mit *?
dot add ~/.config/.lothar/Brewfile
dot add ~/.config/.lothar/bin/*
dot add ~/.config/.lothar/media.yaml
dot add ~/.config/starship.toml
dot add ~/.config/zsh/functions/alias.zsh
dot add ~/.gitignore
dot add ~/.vimrc

# check
dot status
dot diff --cached

dot commit -m "initial commit"
```

## connect remote repo and local $HOME git
push the local git into the github repo  
```
cd ~
dot remote add origin git@github.com:crysis0815/dotfiles.git
dot branch -M main
dot push -u origin main
```

## how to keep up2date

**TODO: what to do to keep synced**  
golden master ist welche maschine?  
da 1 person kein concurrency-problem

cron-skript das automatisch prüft ob nicht-commitete changes vorliegen bei bereits getrackten dateien. und ob es installationen gibt die nicht im Brewfile vorhanden sind. und: check ob der lokale und remote git stand abweichen um änderungen von einer maschine zu entdecken. als nightly check um etwas luft zu lassen? falls ja: alarm **TODO: ntfy oder anderen default-alarm-mechanismus an handy/whatsapp/signal whatever**


## Sources / Links
- https://stegosaurusdormant.com/bare-git-repo/#initial-setup
- https://coffeeaddict.dev/how-to-manage-dotfiles-with-git-bare-repo/
