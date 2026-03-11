# code-session-manager

A lightweight dev session manager that ties together tmux, Ghostty, and VS Code. Switch between projects and have your terminal session and editor follow together.

## What it does

- Opens a single Ghostty window with a session picker on the left and tmux on the right
- Each project gets a tmux session with Claude Code (top pane) and a shell (bottom pane)
- Switching sessions in the picker also switches VS Code to that project's folder
- Create and kill sessions from the picker

## Dependencies

- [Ghostty](https://ghostty.org/) terminal
- [tmux](https://github.com/tmux/tmux)
- [fzf](https://github.com/junegunn/fzf)
- VS Code with `code` CLI

## Setup

1. Add the `bin` directory to your PATH:
   ```sh
   export PATH="$HOME/dev/code-session-manager/bin:$PATH"
   ```

2. Add the VS Code auto-switch hook to your `~/.tmux.conf`:
   ```sh
   set-hook -g client-session-changed 'run-shell "$HOME/dev/code-session-manager/bin/dev-code-switch \"#{session_path}\""'
   ```

3. Reload tmux config:
   ```sh
   tmux source-file ~/.tmux.conf
   ```

## Usage

Run `dev-start` to open the environment. In the picker:

- **Enter** or **double-click** — switch to a session
- **n** — create a new session
- **x** — kill the selected session
- **q** — quit the picker
