---
description: >-
  A guide to setting up an Apple Mac for DevOps and software development. This
  is current for macOS 13 (Ventura).
---

# Development Environment Setup

## Do This First!

Log in once, run Software Update, and ensure that the operating system is at the latest point release. After all of the updates have been applied, restart the computer.

### Hardware Configuration

#### Configuring The Trackpad

To make the trackpad behave correctly, ensure that these settings are enabled:

* _System Preferences > Trackpad > Tap to click_
* _System Preferences > Accessibility > Mouse & Trackpad > Trackpad Options... > Enable dragging: three finger drag_

#### Configuring The Mouse

_System Preferences > Mouse > Turn Natural scroling off_

#### External keyboard

_System Preferences > Keyboard > Keyboard Shortcuts... > Modifier Keys_ and _Select_ your own external keyboard.

Switch _Option Key & Command Key_ configurations.

### Configuring Security

#### Securing the Safari Browser

Whether or not you regularly use Safari, you should open it once, and adjust the settings in case that you use it later.

First, choose _Safari > Preferences > General_ and deselect the option _Open “safe” files after downloading_.

Second, go to _Safari > Preferences > Search_. Decide which search engine that you want to use. Ensure that _Safari Suggestions_ and _Preload Top Hit in the background_ are not enabled.

#### Security & Privacy

Select _System Preferences > Security & Privacy_, and set the following:

* Under _General, s_et require a _password after sleep or screen saver begins_ to _immediately_
* Under _General,_ click _Advanced... and s_elect _Require an administrator password to access system-wide preferences_
* Under _Firewall_, click _Turn Firewall On._
* Under _Privacy_, select _Analytics & Improvements_ and ensure that the options are not enabled.

#### Spotlight

By default, Spotlight sends queries to Apple. Unless you want this feature, turn it off.

Select _System Preferences > Spotlight > Search Results_, and ensure that _Siri Suggestions_ is not enabled.

### System software Configuration

#### Dock

* _Visual Settings_
  * _Position on screen: Left_
  * _Size: Small_
* _Other settings_
  * _Remove workspace auto-switching by running the following command:_

```bash
$ defaults write com.apple.dock workspaces-auto-swoosh -bool NO
$ killall Dock # Restart the Dock process
```

#### Finder Preferences

* _General_
  * _New Finder windows show >_ your _Home Directory_
* _Sidebar_
  * Add _Home_ & your _Code Directory_
  * Uncheck all _Shared_ boxes

## Setting Up for Development

