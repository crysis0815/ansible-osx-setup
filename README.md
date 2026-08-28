# Full Mac Setup Process

Recipe for erasing and setting up a mac.
100% automation of this process isn't possible (not with Time Machine, nor with ASR, nor with Ansible), thus some manual steps are required. This document tries to cover that,

## When formatting old Mac

  - Sign out of tailscale, any others too?
  - Deauthorize Apple Music in iTunes/Music App
  - Follow Apple's guide [here](https://support.apple.com/en-au/HT212749)



## Initial configuration of a brand new Mac

Before starting, I completed Apple's mandatory macOS setup wizard (creating a local user account, signing into my iCloud account). 
TODO: syncing iCloud Drive and wait for completion?
Once on the macOS desktop, I do the following (in order):

There's a simple shell script in `bin/bootstrap` which will perform the initial steps of:

1. Installing Xcode
2. Installing TODO


installation homebrew und configuration von homebrew
install casks/packages (from Brewfile?)
install dotfiles from git repo
TODO: list files?, wie commit wenn änderungen?
dotfiles_files:
  - .zshrc
  - .gitignore
  - .inputrc
  - .vimrc

systemeinstellungen (defaults write etc)
  shell script used in dotfiles repositories to automate the configuration of macOS system preferences and defaults
  home/.osx, chmod +x ~/.osx

DNS resolver config?
tailscale?
asdf?

installation apps aus Mac App Store und anderen Quellen TODO auflisten?
  https://github.com/mas-cli/mas, via homebrew installieren
apps konfigurieren, gfs manuelle steps

dockutil: add/remove Apps from/to the dock



  - Install Ansible (following the guide in [README.md](README.md))
  - **Sign in to App Store** (since `mas` can't sign in automatically)
  - Clone mac-dev-playbook to the Mac: `git clone git@github.com:geerlingguy/mac-dev-playbook.git`
  - Drop `config.yml` from `~/Dropbox/Apps/Config` to the playbook (copy over the network or using a USB flash drive).
  - Run the playbook.
    - If there are errors, you may need to finish up other tasks like installing 'old-fashioned' apps first. Then, run the playbook again ;)
  - Start Synchronization tasks:
    - Open Photos and make sure iCloud sync options are correct TODO
    - Open Music, make sure computer is authorized, and set Library sync options TODO
    - Open Dropbox, sign in, and set up sync
  - Install or complete setup for old-fashioned apps:
    - Open Creative Cloud, sign in, and install needed apps
    - Open iStat Menus and configure CPU/Net/Temp Combined view
    - TODO?
  - Configure FastMail account:
    - Log into Fastmail
    - Go to settings, then Privacy & Security
    - Create a new app password, and on that page, download the configuration file
    - Open the downloaded profile, then go to System Preferences, and Device Management
    - Double-click on the Fastmail profile
    - Click 'Install...' and install it
    - Configure which accounts are enabled in the 'Internet Accounts' System Preferences pane
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
    - Dock:
      - Add jgeerling, Downloads, Applications, and shared "mercury" folders
  - Final Cut Pro
    - Install FxFactory and sign in: https://fxfactory.com
    - Copy contents of `~/Development/youtube/fcpx` into respective directories
  - These things might be automatable, but I do them manually right now:
    - Configure Time Machine backup drive
    - Install Wireguard VPN configurations (if needed)

## To Wrap in Post-provision automation

The following tasks have to wait for the initial Dropbox sync to complete before they'll succeed. So ideally I'll stick this all in a post-provision script but somehow flag it not to run on first provision.

```
# ZSH Aliases.
ln -s /Users/jgeerling/Dropbox/Apps/Config/.aliases /Users/jgeerling/.aliases

# Electrum BTC Wallet (open Electrum first).
ln -s /Users/jgeerling/Dropbox/Apps/Electrum/default_wallet /Users/jgeerling/.electrum/wallets/default_wallet

# SSH setup.
ssh-keygen  # and create a default key to set up .ssh folder
sudo ln -s /Users/jgeerling/Dropbox/Apps/Config/ssh/config ~/.ssh/config
# TODO - Manually copy any shared SSH keys that are needed.

# Ansible setup.
sudo mkdir -p /etc/ansible
sudo ln -s /Users/jgeerling/Dropbox/Apps/Config/ansible/ansible.cfg /etc/ansible/ansible.cfg
sudo ln -s /Users/jgeerling/Dropbox/Apps/Config/ansible/hosts /etc/ansible/hosts
sudo ln -s /Users/jgeerling/Dropbox/VMs/roles /etc/ansible/roles
mkdir -p /Users/jgeerling/.ansible
ln -s /Users/jgeerling/Dropbox/Apps/Config/ansible/galaxy_token /Users/jgeerling/.ansible/galaxy_token
ln -s /Users/jgeerling/Dropbox/Apps/Config/ansible/mm-vault-password.txt /Users/jgeerling/.ansible/mm-vault-password.txt
ln -s /Users/jgeerling/Dropbox/VMs/ansible_collections /Users/jgeerling/.ansible/collections

# Final Cut Pro setup. (Open Motion first)
cp -r /Users/jgeerling/Dropbox/Apps/Config/Motion/Motion\ Templates.localized/ /Users/jgeerling/Movies/Motion\ Templates.localized/
cp -r /Users/jgeerling/Dropbox/Apps/Config/Motion/Text\ Styles/ /Users/jgeerling/Library/Application\ Support/Motion/Library/Text\ Styles.localized/

# Sequel Ace favorites. (Open Sequel Ace first)
cp /Users/jgeerling/Dropbox/Apps/Config/Sequel\ Ace/Favorites.plist /Users/jgeerling/Library/Containers/com.sequel-ace.sequel-ace/Data/Library/Application\ Support/Sequel\ Ace/Data/Favorites.plist

# Font setup.
cp ~/Dropbox/Apps/Config/Fonts/* ~/Library/Fonts/

# Vim setup.
mkdir -p ~/.vim/autoload
mkdir -p ~/.vim/bundle
cd ~/.vim/autoload
curl https://raw.githubusercontent.com/tpope/vim-pathogen/master/autoload/pathogen.vim > pathogen.vim
cd ~/.vim/bundle
git clone https://github.com/preservim/nerdtree.git
```

