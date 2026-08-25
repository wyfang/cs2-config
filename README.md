# CS2 Config

一套按 Steam 目录组织的 CS2 个人配置，包含按键、准星、画面设置与 Windows 备份同步脚本。

简体中文 · [English](./README.en.md)

## 安装

1. 下载仓库并将 `steamapps`、`userdata` 复制到 Steam 根目录，例如 `C:\Steam`。
2. 将 `userdata/89582913` 替换为自己的 Steam userdata ID。
3. 启动 CS2，在控制台执行 `exec autoexec`，或确认游戏已自动加载 `autoexec.cfg`。
4. 建议将 `cs2_video.txt` 设为只读，以保留仓库中的画面参数。

主要位置：

- 游戏脚本：`steamapps/common/Counter-Strike Global Offensive/game/csgo/cfg`
- 用户配置：`userdata/<Steam userdata ID>/730`

## Windows 脚本

| 脚本 | 用途 |
| --- | --- |
| `备份730逐个文件和CFG后启动Steam.bat` | 按 `730-Original` 清单备份主账号与 CFG，各保留最近 5 份时间戳快照，然后启动 Steam |
| `同步主账号730.bat` | 删除主账号现有 `730`，从 OneDrive 完整恢复，再覆盖共享 CFG |
| `同步所有账号730-Onedrive.bat` | 对本机全部真实 Steam 账号执行同一份 `730` 恢复 |

同步脚本固定使用 `C:\Steam` 与 `%OneDrive%\CS2`。运行前必须完全退出 Steam，并确认 OneDrive 已同步完成；两个同步脚本会永久删除目标账号原有的整个 `730`，不会逐文件合并。

## 主要按键

| 按键 | 功能 |
| --- | --- |
| `/` / `Mouse4` | 麦克风常开切换 / 按住说话 |
| `O` | 在跟随后坐力准星与常规准星之间切换 |
| `Mouse5` | 切换两套持枪视角 |
| `V` | 标记位置并发送中英文警告 |
| `\` | 在 10% 与 100% 主音量之间切换 |
| `.` | 切换 `voice_loopback` |
| `Caps Lock` | 切换左右手持枪 |
| `F5`–`F8` | 循环发送预设聊天内容 |
| `K` / `Alt` | 仅在允许作弊的环境执行模型或调试命令 |

完整绑定以 `autoexec.cfg` 与 `wifi-*.cfg` 为准。`730-Original` 是备份文件的唯一清单；清单变化会自动反映到下一次备份，恢复脚本则始终复制完整恢复源。

## 许可

原创代码与代码文档依据 [Apache License 2.0](./LICENSE) 发布；个人素材、品牌、游戏内容与第三方内容不在许可范围内。详见 [NOTICE](./NOTICE) 与 [LICENSE_SCOPE.md](./LICENSE_SCOPE.md)。
