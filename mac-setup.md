# Guide for the setup of a new Mac

**Prerequisite:**  
we have created a "golden master" of dotfiles as described in [dotfiles-create-golden-master.md](dotfiles-create-golden-master.md) and have a Mac with a fresh OS installation. Then follow these steps:  
1. [Setup Wizard](#setup-wizard)
1. [Preparations](#preparations) (xcode-cli, hostname, homebrew, ssh, git)
1. [dotfiles](#install-dotfiles) 
1. [Applications](#install-applications) 
1. [Post-processing](#post-processing) 
1. [Finishing steps](#finishing-steps) 



## Setup Wizard
complete the mandatory macOS setup wizard  
- create local user account lothar with descriptor LH (allow AppleID password reset mechanism)
- sign in to iCloud with AppleID (necessary for software installation with homebrew/mas)

**hint:** if we (test)-install in a VM, install the Guest Tools to enable copy & paste  


## 1. preparations
install Xcode commandline tools and set the hostname in  
- set system settings->Allgemein->Name to hostname
- replace **TODO** with hostname

```
xcode-select --install

# ComputerName (displayed in System Settings > General > About)
# LocalHostName (displayed in System Settings > General > Sharing, used for Bonjour/mDNS)
# HostName (used by standard Unix networking and the `hostname` shell command)
sudo scutil --set ComputerName "TODO"
sudo scutil --set LocalHostName "TODO"
sudo scutil --set HostName "TODO"

typeset -x HOSTNAME="$(hostname)"
```


## 2. install and configure homebrew

open a terminal and call the official script to install homebrew in /opt/homebrew with minimal setup for now

```
cd ~
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

echo >> ~/.zprofile
echo 'eval "$(/opt/homebrew/bin/brew shellenv zsh)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv zsh)"
```


## 3.  configure ssh

copy the [ssh configuration file](https://github.com/crysis0815/dotfiles/blob/master/.ssh/config)  to ~/.ssh/config and set the correct permissions
```
cd ~
mkdir -p ~/.ssh
chmod 700 ~/.ssh
touch ~/.ssh/config
# TODO: paste config in ~/.ssh/config
chmod 600 ~/.ssh/config
```

create a github-specific ssh key and add it in github to allow access to repo with additional stuff to install.
```
# ssh-agent should be active
eval "$(ssh-agent -s)"

# create github keys and add to keychain
ssh-keygen -t ed25519 -C "lothar@${HOSTNAME}" -f ~/.ssh/id_ed25519_github
ssh-add --apple-use-keychain ~/.ssh/id_ed25519_github
```


## 4. install and configure git

install git and gh (github-cli)
```
HOMEBREW_NO_VERIFY_ATTESTATIONS=1 brew install git gh rsync
```

copy the [git configuration file](https://github.com/crysis0815/dotfiles/blob/master/.gitconfig)  to ~/.gitconfig and set the correct permissions
```
cd ~
touch ~/.gitconfig
# TODO: paste config in ~/.gitconfig
chmod 644 ~/.gitconfig
```

login to github and add this machines ssh key  
**(hint:** github.com, ssh, select new ssh key, title=hostname, web browser with one-time code given in output)
```
gh auth login

# only if key wasnt added already
# gh ssh-key add ~/.ssh/id_ed25519.pub

# test if access works (hint: yes)
ssh -T git@github.com
```


## install dotfiles

**ACHTUNG: erst wenn alle apps installiert sind (wie sheldon etc) ist Funktionsfähigkeit gegeben, sonst system hang**
**TODO: test with app installation before dotfile sync**

initially clone the dotfiles repo as a non-bare repository into a temporary directory to avoid file collisions. then just once rsync the dotfiles to their proper location in $HOME and delete the temporary directory.  
At this point, your new machine has the dotfiles in the correct locations in your home directory and is tracking their history in ~/.dotfiles, which is exactly the same state that your original machine.
```
cd ~
git clone --separate-git-dir=$HOME/.dotfiles git@github.com:crysis0815/dotfiles.git .dotfiles-tmp

rsync --recursive --verbose --exclude '.git' .dotfiles-tmp/ $HOME/
rm -rf .dotfiles-tmp
```


## install applications

homebrew can install applications from many sources. We use it to install formulae and casks from homebrew and, using mas, directly from the App Store.  
We call homebrew to install all applications listed in [Brewfile](https://github.com/crysis0815/dotfiles/blob/master/.config/.lothar/Brewfile) 
**TODO: mit --verbose ausprobieren?**
```
brew bundle install --file ~/.config/.lothar/Brewfile
```

**TODO: check ob die probleme weiter bestehen bei clean install:**
terminal braucht in datenschutz irgendeine permission (system settings)  
	dito xcode mit PW in shell und touchid  
ausweisapp braucht rosettaapp und frägt nach PW  
zed frägt nach passwort (irgend xattr?)  
* weasis, utm, makemkv wollen das passwort. warum und damit hinfällig oder in VM?  (nur auf macstudio???)  
Error: You have not agreed to the Xcode license. Please resolve this by running:  
  sudo xcodebuild -license accept  
sudo softwareupdate --install-rosetta  


Some applications have to installed manually:
- Brave Browser
- MakeMKV: as unnotarized app we have to manually approve it to skip the Gatekeeper-Check (https://support.apple.com/en-us/102445) . The dmg can be found in MyShare-Software-Archive or on (https://www.makemkv.com/)
- NewsLazer (https://newslazer.com/)
- Trezor Suite (https://trezor.io/de/trezor-suite)

**TODO: describe manual steps?**  

**recreate the dock**
```
~/.config/.lothar/bin/setup_dock-helper.sh
```


## Post-processing

rm -f .dotfiles-backup

Populate the dock
```
.config/.lothar/bin/setup_dock-helper.sh
```
 
**TODO: macos-skript bauen**

Temrinal: font auf jetbrains setzen und textgrösse 18

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

4. **TODO: add relevant files to dotfiles repo and update these guides**

* sync across machines via git-dotfiles-repo for: public keys?
pen Calendar and enable personal Google CalDAV account (you have to manually sign in).
- Configure Time Machine backup drive

setup sync/backup wesentlicher dateien zwischen den rechnern, git repo?

**TODO tailscale/netbird?, mullvad VPN?**
