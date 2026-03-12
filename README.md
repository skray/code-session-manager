# code-session-manager

A lightweight dev session manager that ties together tmux, Ghostty, and VS Code. Switch between projects and have your terminal session and editor follow together.

## What it does

- Opens a single Ghostty window with a session picker on the left and tmux on the right
- Each project gets a tmux session with Claude Code (top pane) and a shell (bottom pane)
- Switching sessions in the picker also switches VS Code to that project's folder
- Create sessions from project folders in `~/dev`, with optional git worktree support via [worktrunk](https://worktrunk.dev/) (`wt`)
- Kill sessions from the picker without losing the tmux pane

## Dependencies

- [Ghostty](https://ghostty.org/) terminal
- [tmux](https://github.com/tmux/tmux)
- [fzf](https://github.com/junegunn/fzf)
- VS Code with `code` CLI
- [worktrunk](https://worktrunk.dev/) (`wt`) — for git worktree support (optional but recommended)

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

### Worktrunk configuration

The picker integrates with worktrunk for git worktree management. The following settings in `~/.config/worktrunk/config.toml` control how worktrees are created:

**Worktree path** — worktrees are stored in `~/dev/worktrees/<repo>/<branch>`:

```toml
worktree-path = "~/dev/worktrees/{{ repo }}/{{ branch | sanitize }}"
```

**New branches** — the picker creates new worktrees from `origin/main` (via `wt switch --create <branch> --base origin/main`) so you always branch from the latest remote state. A `git fetch` runs automatically before branch creation.

When creating a new session with **n**, the picker will:
1. Let you pick a project from `~/dev`
2. Show options: open directly, create a new worktree, or select an existing worktree
3. For existing branches (local or remote), use `wt switch` without `--create`
4. For new branches, use `wt switch --create` with `--base origin/main`

## Usage

Run `dev-start` to open the environment. In the picker:

- **Enter** or **double-click** — switch to a session
- **n** — create a new session (project + worktree picker)
- **x** — kill the selected session
- **q** — quit the picker and close the Ghostty panes