The first step is to install the _Command Line Tools for Xcode_. Once you have installed Command Line Tools, you can use [Homebrew](http://brew.sh/) to install everything else that you need.

### Getting Xcode

Apple now provide the Xcode suite as a free download from the App Store. To install the Command Line Tools, install Xcode from the App Store, then open a Terminal window and enter the following command:

```bash
xcode-select --install
```

And run the following command to agree the Xcode license.

```
sudo xcodebuild -license
```

### Setting Up Homebrew

[Homebrew](http://brew.sh/) provides a package management system for macOS, enabling you to quickly install and update the tools and libraries that you need. Follow the instructions on the site.

You should also amend your PATH, so that the versions of tools that are installed with Homebrew take precedence over others. Run these three commands in your terminal to add Homebrew to your **PATH**:

```bash
echo '# Set PATH, MANPATH, etc., for Homebrew.' >> ~/.zprofile
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

To check that Homebrew is installed correctly, run this command in a terminal window:

```bash
brew doctor
```

To update the index of available packages, run this command in a terminal window:

```bash
brew update
```

#### Enabling Auto Completion of Commands <a href="#enabling-auto-completion-of-commands" id="enabling-auto-completion-of-commands"></a>

To enable auto completion, edit the file _.zshrc_ in your home directory to include this line:

```bash
autoload bashcompinit && bashcompinit
```

To use auto completion, type the name of the command, and press the Tab key on your keyboard. You will see a list of possible completions. Press the Tab key to cycle through the completions, and press the Enter key to accept a completion.

### Installing the Git Version Control System

Always set your details before you create or clone repositories on a new system. This requires two commands in a terminal window:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@your-domain.com"
```

The _global_ option means that the setting will apply to every repository that you work with in the current user account.

To enable colors in the output, which can be very helpful, enter this command:

```bash
git config --global color.ui auto
```

### Creating SSH Keys

You will frequently use SSH to access Git repositories or remote UNIX systems. macOS includes the standard OpenSSH suite of tools.

OpenSSH stores your SSH keys in a _.ssh_ directory. To create this directory, run these commands in a terminal window:

```bash
mkdir $HOME/.ssh
chmod 0700 $HOME/.ssh
```

To create an SSH key, run the ssh-keygen command in a terminal window. For example:

```bash
ssh-keygen -t rsa -b 4096 -C "Me MyName (MyDevice) <me@mydomain.com>"
```

{% hint style="info" %}
Use 4096-bit RSA keys for all systems. The older DSA standard only supports 1024-bit keys, which are now too small to be considered secure.
{% endhint %}

### Oh My Zsh

Oh My Zsh is an open source, community-driven framework for managing your zsh configuration. It comes with a bunch of features out of the box and improves your terminal experience.

{% embed url="https://ohmyz.sh/" %}

#### Theme

[Powerlevel10k](https://github.com/romkatv/powerlevel10k) is a theme for Zsh.

#### Plugins

Zsh users have created some useful additions that integrate with it. One of the cool things you can add is syntax highlighting to color command types.

{% embed url="https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md" %}

```shell
git config --global --add oh-my-zsh.hide-dirty 1
```

Another one is auto suggestions, which remember common commands that you can easily re-run.

{% embed url="https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md" %}

```shell
pasteinit() {
  OLD_SELF_INSERT=${${(s.:.)widgets[self-insert]}[2,3]}
  zle -N self-insert url-quote-magic # I wonder if you'd need `.url-quote-magic`?
}

pastefinish() {
  zle -N self-insert $OLD_SELF_INSERT
}
zstyle :bracketed-paste-magic paste-init pasteinit
zstyle :bracketed-paste-magic paste-finish pastefinish
```

Add plugins to your shell by adding the name of the plugin to the `plugin` array in your `.zshrc` .

```
plugins=(git zsh-syntax-highlighting zsh-autosuggestions)
```



### Installing iTerm2

iTerm2 is an open source replacement for Apple's Terminal. It's highly customizable and comes with a lot of useful features.

[https://www.iterm2.com/](https://www.iterm2.com/)

{% hint style="info" %}
Instead of downloading and installing iTerm2 manually, you can use Homebrew

```bash
brew cask install iterm2
```
{% endhint %}

#### Useful Shortcuts

| shortcut |         action         | send |
| :------: | :--------------------: | :--: |
|    ⌘←    | _Send Escape Sequence_ |  OH  |
|    ⌘→    | _Send Escape Sequence_ |  OF  |
|    ⌥←    | _Send Escape Sequence_ |   b  |
|    ⌥→    | _Send Escape Sequence_ |   f  |

{% hint style="info" %}
You may need to edit your config file as well:

{% code title="~/.zshrc" %}
```bash
bindkey -e
bindkey "\e\e[C" forward-word
bindkey "\e\e[D" backward-word
```
{% endcode %}
{% endhint %}

### Text Editors

Installations of macOS include older command-line versions of both **Emacs** and **vim**, as well as TextEdit, a desktop text editor. TextEdit is designed for light-weight word processing, and has no support for programming. Unless you already have a preferred editor, I suggest that you install **Visual Studio Code**, which are powerful graphical text editors.

#### Setting Up Visual Studio Code

To install Visual Studio Code, enter this command in a terminal window:

```bash
brew cask install visual-studio-code
```

To make Visual Studio Code your default editor, use this line in your _\~/.zshrc_ file:

```bash
export EDITOR="code -n -w"
```

### Setting Up Environments

#### Node.js for JavaScript Development

Homebrew provides separate packages for each version of [Node.js](https://nodejs.org/). To ensure that you are using the version of Node.js that you expect, specify the version when you install it. For example, enter this command in a Terminal window to install the Node.js 16, the current LTS release:

```bash
brew install node@16
```

#### Rust Development: rustup <a href="#rust-development-rustup" id="rust-development-rustup"></a>

The official _rustup_ utility enables you to install the tools for building software with the Rust programming language. Click on the Install button on the front page of the [Rust Website](https://www.rust-lang.org/), and follow the instructions.

By default, the installer adds the correct directory to your path. If this does not work, add this to your PATH manually:

```bash
$HOME/.cargo/bin
```

This process installs all of the tools into your home directory, and does not add any files into system directories.
