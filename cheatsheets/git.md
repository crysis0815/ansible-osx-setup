# KnowledgeBase / Cheatsheet for git

## archive a git repository

To create a full, standalone archive of a repository (including all branches, tags, commit history, and remote ref headers), use a mirror clone.  
```
git clone --mirror https://github.com/username/repository.git repository-backup.git
tar -czvf repository-backup.tar.gz repository-backup.git
```  

**To restore:** Uncompress the file, navigate into the directory, and run git push --mirror



## create a dotfiles repository

We want to add the configuration from our $HOME directory (Worktree ist $HOME, not a subdir) to a remote repository on github.  
Step-by-Step:  
### create repo on github
Create a repository on github, manually. Use the name dotfiles and set the repo to private.

### create a local git repo in $HOME
```
cd ~  
git init  
git config --local status.showUntrackedFiles no
```
create [~/.gitignore](https://github.com/crysis0815/dotfiles/blob/master/.gitignore) 

### create git hooks
create [~/.config/.lothar/bin/git-dotfiles-fixpermissions.sh] and set its permissions: chmod +x (https://github.com/crysis0815/dotfiles/blob/master/.config/.lothar/bin/git-dotfiles-fixpermissions.sh)   

upon checkout of the dotfiles from the remote repo we have to run hook scripts to fix local file permissions  
```
cat > ~/.git/hooks/post-checkout <<'EOF'
#!/bin/zsh
exec "$HOME/.config/.lothar/bin/git-dotfiles-fixpermissions.sh"
EOF
cp ~/.git/hooks/post-checkout ~/.git/hooks/post-merge
chmod +x ~/.git/hooks/post-checkout ~/.git/hooks/post-merge
```

### add the local master from $HOME
add all files to [dotfiles repo](https://github.com/crysis0815/dotfiles/) 
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

### add remote repo and push the local state
auf github pushen
```
cd ~
git remote add origin git@github.com:crysis0815/dotfiles.git
git branch -M main
git push -u origin main
```

### how to keep up2date

**TODO: what to do to keep synced**  
golden master ist welche maschine?  
da 1 person kein concurrency-problem


## New Mac

```
cd ~
git init
git remote add origin git@github.com:crysis0815/dotfiles.git
git fetch origin
git config --local status.showUntrackedFiles no

#checkout testen
git checkout main

# Wenn vorhandene Dateien im Weg sind:
mkdir -p ~/.dotfiles-backup
# die im Fehler genannten Dateien:
mv ~/.zprofile ~/.gitconfig ~/.ssh/config .dotfiles-backup/ 2>/dev/null || true
git checkout main

# once we have to set the permissions manually
chmod +x ~/.config/.lothar/bin/git-dotfiles-fixpermissions.sh
.config/.lothar/bin/git-dotfiles-fixpermissions.sh
```
### create git hooks
we also have to create the hooks to fix permissions on this machine
```
cat > ~/.git/hooks/post-checkout <<'EOF'
#!/bin/zsh
exec "$HOME/.config/.lothar/bin/git-dotfiles-fixpermissions.sh"
EOF
cp ~/.git/hooks/post-checkout ~/.git/hooks/post-merge
chmod +x ~/.git/hooks/post-checkout ~/.git/hooks/post-merge
```

