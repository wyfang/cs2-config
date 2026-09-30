# CS2 Config

A personal CS2 configuration arranged as a Steam directory tree, with key bindings, optional SOCD movement, crosshairs, video settings, and Windows backup and synchronization scripts.

[简体中文](./README.md)

## Features

### Wi-Fi SOCD

The supplied key configuration initially uses standard `W` / `A` / `S` / `D` movement; subsequent launches retain the last saved SOCD choice. Pressing `I` or running `exec wifi-socd` in the console enables last input priority for `A` / `D` (left and right) and `W` / `S` (forward and backward), handling the two axes independently. Separate files enable and disable SOCD; reloading the main configuration preserves the enabled or disabled choice.

Once enabled, for example, holding `A` and then pressing `D` switches the configured direction to the right. Releasing `D` while still holding `A` restores the leftward input; releasing both keys stops input on that axis. `W` / `S` follows the same logic.

### Key bindings

| Key | Action |
| --- | --- |
| `W` / `A` / `S` / `D` | Standard movement initially; subsequently retain the last saved SOCD choice |
| `/` / `Mouse4` | Toggle an open microphone / push to talk |
| `I` | Toggle SOCD, replacing the previous loadout display toggle; retain the saved choice |
| `O` | Cycle through three crosshair presets; all follow recoil |
| `P` | Reload `autoexec.cfg` and restore the startup viewmodel, crosshair, and HUD color while preserving the current SOCD state |
| `Mouse5` | Switch between two viewmodels with their matching crosshair and HUD color |
| `V` | Mark a position and send warnings in Chinese and English |
| `\` | Toggle master volume between 10% and 100% |
| `.` | Toggle `voice_loopback` |
| `Caps Lock` | Switch the weapon between left and right hands |
| `F5`–`F8` | Cycle preset chat messages |
| `K` / `Alt` | Run model or debugging commands only where cheats are allowed |

## Usage

1. Back up existing settings, then copy `steamapps` and `userdata` into the Steam root, such as `C:\Steam`.
2. Replace `userdata/89582913` with your own Steam userdata ID. Also update the hard-coded account ID in scripts that target the primary account.
3. Start CS2 and run `exec autoexec`, or confirm that `autoexec.cfg` loads automatically.
4. On a new computer, verify the video file path and loaded settings as described below; `exec autoexec` does not restore the video settings in `cs2_video.txt`.

Main locations:

- Game scripts: `steamapps/common/Counter-Strike Global Offensive/game/csgo/cfg`
- User data: `userdata/<Steam userdata ID>/730`

### SOCD configuration

`autoexec.cfg` loads the runtime command definitions in `wifi-socd-core.cfg` without overwriting WASD or `I` bindings. Press `I` to alternate between enabling and disabling SOCD, replacing `show_loadout_toggle` (the loadout display toggle). You can also enable it directly in the console:

```text
exec wifi-socd
```

Enabling it replaces the `W` / `A` / `S` / `D` bindings and prints the `Wi-Fi SOCD ON!!` banner after saving the configuration. The core definitions file sets `joy_side_sensitivity` and `joy_forward_sensitivity` to `1`. This configuration is intended for personal testing and research; actual behavior depends on the current game version and server.

To disable SOCD, run:

```text
exec wifi-socd-off
```

The off file clears directional input, restores standard WASD, and prints the `Wi-Fi SOCD OFF!!` banner after saving the configuration, without changing the viewmodel, crosshair, or HUD color. The on file binds `I` to `exec wifi-socd-off`; the off file binds it to `exec wifi-socd`. Both switch files run `host_writeconfig` to save the current game configuration, including WASD and `I` bindings. On the next launch, the main configuration prepares the SOCD command definitions again and retains the saved bindings, restoring the previous choice. The user key configuration must be writable; replacing `userdata` or a cloud sync that overwrites local configuration can also change the saved choice.

Running `exec autoexec` or pressing `P` reloads the main configuration without changing whether SOCD is enabled. Because the script's key handling commands are reinitialized, release all movement keys before enabling, disabling, or reloading.

No launch option is needed to force either switch file. Remove any previously added `+exec wifi-socd-off` or `+exec wifi-socd` so it does not override the saved choice. If you update only the game CFG files without copying the supplied account key configuration, run `exec wifi-socd` or `exec wifi-socd-off` once to select the current state and establish the `I` binding. Copy `autoexec.cfg`, `wifi-socd-core.cfg`, `wifi-socd.cfg`, and `wifi-socd-off.cfg` together when updating the game configuration.

### Video settings on a new computer

Video settings reside in `userdata/<Steam userdata ID>/730/local/cfg/cs2_video.txt`, separately from the game's `autoexec.cfg`. The current preset includes `1920×1080`, a borderless window, disabled VSync, `4`-sample MSAA, and independent shadow, texture, and particle settings; it mixes quality levels.

The file also records `VendorID`, `DeviceID`, `Version`, and `Autoconfig`. A different GPU or game version may trigger hardware detection and rewrite the settings. Closing the game and Steam before restoring is recommended; confirm the actual signed-in account and Steam installation's `730/local/cfg` path, then compare `cs2_video.txt` before and after the first launch. The scripts default to `C:\Steam`; adjust them for other installation paths. Copying only `game/csgo/cfg` does not restore this video file.

Do not force the old GPU identifiers as universal values across computers. Let the new computer complete hardware detection, verify and apply the desired video settings in-game, then back up its generated file. Read-only status only restricts writes; it does not guarantee acceptance of an old computer's settings and prevents saving later video changes.

### O-key crosshairs

Startup loads `wifi-crosshair1.cfg`; pressing `O` then cycles through `2 → 3 → 1`. All three presets follow recoil and use the new pixel commands. Their current values are:

| Preset | Style and color | Length / thickness / gap (pixels) | Opacity / outline |
| --- | --- | --- | --- |
| `wifi-crosshair1.cfg` | Dynamic Quad, yellow (RGB `255/225/0`) | `120 / 31 / 999` | `180` / full outline |
| `wifi-crosshair2.cfg` | Dynamic Quad, white, currently fully transparent | `120 / 31 / 128` | `0` / no outline |
| `wifi-crosshair3.cfg` | White dot | `0 / 4 / 0` | `255` / full outline |

For `Mouse5`, mode 1 loads crosshair 1 and HUD color `11`; mode 2 loads crosshair 2 and HUD color `0`. Each mode sets the next `O` press to the following preset. `P` reloads the main configuration and restores mode 1.

To update, copy all three `wifi-crosshair*.cfg` files together with the main configuration and SOCD files listed above into the game's `game/csgo/cfg`, then run `exec autoexec` in the console to reload the bindings. Sizes are applied as pixels at the current resolution when a preset loads. The game scales them proportionally after a resolution change; loading a preset again reapplies its configured pixel values. Some comments in the original files no longer match their values; the table reflects the actual configuration. Crosshair 2 retains opacity `0` from the latest backup.

### Windows scripts

| Script | Purpose |
| --- | --- |
| `备份730逐个文件和CFG后启动Steam.bat` | Backs up primary-account files listed in `730-Original` and shared CFG files, retains the five latest timestamped snapshots of each, then starts Steam |
| `同步主账号730.bat` | Deletes the primary account's current `730`, fully restores it from OneDrive, then overwrites shared CFG files |
| `同步所有账号730-Onedrive.bat` | Performs the same full `730` restoration for every detected real Steam account |

## Notes

The repository's `.gitignore` excludes the personal China-region launch record `cnlauncher.txt`, item preferences in `cs2_preferred_items.txt`, and `workshop_saves/` Workshop saves. These files can remain in private backups; the key bindings, crosshairs, and SOCD configuration do not depend on them. The ignore rules neither delete existing backups nor change what the Windows scripts restore.

The scripts use fixed paths under `C:\Steam` and `%OneDrive%\CS2`.

The backup script can run while Steam or CS2 is running; neither needs to be closed first. It copies files individually as they exist on disk when copied, without asking the game to save its settings. Settings that have not yet been written to disk are not included.

The two synchronization scripts restore configuration by permanently deleting the target account's entire existing `730` directory and copying the OneDrive restore source; they do not merge individual files. The scripts do not require Steam or CS2 to be closed, but closing the game and Steam and confirming that the OneDrive restore source has finished syncing before restoring is recommended, so the running game or cloud synchronization does not subsequently overwrite the restored files.

See `autoexec.cfg` and `wifi-*.cfg` for the full bindings. `730-Original` is the sole backup file list: changes to it apply to the next backup, while restoration always copies the complete restore source.

## License

Original code is licensed under the [Apache License 2.0](./LICENSE). Valve and Counter-Strike names, formats, game content, and your Steam data are excluded.

See [license scope](./LICENSE_SCOPE.md) for the boundaries.
