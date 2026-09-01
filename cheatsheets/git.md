# KnowledgeBase / Cheatsheet for git

## archive a git repository

To create a full, standalone archive of a repository (including all branches, tags, commit history, and remote ref headers), use a mirror clone.  
```
git clone --mirror https://github.com/username/repository.git repository-backup.git
tar -czvf repository-backup.tar.gz repository-backup.git
```  

**To restore:** Uncompress the file, navigate into the directory, and run git push --mirror

## automate dotfiles in $HOME

Worktree ist $HOME

cd ~
git init
git config --local status.showUntrackedFiles no
create ~/.gitignore
```
cd ~
git add ~/.config/.lothar/*
git add ~/.config/starship.toml
# TODO: gh, rclone, zed
git add ~/.gitconfig
git add ~/.gitignore
git add ~/.gitignore_global
git add ~/.ssh/config
git add ~/.vimrc
git add ~/.zprofile
git add ~/.zshrc
```
git commit -m "initial commit"

script to fix permissions after checkout (as git doesnt store file permissions): .config/.lothar/sbin/git-dotfiles-fixpermissions.sh

git hooks call the script to fix permissions
```
cat > ~/.git/hooks/post-checkout <<'EOF'
#!/bin/zsh
exec "$HOME/bin/fix-dot-perms"
EOF
cp ~/.git/hooks/post-checkout ~/.git/hooks/post-merge
chmod +x ~/.git/hooks/post-checkout ~/.git/hooks/post-merge
```

Create a private repository dotfiles on github, manually

auf github pushen
```
cd ~
git remote add origin git@github.com:crysis0815/dotfiles.git
git branch -M main
git push -u origin main
```

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
mv ~/.zshrc ~/.gitconfig ~/.gitignore ~/.dotfiles-backup/ 2>/dev/null || true
git checkout main

chmod +x ~/bin/fix-dot-perms
# Hooks wie in Schritt 5 anlegen
~/bin/fix-dot-perms
```

