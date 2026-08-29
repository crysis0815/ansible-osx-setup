# Full Mac Setup Process

Recipe for erasing and setting up a mac.
100% automation of this process isn't possible (not with Time Machine, nor with ASR, nor with Ansible), thus some manual steps are required. This document tries to cover that,

![placeholder](non-transferable%20configuration.jpg)


## When formatting old Mac

  - Sign out of tailscale, any others too?
  - Deauthorize Apple Music in iTunes/Music App
  - Follow Apple's guide [here](https://support.apple.com/en-au/HT212749)



## Initial configuration of a brand new Mac

Before starting, I completed Apple's mandatory macOS setup wizard (creating a local user account, signing into my iCloud account). 
TODO: syncing iCloud Drive and wait for completion?
**Sign in to App Store** (since `mas` can't sign in automatically)

Once on the macOS desktop, I do the following (in order):

There's a simple shell script in `bin/bootstrap` which will perform the initial steps of:

Installing Xcode

installation homebrew und configuration von homebrew
.bashrc/.zshrc ergänzen und sourcen

install casks/packages (from Brewfile, incl. AppStore via https://github.com/mas-cli/mas)
brew bundle with casks, taps, Mac App Store apps, WinGet packages on WSL, VS Code extensions, Go packages, Cargo packages, npm packages, uv tools, Flatpak packages and krew kubectl plugins and to start background services with brew services. https://docs.brew.sh/Brew-Bundle-and-Brewfile
TODO: regelmässige sicherung der installierten apps incl. AppStore mit 
brew bundle dump --mas

install git (=Brewfile)


install dotfiles from git repo
  Clone dotfiles to the Mac: `git clone git@github.com:geerlingguy/mac-dev-playbook.git`
dotfiles_files:
  - .zshrc + https://starship.rs/ (.zprofile?)
  - .gitignore, .gitignore_global
  - .inputrc
  - .vimrc, .viminfo
  - directory .gnupg
  - directory .ssh
  - directory .config (?)
  - .bash_profile und .bashrc (legacy), .profile
dann diese dateien (dir .git) unter versionsverwaltung plus autom. commits bei änderungen/daily

systemeinstellungen (defaults write etc)
  shell script used in dotfiles repositories to automate the configuration of macOS system preferences and defaults
  home/.osx, chmod +x ~/.osx

TODO: DNS resolver config?

softwareupdate für systemsoftware (man softwareupdate; evtl skripten?)

dockutil: add/remove Apps from/to the dock


tailscale/netbird?, mullvad VPN?

standard-OS-apps nicht aufgeführt?

1Password.app
AusweisApp.app
AutoRaise.app
BBEdit.app besser Zed?
--BetterDisplay
BitBox
--Blackmagic Disk Speed Test
BraveBrowser.app (statt Chrome?)
Breitbandmessung
Canon Utilities (Drucker + Scanner?)
Developer.app
--Discovery
DriveDX
Freeform
GarageBand.app
Gemini.app
--Google Chrome
HandBrake.app
IINA.app
iMovie.app
Infuse.app
iTerm.app
JetBrains Toolbox.app
Keynote Creator Studio.app
--Kindle
--KnockKnock
--LaunchControl.app
--Lingon X
MacFamilyTree 11.app
MacWhisper (aus Gumroad)
--Magnet
MakeMKV (TODO: dvdcss braucht man noch)
--NewsLazer
Numbers Creator Studio.app
Pages Creator Studio.app
--Passwörter
PDF Expert.app
Pixelmator Pro.app
--Safari.app
Tailscale.app
--Telegram.app
Time Machine
TradingView.app
Trezor Suite.app
TV
--UTM
VLC
Weasis.app
WhatsApp.app
Whisper Transcription.app
Xcode.app
Zed.app


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
    - Install Wireguard VPN configurations (if needed)

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

