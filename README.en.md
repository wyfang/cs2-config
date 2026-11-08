# CS2 Config

> [!TIP]
> **For players in China:** In your Steam library, right-click CS2 → **Properties → General → Launch Options** and add `-promptperfectworld`. Each launch will display a choice between the Perfect World (China) and international versions, so you can select the region manually and avoid system proxy settings affecting automatic region selection.

A personal CS2 configuration arranged as a Steam directory tree, with key bindings, optional SOCD movement, crosshairs, video settings, and Windows backup and synchronization scripts.

[简体中文](./README.md)

## Features

### Wi-Fi SOCD

The supplied key configuration is saved with SOCD OFF. Each complete press and release of `I` advances one stage in `ON → RESET → OFF → RESET → ON`. From the supplied OFF state, the first press enters RESET and the second enables SOCD. RESET pauses WASD. Team messages report `SOCD-ON`, `SOCD-RESET`, or `SOCD-OFF`.

ON applies last input priority independently to `A` / `D` and `W` / `S`. For example, holding `A` and then pressing `D` switches right; releasing `D` while still holding `A` restores left. OFF uses ordinary opposing-key cancellation. Both modes share the `forwardback` / `rightleft` input path. Reloading the main configuration retains the saved mode or RESET stage.

### Key bindings

