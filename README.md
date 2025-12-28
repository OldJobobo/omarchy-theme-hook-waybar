# Omarchy theme-set hook

Adds Waybar theme integration when Omarchy switches themes.

## What it does

- Runs after `omarchy-theme-set` completes (via Omarchy hooks).
- If the selected theme has `waybar-theme/` with `config.jsonc` and/or `style.css`, it:
  - Backs up existing `~/.config/waybar/config.jsonc` and `~/.config/waybar/style.css` when they are regular files.
  - If either is a symlink, it is removed (not backed up) before relinking.
  - Symlinks the theme's Waybar files into `~/.config/waybar/`.
  - Restarts Waybar (only if it is running).

## Install

1. Keep the hook script in this repo:
   - `<repo>/theme-set`
2. Symlink it into Omarchy's hook location (run from the repo root):

```bash
cd /path/to/omarchy-theme-hook-waybar
mkdir -p ~/.config/omarchy/hooks
ln -snf "$PWD/theme-set" ~/.config/omarchy/hooks/theme-set
chmod +x "$PWD/theme-set"
```

## Usage

1. Put Waybar files in your theme:

```
~/.config/omarchy/themes/<theme-name>/
  waybar-theme/
    config.jsonc
    style.css
```

2. Switch themes as usual:

```bash
omarchy-theme-set <theme-name>
```

The hook runs automatically after the theme is applied.

## Backups

Each run creates a timestamped backup folder when `waybar-theme/` exists:

```
~/.config/waybar-backups/YYYYMMDD-HHMMSS/
```

Backup behavior details:

- Only `config.jsonc` and `style.css` are considered.
- A file is copied only if it exists and is a regular file (symlinks are not copied).
- If a file is a symlink, it is removed before the new symlink is created.
- If neither file is a regular file, the backup folder may be empty.
