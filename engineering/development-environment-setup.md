---
description: A Modern Front-end Development Environment Setup
---

# Development Environment Setup

## Do This First!

### Configuring The Trackpad

To make the trackpad behave correctly, ensure that these settings are enabled:

* _System Preferences &gt; Trackpad &gt; Tap to click_
* _System Preferences &gt; Accessibility &gt; Mouse & Trackpad &gt; Trackpad Options... &gt; Enable dragging: three finger drag_

## Setting Up for Development

The first step is to install a compiler. The easiest way to install one is with the _Xcode Command Line Tools_ package.

Once you have the compiler that is provided by Xcode, you can use **Homebrew** to install everything else that you need. Homebrew itself manages packages for command-line tools and services. The **Cask** extension to Homebrew enables you to install graphical desktop applications.

### Getting Xcode

Apple now provide the Xcode suite as a free download from the App Store. To install Xcode Command Line Tools, install Xcode from the App Store, the open a Terminal window and enter the following command:

```text
xcode-select --install
```

### Setting Up Homebrew

{% embed url="https://brew.sh/" %}

**Homebrew** provides a package management system for macOS, enabling you to quickly install and update the tools and libraries that you need. Follow the instructions on the site.

You should also amend your PATH, so that the versions of tools that are installed with Homebrew take precedence over others. To do this, edit the file _.bash\_profile_ in your home directory to include this line:

```text
export PATH="/usr/local/bin:/usr/local/sbin:~/bin:$PATH"
```

To check that Homebrew is installed correctly, run this command in a terminal window:

```text
brew doctor
```

To update the index of available packages, run this command in a terminal window:

```text
brew update
```

Once you have set up Homebrew, use the _brew install_ command to add command-line software to your Mac, and _brew cask install_ to add graphical software. For example, this command installs the Slack app:

```text
brew cask install slack
```

### Installing the Git Version Control System

The Xcode Command Line Tools include a copy of **Git**, which is now the standard for Open Source development, but this will be out of date.

To install a newer version of Git than Apple provide, use Homebrew. Enter this command in a terminal window:

```text
brew install git
```

Always set your details before you create or clone repositories on a new system. This requires two commands in a terminal window:

```text
git config --global user.name "Your Name"
git config --global user.email "you@your-domain.com"
```

The _global_ option means that the setting will apply to every repository that you work with in the current user account.

To enable colors in the output, which can be very helpful, enter this command:

```text
git config --global color.ui auto
```

### Text Editors

Installations of macOS include older command-line versions of both **Emacs** and **vim**, as well as TextEdit, a desktop text editor. TextEdit is designed for light-weight word processing, and has no support for programming. Unless you already have a preferred editor, I suggest that you install either **Visual Studio Code** or **Oni**, which are powerful graphical text editors, or **Neovim** for a console Vim editor.

#### Setting The EDITOR Environment Variable

Whichever text editor you choose, remember to set the EDITOR environment variable in your _~/.bash\_profile_ file, so that this editor is automatically invoked by command-line tools like your version control system. For example, put this line in your profile to make Neovim \(nvim\) the favored text editor:

```text
export EDITOR="nvim"
```

#### Setting Up Visual Studio Code

To install Visual Studio Code, enter this command in a terminal window:

```text
brew cask install visual-studio-code
```

Consider installing these extensions:

* **ESLint** or **TSLint** for JavaScript linter integration
* **Debugger for Chrome** to debug JavaScript in the Web browser
* **Git Lens** to enhance the Git support in the user interface
* The **Docker** extension
* **YAML Support**

To make Visual Studio Code your default editor, use this line in your _~/.bash\_profile_ file:

```text
export EDITOR="code -w"
```

Visual Studio Code enables telemetry and crash reporting by default. To disable these, set these options in _Preferences &gt; Settings_:

```text
"telemetry.enableTelemetry": false
"telemetry.enableCrashReporter": false
```

If you would like to enable Vim keybindings in Visual Studio Code, install the **VSCodeVim** extension.

### Creating SSH Keys

You will frequently use SSH to access Git repositories or remote UNIX systems. macOS includes the standard OpenSSH suite of tools.

OpenSSH stores your SSH keys in a _.ssh_ directory. To create this directory, run these commands in a terminal window:

```text
mkdir $HOME/.ssh
chmod 0700 $HOME/.ssh
```

To create an SSH key, run the ssh-keygen command in a terminal window. For example:

```text
ssh-keygen -t rsa -b 4096 -C "Me MyName (MyDevice) <me@mydomain.com>"
```

{% hint style="info" %}
Use 4096-bit RSA keys for all systems. The older DSA standard only supports 1024-bit keys, which are now too small to be considered secure.
{% endhint %}

## Setting Up Environments

### Node.js for JavaScript Development

Homebrew provides seperate packages for each version of Node.js. To ensure that you are using the version of Node.js that you expect, specify the version when you install it. For example, enter this command in a Terminal window to install the Node.js 10, the current LTS release:

```text
brew install node@10
```

### rustup for Rust Development

The official _rustup_ utility enables you to install the tools for building software with the Rust programming language. Click on the Install button on the front page of the **Rust Website**, and follow the instructions.

{% embed url="https://www.rust-lang.org/en-US/" %}

By default, the installer adds the correct directory to your path. If this does not work, add this to your PATH manually:

```text
$HOME/.cargo/bin
```

This process installs all of the tools into your home directory, and does not add any files into system directories.

