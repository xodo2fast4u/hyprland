# Hyprland Desktop

A dark, keyboard driven desktop for Hyprland on Arch Linux. The setup is split into separate repositories so each part can be installed or replaced on its own: the compositor configuration, a Waybar theme, a wofi launcher theme, dunst notifications, GTK 3 and GTK 4 themes and a few helper scripts.

![Desktop screenshot](https://imglink.cc/cdn/Ssd0DHapYA.png)

## What is included

| Repository | Installs to | Purpose |
| --- | --- | --- |
| [hypr](https://github.com/xodo2fast4u/hypr) | `~/.config/hypr` | Hyprland configuration, split into Lua modules |
| [waybar](https://github.com/xodo2fast4u/waybar) | `~/.config/waybar` | Top bar layout, styling and dark and light stylesheets |
| [wofi](https://github.com/xodo2fast4u/wofi) | `~/.config/wofi` | Application launcher styling |
| [dunst](https://github.com/xodo2fast4u/dunst) | `~/.config/dunst` | Notification daemon settings |
| [gtk-3.0](https://github.com/xodo2fast4u/gtk-3.0) | `~/.config/gtk-3.0` | GTK 3 theme settings, dark and light CSS, file manager bookmarks |
| [gtk-4.0](https://github.com/xodo2fast4u/gtk-4.0) | `~/.config/gtk-4.0` | GTK 4 theme settings and light and dark CSS |
| [bin](https://github.com/xodo2fast4u/bin) | `~/.local/bin` | Theme switching and volume scripts |

## Requirements

- Arch Linux or a distribution based on Arch.
- Hyprland 0.55 or newer. The configuration uses the Lua API that Hyprland introduced in version 0.55. Check your version with:

```bash
hyprctl version
```

- Packages from the official repositories and the AUR, listed in the installation steps below.

## Installation

### 1. Install the packages

Update the system first:

```bash
sudo pacman -Syu
```

Install the packages from the official repositories:

```bash
sudo pacman -S --needed hyprland hyprpaper xdg-desktop-portal-hyprland waybar wofi dunst copyq gnome-keyring kitty thunar grim brightnessctl playerctl wireplumber libnotify pavucontrol btop papirus-icon-theme ttf-jetbrains-mono-nerd
```

Install an AUR helper if you do not already have one:

```bash
sudo pacman -S --needed base-devel git
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si
```

Then install these from the AUR. Search for each name first with `paru -Ss <name>` to confirm the package name:

- `grimblast`, used for screenshots.
- A Bibata cursor theme that provides `Bibata-Modern-Ice`.
- The Nordic GTK theme, which must be named `Nordic`.
- The Catppuccin Mocha Lavender GTK theme. The theme toggle expects it at `/usr/share/themes/catppuccin-mocha-lavender-standard+default`.

The dark mode toggle depends on the two GTK themes, so install both before you use it.

### 2. Clone the repositories

```bash
mkdir -p ~/.config ~/.local/bin ~/Pictures/Screenshots
git clone https://github.com/xodo2fast4u/hypr ~/.config/hypr
git clone https://github.com/xodo2fast4u/waybar ~/.config/waybar
git clone https://github.com/xodo2fast4u/wofi ~/.config/wofi
git clone https://github.com/xodo2fast4u/dunst ~/.config/dunst
git clone https://github.com/xodo2fast4u/gtk-3.0 ~/.config/gtk-3.0
git clone https://github.com/xodo2fast4u/gtk-4.0 ~/.config/gtk-4.0
git clone https://github.com/xodo2fast4u/bin ~/.local/bin/bin-repo
```

Copy the scripts into `~/.local/bin` and make them executable:

```bash
cp ~/.local/bin/bin-repo/*.sh ~/.local/bin/
chmod +x ~/.local/bin/*.sh
rm -rf ~/.local/bin/bin-repo
```

If a configuration folder already exists, move it aside before cloning. For example:

```bash
mv ~/.config/hypr ~/.config/hypr.bak
```

### 3. Change the machine specific values

The configuration was written for one machine. Change these before the first login:

| File | What to change |
| --- | --- |
| `~/.config/hypr/monitors.lua` | Output name, resolution and refresh rate. Run `hyprctl monitors` to list your outputs. |
| `~/.config/hypr/hyprpaper.conf` | Wallpaper path and monitor name. Remove the `wallpaper` block to run without a wallpaper. |
| `~/.config/hypr/programs.lua` | Terminal, file manager and launcher commands. |
| `~/.config/hypr/autostart.lua` | Applications started at login. Remove any you do not use. |
| `~/.config/hypr/keybindings.lua` | Path to `copyq` if it is not at `/usr/bin/copyq`. |
| `~/.config/hypr/input.lua` | Keyboard layout and touchpad options. |
| `~/.config/hypr/env.lua` | Cursor theme name and size. |
| `~/.config/hypr/permissions.lua` | Paths of screenshot tools if they are not in `/usr/bin` or `/usr/local/bin`. |
| `~/.config/gtk-3.0/bookmarks` | File manager bookmarks. Replace the paths with your own folders or delete the file. |

Hyprland loads `hyprland.lua` as its entry point. The file named `hyprland` with no extension in the hypr repository is empty and can be deleted.

### 4. Log in

Log out of your current session, select Hyprland from the display manager, or run `Hyprland` from a TTY.

If something does not start, check the logs:

```bash
cat ~/.cache/hyprland/hyprland.log
journalctl --user -b -e
```

## Keybindings

The main modifier is `Super`, the Windows key.

| Binding | Action |
| --- | --- |
| `Super+Q` | Open the terminal |
| `Super+E` | Open the file manager |
| `Super+R` | Open the application launcher |
| `Super+C` | Close the active window |
| `Super+V` | Toggle floating for the active window |
| `Super+P` | Toggle pseudo tiling |
| `Super+J` | Toggle split direction |
| `Super+F` | Toggle fullscreen |
| `Super+1` to `Super+0` | Switch to workspace 1 to 10 |
| `Super+Shift+1` to `Super+Shift+0` | Move the active window to workspace 1 to 10 |
| `Super+Mouse wheel` | Move between workspaces |
| `Super+Arrow keys` | Move focus between windows |
| `Super+Shift+Arrow keys` | Resize the active window |
| `Super+Alt+Arrow keys` | Swap the active window with its neighbor |
| `Super+S` | Show or hide the `magic` special workspace |
| `Super+Shift+S` | Move the active window to the `magic` special workspace |
| `Super+Z` | Save a screenshot of the whole screen |
| `Super+Shift+Z` | Save a screenshot of a selected area |
| `Super+Shift+V` | Open clipboard history |
| `Super+L` | Switch between dark and light theme |
| `Super+M` | Exit Hyprland |
| `Super+Left mouse drag` | Move a window |
| `Super+Right mouse drag` | Resize a window |
| `Escape` | Close the launcher |

The volume, microphone, brightness and media keys are bound as well. Touchpad gestures use three fingers: swipe left or right to change workspace, swipe up for fullscreen, and swipe down to float a window.

Screenshots are saved to `~/Pictures/Screenshots`.

## Theme switching

Pressing `Super+L` runs `~/.local/bin/toggle-theme.sh`, which:

1. Switches between dark and light mode and remembers the mode in `/tmp`.
2. Replaces the Waybar stylesheet with the matching file from `waybar/styles/`, then reloads Waybar.
3. Runs `toggle-gtk-theme.sh`, which updates the GTK 3 and GTK 4 settings and CSS and replaces the wofi stylesheet.

Dark mode uses the Nordic theme for GTK 3 and the Catppuccin theme for GTK 4. Light mode uses Adwaita.

## Repository layout

Each configuration file has one job:

| File | Purpose |
| --- | --- |
| `hyprland.lua` | Loads all other modules |
| `monitors.lua` | Display modes and placement |
| `env.lua` | Environment variables such as cursor theme |
| `autostart.lua` | Applications started at login |
| `programs.lua` | Default terminal, file manager and launcher |
| `look.lua` | Gaps, borders, rounding, blur, animations and workspace rules |
| `input.lua` | Keyboard, touchpad and gestures |
| `keybindings.lua` | Keyboard and mouse bindings |
| `rules.lua` | Window and layer rules |
| `permissions.lua` | Permissions for screen capture and related tools |
| `misc.lua` | General Hyprland behavior |
| `hyprpaper.conf` | Wallpaper |

## Customization

- To change colors, edit `style.css` in `waybar`, `wofi`, `dunst` or the GTK folders.
- To change what appears in the bar, edit `config.jsonc` in `waybar`.
- To change animations or layout, edit `look.lua`.
- To add a keybinding, add a line to `keybindings.lua` using the same format as the existing lines.

After editing Hyprland files, reload the configuration with the Hyprland reload command. Restart Waybar or dunst after changing their files.

## Troubleshooting

**Hyprland does not start.** Confirm the version with `hyprctl version`. Versions older than 0.55 cannot load `hyprland.lua`. Then read `~/.cache/hyprland/hyprland.log`.

**The bar is missing.** Run `waybar` in a terminal to see the error. Check that the Waybar configuration is in `~/.config/waybar`.

**Notifications do not appear.** Run `dunst` in a terminal to see the error. Install `libnotify` so that `notify-send` works.

**Screenshots fail.** Confirm that `grim` and `grimblast` are installed and that `~/Pictures/Screenshots` exists.

**Icons or fonts look wrong.** Confirm that `papirus-icon-theme` and `ttf-jetbrains-mono-nerd` are installed, then restart Hyprland.

**The theme toggle does nothing or the GTK colors do not change.** Confirm that the Nordic and Catppuccin themes are installed in the expected locations.

**Monitors are in the wrong place.** Run `hyprctl monitors` and update `monitors.lua` with the correct output names.

## License

This configuration is released under the MIT License.
