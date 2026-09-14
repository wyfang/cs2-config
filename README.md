# CS2 Config

一套按 Steam 目录组织的 CS2 个人配置，包含按键、SOCD 移动、准星、画面设置与 Windows 备份同步脚本。

[English](./README.en.md)

## 功能

### Wi-Fi SOCD

SOCD 用于处理同时按下相反方向键的情况。`wifi-socd.cfg` 为 `A` / `D`（左右）和 `W` / `S`（前后）分别配置“最后输入优先”的移动逻辑，两组方向独立处理。

例如，按住 `A` 后再按 `D`，脚本将方向切换为向右；松开 `D` 时，如果 `A` 仍按住，则恢复向左；两键都松开后，停止该方向轴的输入。`W` / `S` 使用相同逻辑。

### 主要按键

| 按键 | 功能 |
| --- | --- |
| `W` / `A` / `S` / `D` | SOCD 移动，相反方向键采用最后输入优先 |
| `/` / `Mouse4` | 麦克风常开切换 / 按住说话 |
| `O` | 在跟随后坐力准星与常规准星之间切换 |
| `Mouse5` | 切换两套持枪视角 |
| `V` | 标记位置并发送中英文警告 |
| `\` | 在 10% 与 100% 主音量之间切换 |
| `.` | 切换 `voice_loopback` |
| `Caps Lock` | 切换左右手持枪 |
| `F5`–`F8` | 循环发送预设聊天内容 |
| `K` / `Alt` | 仅在允许作弊的环境执行模型或调试命令 |

## 使用

1. 先备份现有设置，再下载仓库并将 `steamapps`、`userdata` 复制到 Steam 根目录，例如 `C:\Steam`。
2. 将 `userdata/89582913` 替换为自己的 Steam userdata ID；处理主账号的脚本中也要修改固定账号 ID。
3. 启动 CS2，在控制台执行 `exec autoexec`，或确认游戏已自动加载 `autoexec.cfg`。
4. 建议将 `cs2_video.txt` 设为只读，以保留仓库中的画面参数。

主要位置：

- 游戏脚本：`steamapps/common/Counter-Strike Global Offensive/game/csgo/cfg`
- 用户配置：`userdata/<Steam userdata ID>/730`

### SOCD 配置

`autoexec.cfg` 已包含 `exec wifi-socd`，加载主配置时会自动加载 SOCD。也可在控制台执行 `exec wifi-socd` 单独加载。

加载后会覆盖 `W` / `A` / `S` / `D` 的绑定，并将 `joy_side_sensitivity` 和 `joy_forward_sensitivity` 设为 `1`。配置用于个人测试与研究，实际效果以当前游戏版本和服务器行为为准。

### Windows 脚本

| 脚本 | 用途 |
| --- | --- |
| `备份730逐个文件和CFG后启动Steam.bat` | 按 `730-Original` 清单备份主账号与 CFG，各保留最近 5 份时间戳快照，然后启动 Steam |
| `同步主账号730.bat` | 删除主账号现有 `730`，从 OneDrive 完整恢复，再覆盖共享 CFG |
| `同步所有账号730-Onedrive.bat` | 对本机全部真实 Steam 账号执行同一份 `730` 恢复 |

## 说明

同步脚本固定使用 `C:\Steam` 与 `%OneDrive%\CS2`。运行前必须完全退出 Steam，并确认 OneDrive 已同步完成；两个同步脚本会永久删除目标账号原有的整个 `730`，不会逐文件合并。

完整绑定以 `autoexec.cfg` 与 `wifi-*.cfg` 为准。`730-Original` 是备份文件的唯一清单；清单变化会自动反映到下一次备份，恢复脚本则始终复制完整恢复源。

## 版权说明

原创代码依据 [Apache License 2.0](./LICENSE) 发布。Valve 与 Counter-Strike 名称、格式、游戏内容及用户 Steam 数据不在许可范围内。

许可边界见[许可范围](./LICENSE_SCOPE.md)。
