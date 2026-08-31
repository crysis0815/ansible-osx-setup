# Guide for a new Mac

Mac with fresh OS, follow these steps in order:  

1. Complete Setup Wizard [Link Text](#setup-wizard)
2. Sign into App Store plus AppleID?
3. Preparations (xcode-cli, ssh, git)
4. dotfiles
5. **TODO**


## Setup Wizard
1. complete the mandatory macOS setup wizard (creating a local user account, signing into my iCloud account)
   **TODO describe all steps**
2. sign in to AppStore (since `mas` can't sign in automatically)
3. **TODO syncing iCloud Drive and wait for completion?**
4. install Xcode commandline tools
```
xcode-select --install
```
4.5 configure ssh  
**TODO: ssh_config**
```
# ssh-agent active?
eval "$(ssh-agent -s)"
# create github keys and add to keychain
ssh-keygen -t ed25519 -C "lothar@TODO-hostname" -f ~/.ssh/id_ed25519_github
ssh-add --apple-use-keychain ~/.ssh/id_ed25519_github

# add ssh key to clipboard and then paste via browser in github settings
pbcopy < ~/.ssh/id_ed25519_github.pub
GitHub → Settings → SSH and GPG keys → New SSH key → paste.

# test
ssh -T git@github.com
git clone git@github.com:crysis0815/dotfiles.git
```
5. install homebrew (official script to /opt/homebrew)
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

echo >> /Users/lothar/.zprofile
echo 'eval "$(/opt/homebrew/bin/brew shellenv zsh)"' >> /Users/lothar/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv zsh)"
```
6. install git
```
HOMEBREW_NO_VERIFY_ATTESTATIONS=1 brew install git gh
```
7. configure git
```
[user]
	name = crysis0815
	email = microsoft@capesso.de
[credential]
	helper = osxkeychain
[color]
	ui = true
[alias]
	ll = log --oneline --graph --decorate --all
	l = log --oneline --decorate
[format]
	pretty = oneline
[core]
	excludesfile = /Users/lothar/.gitignore_global
```
8. authenticate in github (nexessary for brew remote attestation API access)
Use HTTPS/Browser as ssh is not configured yet
```
gh auth login
```
9. install dotfiles from git repo
```
clone dotfiles to the Mac: `git clone git@github.com:crysis0815/dotfiles.git`

TODO: check permissions und funktionieren, evtl. per skript verschieben und permissions anpassen (.osx?)
```
* synced across machines via git-dotfiles-repo for: public keys, ssh-config, known_hosts
chmod 700 ~/.ssh
chmod 600 ~/.ssh/config
chmod 600 ~/.ssh/id_*
chmod 644 ~/.ssh/*.pub


10.**TODO install all applications**  
Applications can be installed with homebrew from the AppStore, brew (formulae and casks) or manually. The latter should be avoided whenever possible.  
Automagically installable with homebrew: [Brewfile](https://github.com/crysis0815/dotfiles/blob/master/.config/Brewfile)  
Some applications have to installed manually:  
- Brave Browser
- MakeMKV: dmg in MyShare-Software-Archive. As it is an unnotarized app, have to skip the Gatekeeper-Check (https://support.apple.com/en-us/102445)  
- NewsLazer (https://newslazer.com/
- Trezor Suite.app

11. Postprocessing, add to Dock  
dockutil: add/remove Apps from/to the dock  

## Systemeinstellungen (defaults write etc)
shell script used in dotfiles repositories to automate the configuration of macOS system preferences and defaults  
```
home/.osx, chmod +x ~/.osx
```
https://github.com/olafhartong/osx-defaults/blob/master/.osx  
https://github.com/alberti42/macOS-config-manager (defaults backup/restore)  

systemeinstellungen per CLI:
* defaults write – die meisten App- und System-Prefs
* systemsetup – Host, Timezone, Remote Login (braucht oft sudo)
* osascript – System Settings, die kein defaults-Key haben
* pmset, networksetup, scutil – Power, Netzwerk, Hostname
* https://github.com/kevinSuttle/macOS-Defaults/blob/master/REFERENCE.md
* launchctl, mdutil for spotlight?, systemsetup, tmutil

* softwareupdate für systemsoftware (man softwareupdate; evtl skripten?)  

aber: viele moderne Settings (Datenschutz, Notifications) gehen nicht zuverlässig per CLI und brauchen ein signiertes Configuration Profile oder MDM. Ggfs. Apple Configurator for mac oder iMazing Profile Editor oder Manuell (XML/plist)


TODO: DNS resolver config?
tailscale/netbird?, mullvad VPN?



	

## Post-processing steps

  - Start Synchronization tasks:
    - Open Photos and make sure iCloud sync options are correct TODO
    - Open Music, make sure computer is authorized, and set Library sync options TODO
    - else?

apps konfigurieren, gfs manuelle steps, funktionsfähigkeit prüfen (ggfs. skriptbar?)
TODO: dokumentieren

  - Open Calendar and enable personal Google CalDAV account (you have to manually sign in).
  - Manually copy `~/Development` folder from another Mac (to save time).
  - Manual settings to automate someday:
    - Finder:
      - Disable click-to-show Desktop: `defaults write com.apple.WindowManager EnableStandardClickToShowDesktop -bool false`
    - System Preferences:
      - Accessibility > Display > Reduce transparency
      - Keyboard > Keyboard Shortcuts... > Modifier Keys... > Caps Lock to Esc
      - Keyboard > Key repeat rate to 'Fast', Delay until repeat to 'Short'
      - Privacy & Security > Full Disk Access > enable "Terminal"
    - Safari:
      - View > Show Status Bar
      - Preferences > Advanced > "Show full website address"
      - Preferences > Advanced > "Show features for web developers"
      - Install the 'Return YouTube Dislike' Userscript in Userscripts

  - These things might be automatable, but I do them manually right now:
    - Configure Time Machine backup drive
    - Install Wireguard VPN configurations (if needed


## To Wrap in Post-provision automation

The following tasks have to wait for the initial Dropbox sync to complete before they'll succeed. So ideally I'll stick this all in a post-provision script but somehow flag it not to run on first provision.

```
# ZSH Aliases.
ln -s /Users/jgeerling/Dropbox/Apps/Config/.aliases /Users/jgeerling/.aliases

# SSH setup.
ssh-keygen  # and create a default key to set up .ssh folder
sudo ln -s /Users/jgeerling/Dropbox/Apps/Config/ssh/config ~/.ssh/config
# TODO - Manually copy any shared SSH keys that are needed.


# Vim setup.
mkdir -p ~/.vim/autoload
mkdir -p ~/.vim/bundle
cd ~/.vim/autoload
curl https://raw.githubusercontent.com/tpope/vim-pathogen/master/autoload/pathogen.vim > pathogen.vim
cd ~/.vim/bundle
git clone https://github.com/preservim/nerdtree.git
```

## Erasing a Mac

  - Sign out of tailscale
  - Deauthorize Apple Music in iTunes/Music App
  - **TODO: what else?**
  - Follow Apple's guide [here](https://support.apple.com/en-au/HT212749)


