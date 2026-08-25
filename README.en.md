# CS2 Config

A personal CS2 configuration arranged as a Steam directory tree, with key bindings, crosshairs, video settings, and Windows backup scripts.

[简体中文](./README.md) · English

## Install

1. Copy `steamapps` and `userdata` into the Steam root, such as `C:\Steam`.
2. Replace `userdata/89582913` with your own Steam userdata ID.
3. Start CS2 and run `exec autoexec`, or confirm it loads automatically.
4. Making `cs2_video.txt` read-only is recommended if you want to preserve these video settings.

Main locations:

- Game scripts: `steamapps/common/Counter-Strike Global Offensive/game/csgo/cfg`
- User data: `userdata/<Steam userdata ID>/730`

## Windows scripts

| Script | Purpose |
| --- | --- |
| `备份730逐个文件和CFG后启动Steam.bat` | Backs up files listed in `730-Original` and CFG files, retains the five latest snapshots, then starts Steam |
| `同步主账号730.bat` | Deletes the primary account's current `730`, restores it from OneDrive, then copies shared CFG files |
| `同步所有账号730-Onedrive.bat` | Performs the same full `730` restoration for every detected Steam account |

The synchronization scripts permanently delete the target `730` directory before restoring it. Exit Steam and confirm OneDrive has finished syncing before use.

## License

Original code and code documentation are licensed under the [Apache License 2.0](./LICENSE). Personal material, branding, game content, and third-party material are excluded. See [NOTICE](./NOTICE) and [LICENSE_SCOPE.md](./LICENSE_SCOPE.md).