| Key | Action |
| --- | --- |
| `W` / `A` / `S` / `D` | ON: last input priority; OFF: opposing keys cancel; RESET: movement input paused |
| `/` / `Mouse4` | Toggle an open microphone / push to talk |
| `I` | Each complete press/release advances ON → RESET → OFF → RESET → ON, sends the corresponding team message, and saves |
| `O` | Cycle through three crosshair presets; all follow recoil |
| `P` | Reload `autoexec.cfg` and restore the startup viewmodel, crosshair, and HUD color while preserving the current SOCD state |
| `Mouse5` | Switch between two viewmodels with their matching crosshair and HUD color |
| `V` | Mark a position and send warnings in Chinese and English |
| `\` | Set master volume to 10% / 100% and send `VOLUME-10%` / `VOLUME-100%` to team chat |
| `.` | Toggle microphone self-monitoring and send `MIC-LOOPBACK-ON` / `MIC-LOOPBACK-OFF` to team chat |
| `Caps Lock` | Switch the weapon between left and right hands |
| `F5`–`F7` | Cycle preset chat messages |
| `F8` | Send the preset message explaining an unsuccessful commendation |
| `F9` / `F10` | Send the C4 countdown message for 7 seconds / 12 seconds |
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

`autoexec.cfg` loads `wifi-socd-core.cfg`, defining both movement modes and the I-key phase handlers without overwriting saved WASD or `I` bindings. `I` replaces `show_loadout_toggle` (the loadout display toggle). Copy the main configuration and all six SOCD files together when updating:

| File | Purpose |
| --- | --- |
| `autoexec.cfg` | Load core definitions at startup while preserving saved bindings |
| `wifi-socd-core.cfg` | Define last input priority, ordinary opposing-key cancellation, the RESET no-op, and I-key down/up handlers |
| `wifi-socd.cfg` | Enter ON, bind `+wifi-*`, and send `SOCD-ON` |
| `wifi-socd-off.cfg` | Enter OFF, bind `+wifi-normal-*`, and send `SOCD-OFF` |
| `wifi-socd-reset-to-off.cfg` | Handle ON → RESET key-down; the following stage is OFF |
| `wifi-socd-reset-to-on.cfg` | Handle OFF → RESET key-down; the following stage is ON |
| `wifi-socd-reset.cfg` | Console-only full movement recovery, ending in OFF |

Pressing `I` in ON/OFF executes the matching reset-to file: it issues only `forwardback 0 0 0` and binds WASD to a no-op. Releasing that same I press issues only `rightleft 0 0 0`, then selects the corresponding RESET continuation, saves the configuration, and sends `SOCD-RESET`. The next complete I press/release enters OFF or ON on release. Each I down/up event therefore issues at most one movement-axis command. I is rebound only on release so its down/up handlers remain paired.

WASD being paused during RESET is intentional. The cycle does not use `exec_async` or unbind I. ON and OFF use the same analog axes, avoiding mixed digital-button and analog-axis movement state across switches. The core definitions set `joy_side_sensitivity` and `joy_forward_sensitivity` to `1`.

For first-time installation, outdated bindings, or recovery from residual movement input, release WASD and run this in the console:

```text
exec wifi-socd-reset
```

This console recovery script clears digital movement buttons and both analog axes, then restores OFF and a working I binding. Do not bind it directly to I: it contains multiple movement commands. To recover and immediately enable SOCD, run in the console:

```text
exec wifi-socd-reset; exec wifi-socd
```

With WASD released and input state normal, `exec wifi-socd` / `exec wifi-socd-off` can also select a mode directly; these two files do not perform full input recovery. ON/OFF and entry into RESET run `host_writeconfig` to save current settings and WASD/I bindings. On the next launch, core definitions are loaded again and the saved mode or RESET stage is retained. User key configuration files must be writable; replacing `userdata` or cloud synchronization overwriting local files can change the saved state.

Running `exec autoexec` or pressing `P` preserves the current binding stage; release movement keys before reloading. No launch option is needed to force a mode. Remove old `+exec wifi-socd` / `+exec wifi-socd-off` options so they do not override the saved choice. If updating game CFG files without copying the supplied account bindings, run the console recovery command once to install the current I cycle.

This version completed two cycles in the local game, including tests with W or S held while pressing I. That validation does not cover every game version or server environment; actual behavior depends on the current environment.

### Volume and microphone self-monitoring

`\` runs `wifi-volume-low.cfg` / `wifi-volume-high.cfg`, setting master volume to `0.1` / `1` and sending `VOLUME-10%` / `VOLUME-100%` to team chat. `.` runs `wifi-loopback-on.cfg` / `wifi-loopback-off.cfg`, setting `voice_loopback` to `1` / `0` and sending `MIC-LOOPBACK-ON` / `MIC-LOOPBACK-OFF`. Self-monitoring lets you hear your own microphone; it does not enable or disable speaking to teammates.

Each state file rebinds its key to the opposite state and runs `host_writeconfig`. Copy all five audio files: `wifi-audio-keys.cfg`, `wifi-volume-*.cfg`, and `wifi-loopback-*.cfg`. If you update only the game scripts, or the running game still has the old bindings in memory, run this once:

```text
exec wifi-audio-keys
```

The installer sets the next `\` / `.` target to low volume / self-monitoring on without changing current audio values. Normal switching saves the next target, so do not reinstall on every main configuration reload. Team messages report the value explicitly set by the script; changing audio values elsewhere does not change the next bound target.

### Video settings on a new computer

Video settings reside in `userdata/<Steam userdata ID>/730/local/cfg/cs2_video.txt`, separately from the game's `autoexec.cfg`. The current preset includes `1920×1080`, a borderless window, disabled VSync, `4`-sample MSAA, and independent shadow, texture, and particle settings; it mixes quality levels.

The file also records `VendorID`, `DeviceID`, `Version`, and `Autoconfig`. A different GPU or game version may trigger hardware detection and rewrite the settings. Closing the game and Steam before restoring is recommended; confirm the actual signed-in account and Steam installation's `730/local/cfg` path, then compare `cs2_video.txt` before and after the first launch. The scripts default to `C:\Steam`; adjust them for other installation paths. Copying only `game/csgo/cfg` does not restore this video file.

Do not force the old GPU identifiers as universal values across computers. Let the new computer complete hardware detection, verify and apply the desired video settings in-game, then back up its generated file. Read-only status only restricts writes; it does not guarantee acceptance of an old computer's settings and prevents saving later video changes.

### O-key crosshairs

Startup loads `wifi-crosshair1.cfg`; pressing `O` then cycles through `2 → 3 → 1`. All three presets follow recoil and use pixel parameters that scale with resolution:

| Preset | Style and color | Length / thickness / gap (file values) | Opacity / outline |
| --- | --- | --- | --- |
| `wifi-crosshair1.cfg` | Dynamic Cross (Legacy), style `2`, orange RGB `255/150/0` | `110 / 32 / 128` | `255` / Full Outline |
| `wifi-crosshair2.cfg` | Static Circle, style `3`, white RGB `255/255/255` | `120 / 32 / 9999` | `255` / Full Outline |
| `wifi-crosshair3.cfg` | White dot (style `4` with zero bar length) | `0 / 4 / 0` | `255` / Full Outline |

Preset 1 uses legacy dynamic splitting with threshold `10`, outer-bar length ratio `0.9`, and inner/outer alpha multipliers of `1`; its outline is opaque black. Only preset 1 contains concise technical comments and confirmed ranges; presets 2 and 3 contain commands only. Preset 2 uses Static Circle (style `3`), so bar length and legacy split parameters do not control its circle appearance. Its gap input `9999` is preserved; the effective value depends on the engine's accepted range. Presets 2 and 3 inherit the current outline RGBA because they do not explicitly set it.

Each preset also explicitly sets the same 15 additional options:

| Group | Current values |
| --- | --- |
| Scope dot | Do not use crosshair color; scale `1` |
| Observed crosshairs | Show everyone's crosshair; use your own when the bot can be taken over |
| Grenade line-up reticles | Flash, HE, fire, smoke, and decoy disabled; retain regular crosshair enabled |
| Grenade reticle delays | `1` second after pulling the pin for all five types |

These are global client variables. Writing identical values in every preset keeps these preferences consistent when switching.

For `Mouse5`, mode 1 loads crosshair 1 and HUD color `11`; mode 2 loads crosshair 2 and HUD color `0`. Each mode sets the next `O` press to the following preset. Pressing `P` reloads the main configuration and restores mode 1.

To update, copy all three `wifi-crosshair*.cfg` files together with the main configuration and SOCD files into the game's `game/csgo/cfg`, then run `exec autoexec` to reload the bindings. You can also load a preset directly with `exec wifi-crosshair1`, `exec wifi-crosshair2`, or `exec wifi-crosshair3`. Resolution changes scale the existing sizes; reloading a preset reapplies its pixel values at the current resolution.

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
