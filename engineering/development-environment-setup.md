---
description: >-
  A guide to setting up a modern Apple Silicon Mac for high-performance Frontend
  and AI engineering. This setup prioritizes speed (Rust-based CLI tools) and
  AI-native workflows.
---

# Development Environment Setup (2026 Edition)

## Do This First!

Log in, run Software Update, and ensure macOS is on the latest version. Restart the computer.

### System Configuration

**1. Keyboard & Trackpad (Crucial for Speed)**

* System Settings > Trackpad:
  * Enable _Tap to click_.
  * _Accessibility > Pointer Control > Trackpad Options_: Enable _Use trackpad for dragging_ (Three Finger Drag).
* System Settings > Keyboard:
  * Key Repeat: Set to _Fastest_.
  * Delay Until Repeat: Set to _Shortest_. (Essential for navigating code quickly).

**2. Finder Tweaks**

* _Finder > Settings > Advanced_:
  * Show all filename extensions.
  * Show warning before changing an extension: OFF.
* Show Path Bar: View > Show Path Bar.
* Show Hidden Files: Press `Cmd + Shift + .` inside Finder.

**3. Replace Spotlight with Raycast**

Stop using the default Spotlight. Raycast is the standard for modern developers (Scriptable, Window Management, AI).

1. Download from [raycast.com](https://www.raycast.com/).
2. Set the hotkey to `Cmd + Space`.
3. Disable Spotlight shortcut: _System Settings > Keyboard > Keyboard Shortcuts > Spotlight_ (Uncheck "Show Spotlight Search").

### The Terminal (Modernized)

We are moving away from legacy tools like iTerm2 and heavy Zsh themes in favor of GPU-accelerated terminals and Rust-based prompts.

#### 1. Terminal Emulator

Choose one of the following:

* Warp: (Recommended) AI-integrated, input works like a text editor, command prediction.
* Ghostty: GPU-accelerated, written in Zig, incredibly fast and minimal.

#### 2. Install Homebrew

The package manager for macOS.

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Add Homebrew to PATH: (Follow the instructions in the terminal output after installation, usually looks like this):

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

#### 3. Shell & Prompt (Zsh + Starship)

macOS uses Zsh by default. We will use Starship (written in Rust) for the prompt because it is instant and requires zero config to support Git/Node/Rust/Web3 contexts.

Install Starship & Tools:

```bash
brew install starship
```

Configure `~/.zshrc`: Open your config file:

```bash
nano ~/.zshrc
```

Add this line to the end:

```bash
eval "$(starship init zsh)"
```

#### 4. Rust-based CLI Replacements

Replace legacy Unix commands with modern, faster alternatives.

```bash
brew install eza bat zoxide ripgrep fzf
```

* eza (better `ls`): Adds icons and git status.
* bat (better `cat`): Syntax highlighting for files.
* zoxide (better `cd`): Smart directory jumping (e.g., `z project` jumps to `~/code/project`).
* ripgrep (better `grep`): Fastest text search.

Add these aliases to your `~/.zshrc` to make them permanent:

```bash
# Modern Aliases
alias ls="eza --icons"
alias ll="eza -l --icons --git"
alias cat="bat"
alias cd="z"

# Initialize zoxide
eval "$(zoxide init zsh)"
```

***

### Git & SSH

#### 1. Configure Git

```bash
git config --global user.name "Your Name"
git config --global user.email "you@your-domain.com"
git config --global init.defaultBranch main
git config --global color.ui auto
```

#### 2. Generate SSH Key (Ed25519)

We now use Ed25519 keys (more secure and compact than RSA).

```bash
ssh-keygen -t ed25519 -C "you@your-domain.com"
```

Start the ssh-agent:

```bash
eval "$(ssh-agent -s)"
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

***

### Runtimes & Environments

#### 1. Node.js (via fnm)

Do not use `brew install node`. Use fnm (Fast Node Manager), built in Rust. It is significantly faster than nvm.

```bash
brew install fnm
```

Add to `~/.zshrc`:

```bash
eval "$(fnm env --use-on-cd)"
```

Install the latest LTS Node:

```bash
fnm install --lts
```

Package Manager: Enable Corepack to use `pnpm` (standard for modern monorepos):

```bash
corepack enable
```

#### 2. Python (via uv)

For AI and scripting, use uv. It replaces pip, poetry, and virtualenv.

```bash
brew install uv
```

#### 3. Rust (via rustup)

Essential for Tauri development and high-performance tooling.

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

#### 4. Containerization (OrbStack)

OrbStack is a lightweight, drop-in replacement for Docker Desktop on macOS. It starts in 2 seconds and uses minimal RAM.

```bash
brew install --cask orbstack
```

***

### Editors (AI-First)

#### 1. Cursor (Recommended)

Cursor is a fork of VS Code with native AI integration. It indexes your entire codebase for smarter context.

* Download: [cursor.com](https://cursor.com/)
* Migration: It can import all your VS Code extensions and settings on the first launch.

#### 2. Visual Studio Code (Legacy/Fallback)

```bash
brew install --cask visual-studio-code
```

Enable CLI: Open VS Code, `Cmd + Shift + P` -> "Shell Command: Install 'code' command in PATH".

#### 3. Recommended Font

Install JetBrains Mono (Nerd Font version) to support icons in the terminal.

```bash
brew install --cask font-jetbrains-mono-nerd-font
brew install --cask font-monaspace
brew install --cask font-cascadia-code
```

_Set this font in your Terminal and Editor._

***

### Summary of `~/.zshrc`

Your final `.zshrc` should look clean like this:

```bash
# Path & Homebrew
export PATH="/opt/homebrew/bin:$PATH"

# Tools Initialization
eval "$(starship init zsh)"
eval "$(fnm env --use-on-cd)"
eval "$(zoxide init zsh)"

# Aliases
alias ls="eza --icons"
alias ll="eza -l --icons --git"
alias cat="bat"

# Editor
export EDITOR="code -n -w" # Or "cursor -n -w"
```
