# CS2 Config

A personal CS2 configuration arranged as a Steam directory tree, with key bindings, crosshairs, video settings, and Windows backup and synchronization scripts.

[简体中文](./README.md)

## Features

### Key bindings

| Key | Action |
| --- | --- |
| `/` / `Mouse4` | Toggle an open microphone / push to talk |
| `O` | Switch between a recoil-following crosshair and a regular crosshair |
| `Mouse5` | Switch between two viewmodels |
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
4. Making `cs2_video.txt` read-only is recommended if you want to preserve these video settings.

Main locations:

- Game scripts: `steamapps/common/Counter-Strike Global Offensive/game/csgo/cfg`
- User data: `userdata/<Steam userdata ID>/730`

### Windows scripts

| Script | Purpose |
| --- | --- |
| `备份730逐个文件和CFG后启动Steam.bat` | Backs up primary-account files listed in `730-Original` and shared CFG files, retains the five latest timestamped snapshots of each, then starts Steam |
| `同步主账号730.bat` | Deletes the primary account's current `730`, fully restores it from OneDrive, then overwrites shared CFG files |
| `同步所有账号730-Onedrive.bat` | Performs the same full `730` restoration for every detected real Steam account |

## Notes

The synchronization scripts use fixed paths under `C:\Steam` and `%OneDrive%\CS2`. Exit Steam completely and confirm that OneDrive has finished syncing before running them. Both synchronization scripts permanently delete the target account's entire existing `730` directory; they do not merge individual files.

See `autoexec.cfg` and `wifi-*.cfg` for the full bindings. `730-Original` is the sole backup file list: changes to it apply to the next backup, while restoration always copies the complete restore source.

## License

Original code is licensed under the [Apache License 2.0](./LICENSE). Valve and Counter-Strike names, formats, game content, and your Steam data are excluded.

See [license scope](./LICENSE_SCOPE.md) for the boundaries.
