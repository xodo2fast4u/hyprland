# Hyprland Desktop

A dark, keyboard driven desktop for Hyprland on Arch Linux. The setup is split into separate repositories so each part can be installed or replaced on its own: the compositor configuration, a Waybar theme, a wofi launcher theme, dunst notifications, GTK 3 and GTK 4 themes and a few helper scripts.

![Desktop screenshot](https://imglink.cc/cdn/Ssd0DHapYA.png)

## What is included

| Repository                                        | Installs to         | Purpose                                                          |
| ------------------------------------------------- | ------------------- | ---------------------------------------------------------------- |
| [hypr](https://github.com/xodo2fast4u/hypr)       | `~/.config/hypr`    | Hyprland configuration, split into Lua modules                   |
| [waybar](https://github.com/xodo2fast4u/waybar)   | `~/.config/waybar`  | Top bar layout, styling and dark and light stylesheets           |
| [wofi](https://github.com/xodo2fast4u/wofi)       | `~/.config/wofi`    | Application launcher styling                                     |
| [dunst](https://github.com/xodo2fast4u/dunst)     | `~/.config/dunst`   | Notification daemon settings                                     |
| [gtk-3.0](https://github.com/xodo2fast4u/gtk-3.0) | `~/.config/gtk-3.0` | GTK 3 theme settings, dark and light CSS, file manager bookmarks |
| [gtk-4.0](https://github.com/xodo2fast4u/gtk-4.0) | `~/.config/gtk-4.0` | GTK 4 theme settings and light and dark CSS                      |
| [bin](https://github.com/xodo2fast4u/bin)         | `~/.local/bin`      | Theme switching and volume scripts                               |

## Requirements

- Arch Linux. The package names and installation notes below use Arch's official repositories.
- Hyprland 0.55 or newer. This configuration uses Hyprland's Lua configuration API. Check the installed version with `hyprctl version`.
- The packages listed in the installation section. The Archinstall profile supplies some of them; the rest are additional packages for this setup.

## Installation

### 1. Install Arch Linux

During installation, choose the **Hyprland** profile in `archinstall` if you want its starting package set. In the current profile source checked on October 29, 2026, the profile lists:

`hyprland`, `dunst`, `kitty`, `uwsm`, `dolphin`, `wofi`, `xdg-desktop-portal-hyprland`, `qt5-wayland`, `qt6-wayland`, `polkit-kde-agent`, `grim`, `htop`, `nano`, `openssh`, `smartmontools`, `vim`, `wget` and `slurp`.

The profile defaults to SDDM as its greeter. Profile contents can change between archinstall releases.

The profile packages provide separate parts of the desktop:

| Packages                            | Role                                                                                                             |
| ----------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| `hyprland`, `uwsm`                  | Wayland compositor and optional session manager.                                                                 |
| `dolphin`, `kitty`, `wofi`, `dunst` | File manager, terminal, application launcher and notification daemon. This setup uses Thunar instead of Dolphin. |
| `grim`, `slurp`                     | Screenshot capture and screen-region selection.                                                                  |
| `xdg-desktop-portal-hyprland`       | Desktop portal integration, including Hyprland screen capture and sharing support.                               |
| `qt5-wayland`, `qt6-wayland`        | Wayland platform support for Qt 5 and Qt 6 applications.                                                         |
| `polkit-kde-agent`                  | Graphical authentication prompts for applications that request elevated privileges.                              |

You normally do not need to install each package's dependencies yourself: `pacman` resolves required dependencies when installing a package. Optional features can need separate packages, as with Thunar archives, trash, network backends and MTP below.

For my install, I also chose the `linux-lts` kernel, `Systemd-boot`, `PipeWire` for audio, `power-profiles-daemon` for power profiles and `ufw` for the firewall. I added the `noto-fonts`, `noto-fonts-extra` and `ttf-dejavu` font packages. These are installation choices, not requirements imposed by this dotfiles repository. Configure services such as UFW according to your needs; installing a firewall package alone does not mean you have configured its rules.

Archinstall can also install additional packages during setup. I did not use that option, then installed the extras for this desktop afterward. You can choose either approach. Dolphin is included in the Hyprland profile but this setup uses Thunar; Remove Dolphin after installing the additional packages Thunar needs.

### 2. Install this setup's additional packages

Update the system:

```bash
sudo pacman -Syu
```

Install the official-repository packages used by this setup. Packages already present from your profile will be left alone by `--needed` flag:

```bash
sudo pacman -S --needed hyprpaper waybar wofi dunst copyq gnome-keyring kitty thunar grim brightnessctl playerctl wireplumber libnotify pavucontrol htop papirus-icon-theme ttf-jetbrains-mono-nerd thunar-archive-plugin xarchiver p7zip unzip zip unrar gvfs gvfs-mtp
```

Install the appearance packages used by the configuration from the source you normally use:

- A Bibata cursor theme that provides `Bibata-Modern-Ice`.
- The Nordic GTK theme, which must be named `Nordic`.
- The Catppuccin Mocha Lavender GTK theme. The theme toggle expects it at `/usr/share/themes/catppuccin-mocha-lavender-standard+default`.

Install both GTK themes before using the dark/light theme toggle.

### 3. Understand the extra Thunar packages

Thunar works as a basic file manager on its own. These optional packages add the integrations used here:

| Packages                                | What they add                                                                                                         |
| --------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `thunar-archive-plugin` and `xarchiver` | Archive actions in Thunar's context menu. The plugin adds the menu actions; Xarchiver handles the archive operations. |
| `p7zip`, `unzip`, `zip`, `unrar`        | Backends for common archive formats. Support varies by format and installed backend.                                  |
| `gvfs`                                  | Trash and removable-volume support, plus the base for browsing virtual and remote filesystems.                        |
| `gvfs-mtp`                              | MTP support for compatible phones and media devices connected over USB.                                               |

For network shares, install the matching GVFS backend too, such as `gvfs-smb` for SMB/CIFS shares. Having `gvfs` alone does not provide every network protocol. `gvfs-mtp` handles file transfer over MTP; it is not the same as Android USB debugging.

### 4. Remove Hyprland's generated config, then clone

On its first start, Hyprland creates an example configuration in `~/.config/hypr` if one is not already present. The repository needs that directory for its own configuration, so remove the generated directory before cloning it:

```bash
rm -rf ~/.config/hypr
```

This permanently deletes that directory. Only run the command if it contains the generated/default Hyprland config and nothing you need to keep. If you have personal changes there, back them up or move the directory aside instead.

My first-run experience also had an unexpected `Super+V` behavior: Kitty opened with stray text in its input using `Super + Q` does not open Kitty even though it is supposed to. I cleared it with `Ctrl+U`, installed this repository's config, then logged out and back in. With this config loaded, `Super+Q` opens Kitty as intended.

Clone the repositories:

```bash
mkdir -p ~/.local/bin ~/Pictures/Screenshots
```

```bash
git clone https://github.com/xodo2fast4u/hypr ~/.config/hypr
```

```bash
git clone https://github.com/xodo2fast4u/waybar ~/.config/waybar
```

```bash
git clone https://github.com/xodo2fast4u/wofi ~/.config/wofi
```

```bash
git clone https://github.com/xodo2fast4u/dunst ~/.config/dunst
```

```bash
git clone https://github.com/xodo2fast4u/gtk-3.0 ~/.config/gtk-3.0
```

```bash
git clone https://github.com/xodo2fast4u/gtk-4.0 ~/.config/gtk-4.0
```

```bash
git clone https://github.com/xodo2fast4u/bin ~/.local/bin/
```

```bash
chmod +x ~/.local/bin/*.sh
```

### 5. Change the machine-specific values

The configuration was written for one machine. Change these before the first login:

| File                             | What to change                                                                            |
| -------------------------------- | ----------------------------------------------------------------------------------------- |
| `~/.config/hypr/monitors.lua`    | Output name, resolution and refresh rate. Run `hyprctl monitors` to list your outputs.    |
| `~/.config/hypr/hyprpaper.conf`  | Wallpaper path and monitor name. Remove the `wallpaper` block to run without a wallpaper. |
| `~/.config/hypr/programs.lua`    | Terminal, file manager and launcher commands.                                             |
| `~/.config/hypr/autostart.lua`   | Applications started at login. Remove any you do not use.                                 |
| `~/.config/hypr/keybindings.lua` | Path to `copyq` if it is not at `/usr/bin/copyq`.                                         |
| `~/.config/hypr/input.lua`       | Keyboard layout and touchpad options.                                                     |
| `~/.config/hypr/env.lua`         | Cursor theme name and size.                                                               |
| `~/.config/hypr/permissions.lua` | Paths of screenshot tools if they are not in `/usr/bin` or `/usr/local/bin`.              |
| `~/.config/gtk-3.0/bookmarks`    | File manager bookmarks. Replace the paths with your own folders or delete the file.       |

Hyprland loads `hyprland.lua` as its entry point. The file named `hyprland` with no extension in the hypr repository is empty and can be deleted.

### 6. Log in

Log out of your current session, select Hyprland from the display manager or run `Hyprland` from a TTY.

If something does not start, check the logs:

```bash
cat ~/.cache/hyprland/hyprland.log
journalctl --user -b -e
```

Hyprpaper is not included in the archinstall Hyprland profile. This configuration uses it for the wallpaper; without `hyprpaper` installed and running, the configured wallpaper will not appear and Hyprland's default background will remain visible.

## Keybindings

The main modifier is `Super`, the Windows key.

| Binding                            | Action                                                  |
| ---------------------------------- | ------------------------------------------------------- |
| `Super+Q`                          | Open the terminal                                       |
| `Super+E`                          | Open the file manager                                   |
| `Super+R`                          | Open the application launcher                           |
| `Super+C`                          | Close the active window                                 |
| `Super+V`                          | Toggle floating for the active window                   |
| `Super+P`                          | Toggle pseudo tiling                                    |
| `Super+J`                          | Toggle split direction                                  |
| `Super+F`                          | Toggle fullscreen                                       |
| `Super+1` to `Super+0`             | Switch to workspace 1 to 10                             |
| `Super+Shift+1` to `Super+Shift+0` | Move the active window to workspace 1 to 10             |
| `Super+Mouse wheel`                | Move between workspaces                                 |
| `Super+Arrow keys`                 | Move focus between windows                              |
| `Super+Shift+Arrow keys`           | Resize the active window                                |
| `Super+Alt+Arrow keys`             | Swap the active window with its neighbor                |
| `Super+S`                          | Show or hide the `magic` special workspace              |
| `Super+Shift+S`                    | Move the active window to the `magic` special workspace |
| `Super+Z`                          | Save a screenshot of the whole screen                   |
| `Super+Shift+Z`                    | Save a screenshot of a selected area                    |
| `Super+Shift+V`                    | Open clipboard history                                  |
| `Super+L`                          | Switch between dark and light theme                     |
| `Super+M`                          | Exit Hyprland                                           |
| `Super+Left mouse drag`            | Move a window                                           |
| `Super+Right mouse drag`           | Resize a window                                         |
| `Escape`                           | Close the launcher                                      |

The volume, microphone, brightness and media keys are bound as well. Touchpad gestures use three fingers: swipe left or right to change workspace, swipe up for fullscreen and swipe down to float a window.

Screenshots are saved to `~/Pictures/Screenshots`.

## Theme switching

Pressing `Super+L` runs `~/.local/bin/toggle-theme.sh`, which:

1. Switches between dark and light mode and remembers the mode in `/tmp`.
2. Replaces the Waybar stylesheet with the matching file from `waybar/styles/`, then reloads Waybar.
3. Runs `toggle-gtk-theme.sh`, which updates the GTK 3 and GTK 4 settings and CSS and replaces the wofi stylesheet.

Dark mode uses the Nordic theme for GTK 3 and the Catppuccin theme for GTK 4. Light mode uses Adwaita.

## Repository layout

Each configuration file has one job:

| File              | Purpose                                                       |
| ----------------- | ------------------------------------------------------------- |
| `hyprland.lua`    | Loads all other modules                                       |
| `monitors.lua`    | Display modes and placement                                   |
| `env.lua`         | Environment variables such as cursor theme                    |
| `autostart.lua`   | Applications started at login                                 |
| `programs.lua`    | Default terminal, file manager and launcher                   |
| `look.lua`        | Gaps, borders, rounding, blur, animations and workspace rules |
| `input.lua`       | Keyboard, touchpad and gestures                               |
| `keybindings.lua` | Keyboard and mouse bindings                                   |
| `rules.lua`       | Window and layer rules                                        |
| `permissions.lua` | Permissions for screen capture and related tools              |
| `misc.lua`        | General Hyprland behavior                                     |
| `hyprpaper.conf`  | Wallpaper                                                     |

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

**Screenshots fail.** Confirm that `grim` and `slurp` are installed and that `~/Pictures/Screenshots` exists.

**Icons or fonts look wrong.** Confirm that `papirus-icon-theme` and `ttf-jetbrains-mono-nerd` are installed, then restart Hyprland.

**The theme toggle does nothing or the GTK colors do not change.** Confirm that the Nordic and Catppuccin themes are installed in the expected locations.

**Monitors are in the wrong place.** Run `hyprctl monitors` and update `monitors.lua` with the correct output names.

## License

This configuration is released under the MIT License.
