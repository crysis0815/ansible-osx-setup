# create "golden master"

Step-by-step instructions on how to create a "golden master" of dotfiles to be used for the setup on a new machine.  
Assume we work in the $HOME directory of a Mac we consider the "golden master"-origin to seed selected files from its $HOME directory (Worktree is $HOME, not a subdirectory!) to a remote repository on github.  

## create repo on github
Manually create a private repository named dotfiles on github using the browser webUI.

## create a local git repo in $HOME
```
cd ~  
git init  
git config --local status.showUntrackedFiles no
```
create [~/.gitignore](https://github.com/crysis0815/dotfiles/blob/master/.gitignore) 

## create git hooks
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
git add ~/.config/.lothar/Brewfile
git add ~/.config/.lothar/bin/*
git add ~/.config/.lothar/media.yaml
git add ~/.config/starship.toml
git add ~/.config/zsh/functions/alias.zsh
git add ~/.ssh/config
git add ~/.gitconfig
git add ~/.gitignore
git add ~/.gitignore_global
git add ~/.vimrc
git add ~/.zprofile
git add ~/.zshenv
git add ~/.zshrc
...
git commit -m "initial commit"
```

## connect remote repo and local $HOME git
push the local git into the github repo  
```
cd ~
git remote add origin git@github.com:crysis0815/dotfiles.git
git branch -M main
git push -u origin main
```

## how to keep up2date

**TODO: what to do to keep synced**  
golden master ist welche maschine?  
da 1 person kein concurrency-problem
