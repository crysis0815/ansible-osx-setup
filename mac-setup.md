# Guide for a new Mac

Mac with fresh OS, follow these steps in order:  

1. [Setup Wizard](#setup-wizard)
1. [AppleID](#appleid)
1. [Preparations](#preparations) (xcode-cli, homebrew, ssh, git)
1. [dotfiles](#install-dotfiles) 
1. [Applications](#install-applications) 
1. [Post-processing](#post-processing) 
1. [Finishing steps](#finishing-steps) 



## Setup Wizard
complete the mandatory macOS setup wizard (creating a local user account, signing into my iCloud account)
   **TODO describe all steps**


## AppleID
sign in to AppleID and AppStore (since `mas` can't sign in automatically)
**TODO syncing iCloud Drive and wait for completion?**


## Preparations


### 1. install Xcode commandline tools

```
xcode-select --install
```


### 2. install and configure homebrew

call the official script to install homebrew in /opt/homebrew with minimal setup for now

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

echo >> ~/.zprofile
echo 'eval "$(/opt/homebrew/bin/brew shellenv zsh)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv zsh)"
```


### 3.  configure ssh

copy [ssh_config](ssh_config) to ~/.ssh/config and set the correct permissions  
**TODO: evtl. direkter link zu dotfile mit endgültiger version?**
```
mv ssh_config ~/.ssh/config
chmod 600 ~/.ssh/config
```

create a github-specific ssh key and add it in github to allow access to repo with additional stuff to install. Replace **TODO** with the hostname of the machine we try to set up.
```
# ssh-agent active?
eval "$(ssh-agent -s)"

# create github keys and add to keychain
ssh-keygen -t ed25519 -C "lothar@TODO" -f ~/.ssh/id_ed25519_github
ssh-add --apple-use-keychain ~/.ssh/id_ed25519_github

# copy ssh key to clipboard and with the browser save the key in github settings (GitHub → Settings → SSH and GPG keys → New SSH key → paste)
pbcopy < ~/.ssh/id_ed25519_github.pub
```


### 4. install and configure git

install git and gh (github-cli)
```
HOMEBREW_NO_VERIFY_ATTESTATIONS=1 brew install git gh
```

copy [git_config](git_config) to ~/.gitconfig and set the correct permissions  
**TODO: evtl. direkter link zu dotfile mit endgültiger version?**
```
mv git_config ~/.gitconfig
chmod 644 ~/.gitconfig
```


## install dotfiles

authenticate in github (use HTTPS/Browser); github ssh/API access is required for repo access and homebrew remote attestation
```
gh auth login
```

test if access is working
```
ssh -T git@github.com
```

clone dotfiles and execute script to install  
**TODO: check permissions und funktionieren, evtl. per skript verschieben und permissions anpassen (.osx?) **
**TODO: evtl. direkter link zu dotfile mit endgültiger version?**  
sourcen?
```
git clone git@github.com:crysis0815/dotfiles.git
```
* synced across machines via git-dotfiles-repo for: public keys, ssh-config, known_hosts
chmod 700 ~/.ssh
chmod 600 ~/.ssh/config
chmod 600 ~/.ssh/id_*
chmod 644 ~/.ssh/*.pub


## install applications

homebrew can install applications from many sources. We use it to install formulae and casks from homebrew and, using mas, directly from the App Store.  
List of all applications installed this way: [Brewfile](https://github.com/crysis0815/dotfiles/blob/master/.config/Brewfile)  

Some applications have to installed manually:
- Brave Browser
- MakeMKV: as unnotarized app we have to manually approve it to skip the Gatekeeper-Check (https://support.apple.com/en-us/102445) . The dmg can be found in MyShare-Software-Archive or on (https://www.makemkv.com/)
- NewsLazer (https://newslazer.com/)
- Trezor Suite (https://trezor.io/de/trezor-suite)

**TODO: describe manual steps?**  

**TODO tailscale/netbird?, mullvad VPN?**


check if there are updates available
```
softwareupdate --list

brew update
brew upgrade
```


## Post-processing

dockutil: add/remove Apps from/to the dock  


aber: viele moderne Settings (Datenschutz, Notifications) gehen nicht zuverlässig per CLI und brauchen ein signiertes Configuration Profile oder MDM. Ggfs. Apple Configurator for mac oder iMazing Profile Editor oder Manuell (XML/plist)

Systemeinstellungen (defaults write etc)
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

TODO: DNS resolver config?


## Finishing steps

1. start syncing

2. configure apps
- open Photos and make sure iCloud sync options are correct TODO
- open Music, make sure computer is authorized, and set Library sync options TODO
- **TODO document manual steps to configure every apps**

3. verify functioning of every app  
**TODO: what to to for every app to verify it works correctly. what can be automazed?**


- Open Calendar and enable personal Google CalDAV account (you have to manually sign in).
- Configure Time Machine backup drive

