# CLI Tools Reference

Modern CLI tools and utilities for daily development workflow, plus the classic utility tools from the CSDN article.

## Modern CLI Essentials

### File Search & Navigation

| Tool | Replaces | Install | Description |
|------|----------|---------|-------------|
| `ripgrep` (rg) | grep | `brew install ripgrep` | Much faster text search |
| `fd` | find | `brew install fd` | Simpler file finding |
| `fzf` | Ctrl+R | `brew install fzf` | Fuzzy finder for files, history, git branches |
| `zoxide` | cd | `brew install zoxide` | Learns your habits, smart directory jumping |
| `eza` / `lsd` | ls | `brew install eza` or `brew install lsd` | Modern ls with icons, Git status, colors |

### File Viewing

| Tool | Replaces | Install | Description |
|------|----------|---------|-------------|
| `bat` | cat | `brew install bat` | File viewer with syntax highlighting and line numbers |
| `delta` | git diff | `brew install git-delta` | Beautiful, syntax-highlighted git diff |

### Git Enhancement

| Tool | Install | Description |
|------|---------|-------------|
| `lazygit` | `brew install lazygit` | Terminal UI for git operations |
| `gh` | `brew install gh` | GitHub CLI: manage PRs, Issues, Actions |
| `tig` | `brew install tig` | Interactive git viewer |

### Utilities

| Tool | Install | Description |
|------|---------|-------------|
| `jq` | `brew install jq` | Command-line JSON processor |
| `htop` / `btop` | `brew install htop btop` | System resource monitor |
| `tldr` | Multiple (see below) | Simplified man pages |
| `httpie` / `curlie` | `brew install httpie` | Human-friendly HTTP client |
| `ncdu` | `brew install ncdu` | Disk usage analyzer |
| `tree` | `brew install tree` | Directory tree display |
| `watchexec` | `brew install watchexec` | Run commands on file changes |

## Quick Install All Essentials

```bash
# File & search
brew install ripgrep fd fzf zoxide eza bat git-delta

# Git enhancement
brew install lazygit gh

# Utilities
brew install jq htop btop tree ncdu tldr
```

## Tool Configuration

### fzf (Fuzzy Finder)

```bash
# Install fzf key bindings and fuzzy completion
$(brew --prefix)/opt/fzf/install

# Or add to ~/.zshrc:
[ -f ~/.fzf.zsh ] && source ~/.fzf.zsh
```

Useful shortcuts after setup:
- `Ctrl+R` — Fuzzy search through command history
- `Ctrl+T` — Fuzzy find and insert file path
- `Alt+C` — Fuzzy cd into subdirectory

### zoxide

Add to ~/.zshrc:

```bash
eval "$(zoxide init zsh)"
```

Usage: `z <partial-name>` jumps to the best matching directory.

### bat

```bash
# Set as default cat replacement (add to ~/.zshrc)
alias cat='bat --paging=auto'
```

### delta (Git Diff)

Add to ~/.gitconfig:

```ini
[core]
    pager = delta

[interactive]
    diffFilter = delta --color-only

[delta]
    navigate = true
    side-by-side = true
```

## Classic Utility Tools (from CSDN Article)

### tldr — Simplified Man Pages

Shows practical examples instead of full documentation:

```bash
# Install (choose one)
npm install -g tldr
pip3 install tldr
brew install tlrc

# Usage
tldr tar
tldr git log
```

### ydict — Command Line Dictionary

```bash
# Install
pip3 install ydict

# Usage
ydict hello
```

### rsync — File Synchronization

Usually pre-installed on macOS. Key usage:

```bash
# Copy local -> remote
rsync -avz --progress localdir/ user@host:/remote/path/

# Copy remote -> local
rsync -avz --progress user@host:/remote/path/ localdir/

# Preview changes (dry run)
rsync -avzn localdir/ user@host:/remote/path/

# Delete files on destination that don't exist on source
rsync -avz --delete localdir/ user@host:/remote/path/
```

### colorls — Colorful ls

```bash
# Install (requires Ruby)
brew install ruby
gem install colorls

# Usage
colorls
colorls -la
colorls --tree
```

## GUI Utility Tools (from CSDN Article)

### SwitchHosts — Host File Switcher

Manage multiple hosts configurations for different environments (dev, staging, production):

```bash
brew install --cask switchhosts
```

Use case: Switch between different API endpoints or proxy settings by toggling host file configurations.

Example host configuration:
```
# Development
127.0.0.1 api.local.dev
127.0.0.1 proxy.golang.org

# Production (comment out development entries)
# 127.0.0.1 api.local.dev
```

### Snipaste — Screenshot Pinning

Take screenshots and pin them to the screen as floating windows:

```bash
brew install --cask snipaste
```

Key features:
- `F1` — Take screenshot and pin to screen
- `F3` — Pin image from clipboard
- `Click pin` — Edit, resize, or annotate
- `Esc` — Close pinned screenshot

### Other Recommended GUI Tools

```bash
# Window management
brew install --cask rectangle

# Spotlight replacement / launcher
brew install --cask raycast

# Password manager
brew install --cask bitwarden

# API testing
brew install --cask postman

# Screen recording
brew install --cask cleanshot-x

# Note-taking
brew install --cask obsidian

# Browser
brew install --cask arc
```
