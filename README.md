# Mac Setup Process
Recipes for setting up a new mac aand erasing aa mac. 100% automation is technically impossible, thus everything is documented here

## Setup Wizard
1. complete the mandatory macOS setup wizard (creating a local user account, signing into my iCloud account)
   **TODO describe all steps**
2. sign in to AppStore (since `mas` can't sign in automatically)
3. **TODO syncing iCloud Drive and wait for completion?**
4. install Xcode commandline tools
```
xcode-select --install
```
5. install homebrew (official script to /opt/homebrew)
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

echo >> /Users/lothar/.zprofile
echo 'eval "$(/opt/homebrew/bin/brew shellenv zsh)"' >> /Users/lothar/.zprofile
```
7. amend .zshrc?
```
export PATH="/opt/homebrew/bin:$PATH"
export HOMEBREW_VERIFY_ATTESTATIONS=1
export HOMEBREW_NO_ENV_HINTS=1
export HOMEBREW_QUIET=1
export HOMEBREW_NO_ASK=1
#export HOMEBREW_NO_ANALYTICS=1
#export HOMEBREW_VERBOSE=1
```

brew install --cask font-jetbrains-mono-nerd-font
brew install --cask font-iosevka-term-nerd-font
brew install --cask font-googlesanscode-nerd-font
brew install starship

add to end of .zshrc
eval "$(starship init zsh)"
export STARSHIP_CONFIG=~/.config/starship.toml
export STARSHIP_CACHE=~/.cache/starship/

mkdir -p ~/.config && touch ~/.config/starship.toml


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
https://github.com/olafhartong/osx-defaults/blob/master/.osx
https://github.com/alberti42/macOS-config-manager (defaults backup/restore)

systemeinstellungen per CLI:
* defaults write – die meisten App- und System-Prefs
* systemsetup – Host, Timezone, Remote Login (braucht oft sudo)
* osascript – System Settings, die kein defaults-Key haben
* pmset, networksetup, scutil – Power, Netzwerk, Hostname
* https://github.com/kevinSuttle/macOS-Defaults/blob/master/REFERENCE.md
launchctl, mdutil for spotlight?, systemsetup, tmutil

aber: viele moderne Settings (Datenschutz, Notifications) gehen nicht zuverlässig per CLI und brauchen ein signiertes Configuration Profile oder MDM. Ggfs. Apple Configurator for mac oder iMazing Profile Editor oder Manuell (XML/plist)


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

## Why 100% automation is impossible

![placeholder](non-transferable%20configuration.jpg)
