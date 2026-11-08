# CS2 Config

> [!TIP]
> **中国玩家启动提示：** 在 Steam 库中右键 CS2 → **属性 → 通用 → 启动选项**，添加 `-promptperfectworld`。每次启动 CS2 时都会弹出国服／国际服选择窗口，方便手动选择区服，避免系统代理影响自动区服选择。

一套按 Steam 目录组织的 CS2 个人配置，包含按键、可选 SOCD 移动、准星、画面设置与 Windows 备份同步脚本。

[English](./README.en.md)

## 功能

### Wi-Fi SOCD

仓库随附的按键配置当前保存为 SOCD 关闭状态，`W` / `A` / `S` / `D` 使用普通移动，首次按 `I` 执行开启；之后沿用上一次保存的开关状态。按 `I` 可交替开启和关闭，在控制台执行 `exec wifi-socd` 则直接开启。`wifi-socd.cfg` 为 `A` / `D`（左右）和 `W` / `S`（前后）启用“最后输入优先”的移动逻辑，两组方向独立处理。SOCD 使用独立文件开关，重载主配置不会改变其开关选择。

启用后，例如按住 `A` 后再按 `D`，脚本将方向切换为向右；松开 `D` 时，如果 `A` 仍按住，则恢复向左；两键都松开后，停止该方向轴的输入。`W` / `S` 使用相同逻辑。

### 主要按键

| 按键 | 功能 |
| --- | --- |
| `W` / `A` / `S` / `D` | 当前随附配置为普通 WASD；按 `I` 可启用 SOCD，并保存选择 |
| `/` / `Mouse4` | 麦克风常开切换 / 按住说话 |
| `I` | 切换 SOCD，发送 `SOCD-ON` / `SOCD-OFF` 团队消息并保存所选状态 |
| `O` | 循环切换三套准星，均跟随后坐力 |
| `P` | 重新加载 `autoexec.cfg`，恢复启动时的视角、准星与 HUD 配色，保留当前 SOCD 状态 |
| `Mouse5` | 切换两套持枪视角，同时切换对应准星与 HUD 配色 |
| `V` | 标记位置并发送中英文警告 |
| `\` | 切换 10% / 100% 主音量，发送 `VOLUME-10%` / `VOLUME-100%` 团队消息 |
| `.` | 切换麦克风自听，发送 `MIC-LOOPBACK-ON` / `MIC-LOOPBACK-OFF` 团队消息 |
| `Caps Lock` | 切换左右手持枪 |
| `F5`–`F7` | 循环发送预设聊天内容 |
| `F8` | 发送预设的点赞未完成说明 |
| `F9` / `F10` | 发送 C4 倒计时 7 秒 / 12 秒提示 |
| `K` / `Alt` | 仅在允许作弊的环境执行模型或调试命令 |

## 使用

1. 先备份现有设置，再下载仓库并将 `steamapps`、`userdata` 复制到 Steam 根目录，例如 `C:\Steam`。
2. 将 `userdata/89582913` 替换为自己的 Steam userdata ID；处理主账号的脚本中也要修改固定账号 ID。
3. 启动 CS2，在控制台执行 `exec autoexec`，或确认游戏已自动加载 `autoexec.cfg`。
4. 在新电脑上核对下节的画质文件路径与加载结果；`exec autoexec` 不会恢复 `cs2_video.txt` 中的画质设置。

主要位置：

- 游戏脚本：`steamapps/common/Counter-Strike Global Offensive/game/csgo/cfg`
- 用户配置：`userdata/<Steam userdata ID>/730`

### SOCD 配置

`autoexec.cfg` 加载 `wifi-socd-core.cfg` 准备运行所需的命令定义，但不覆盖 WASD 或 `I` 绑定。按 `I` 可交替开启和关闭 SOCD，替换原来的 `show_loadout_toggle`（装备显示切换）。也可在控制台直接开启：

```text
exec wifi-socd
```

启用后会覆盖 `W` / `A` / `S` / `D` 的绑定，保存配置后发送 `SOCD-ON` 团队消息并打印 `Wi-Fi SOCD ON!!` 图案。核心定义文件会将 `joy_side_sensitivity` 和 `joy_forward_sensitivity` 设为 `1`。配置用于个人测试与研究，实际效果以当前游戏版本和服务器行为为准。

关闭 SOCD 时执行：

```text
exec wifi-socd-off
```

关闭文件恢复普通 WASD，保存配置后发送 `SOCD-OFF` 团队消息并打印 `Wi-Fi SOCD OFF!!` 图案，不修改视角、准星或 HUD 配色。开启文件将 `I` 绑定为 `exec wifi-socd-off`，关闭文件将其绑定为 `exec wifi-socd`。两个开关文件都会执行 `host_writeconfig` 保存当前游戏配置（包含 WASD 和 `I` 绑定）；下次启动时，主配置重新准备 SOCD 命令定义，并沿用保存的绑定，从而恢复上一次选择。用户按键配置必须可写；重新覆盖 `userdata` 或云同步覆盖本地配置也可能改变保存的选择。

执行 `exec autoexec` 或按 `P` 会重载主配置，但不改变 SOCD 的开关选择。启用、停用或重载前应松开所有移动键。开关文件只重绑按键，不再连续发送多条移动清零命令，以避免同一次按键事件中的多输入冲突。

不需要在 Steam 启动选项中强制执行开关文件；若之前添加过 `+exec wifi-socd-off` 或 `+exec wifi-socd`，应移除以免覆盖保存的选择。如果只更新游戏 CFG、没有复制随附的账号按键配置，先执行一次 `exec wifi-socd` 或 `exec wifi-socd-off`，选择当前状态并建立 `I` 键绑定。更新游戏配置时应同时复制 `autoexec.cfg`、`wifi-socd-core.cfg`、`wifi-socd.cfg` 与 `wifi-socd-off.cfg`。

### 音量与麦克风自听

`\` 使用 `wifi-volume-low.cfg` / `wifi-volume-high.cfg`，分别把主音量设为 `0.1` / `1`，发送 `VOLUME-10%` / `VOLUME-100%` 团队消息。`.` 使用 `wifi-loopback-on.cfg` / `wifi-loopback-off.cfg`，分别把 `voice_loopback` 设为 `1` / `0`，发送 `MIC-LOOPBACK-ON` / `MIC-LOOPBACK-OFF`。自听指听到自己的麦克风声音，不影响向队友发言。

每份状态脚本都会将对应按键改绑为另一状态并执行 `host_writeconfig`。同时复制五份 `wifi-audio-keys.cfg`、`wifi-volume-*.cfg` 与 `wifi-loopback-*.cfg`；如果只更新游戏脚本，或游戏仍使用内存中的旧绑定，在控制台执行一次：

```text
exec wifi-audio-keys
```

安装命令仅将下一次 `\` / `.` 的目标设为低音量 / 自听开启，不改动当前音频值。正常切换后由按键保存下一目标，无需每次重载主配置时重新安装。团队消息报告脚本本次明确设置的值；通过其他方式改动音频值后，下一次按键仍会设为绑定对应的目标值。

### 画质与新电脑迁移

画质文件位于 `userdata/<Steam userdata ID>/730/local/cfg/cs2_video.txt`，与游戏目录中的 `autoexec.cfg` 分开。当前预设包含 `1920×1080`、无边框窗口、关闭垂直同步、`4` 倍 MSAA，以及阴影、纹理、粒子等独立参数，是混合画质配置。

该文件同时记录 `VendorID`、`DeviceID`、`Version` 和 `Autoconfig`；更换显卡或游戏版本后，游戏可能重新检测硬件并改写设置。迁移时建议先退出游戏与 Steam，确认复制到实际登录账号和实际 Steam 安装目录下的 `730/local/cfg`，再比较首次启动前后的 `cs2_video.txt`。脚本默认使用 `C:\Steam`，其他安装位置需要自行调整；只覆盖 `game/csgo/cfg` 无法恢复此画质文件。

不要把旧显卡标识作为跨电脑通用值强行保留。新电脑完成硬件检测后，在游戏中核对并应用所需画质，再备份该电脑生成的文件。设为只读只能限制写入，不能保证游戏接受旧电脑的参数，也会妨碍后续保存画质调整。

### O 键准星

启动加载 `wifi-crosshair1.cfg`，之后按 `O` 依次切换 `2 → 3 → 1`。三套均跟随后坐力，尺寸使用按分辨率缩放的像素参数：

| 预设 | 样式与颜色 | 长度 / 粗细 / 间距（文件值） | 不透明度 / 描边 |
| --- | --- | --- | --- |
| `wifi-crosshair1.cfg` | 经典动态十字（样式 `2`），橙色 RGB `255/150/0` | `110 / 32 / 128` | `255` / Full Outline |
| `wifi-crosshair2.cfg` | 静态圆环（样式 `3`），白色 RGB `255/255/255` | `120 / 32 / 9999` | `255` / Full Outline |
| `wifi-crosshair3.cfg` | 白色中心点（样式 `4`，线长为 `0`） | `0 / 4 / 0` | `255` / Full Outline |

准星一采用经典动态分裂：阈值 `10`，外段线长占比 `0.9`，内外段 alpha 倍率均为 `1`；描边为黑色、不透明。只有准星一保留专业简注和已确认的范围，准星二、三仅保留命令。准星二的样式 `3` 为静态圆环，线长和经典分裂参数不用于其圆环外观；间距输入值 `9999` 原样保存，实际读取值受引擎允许范围影响。准星二、三未显式覆盖描边 RGBA，沿用当前游戏值。

三份文件还分别写入以下相同偏好，每套新增 15 项：

| 设置组 | 当前值 |
| --- | --- |
| 镜内中心点 | 不使用准星颜色，缩放倍率 `1` |
| 观战准星 | 显示所有人的准星；可接管机器人时使用自己的准星 |
| 投掷辅助准星 | 闪光、HE、燃烧弹/燃烧瓶、烟雾、诱饵均关闭；保留普通准星开启 |
| 投掷辅助显示延迟 | 五类均为拔销后 `1` 秒 |

这些参数是全局客户端变量；每套 CFG 写入相同值，因此切换预设时保持一致。

`Mouse5` 模式 1 加载准星 1 和 HUD 配色 `11`，模式 2 加载准星 2 和 HUD 配色 `0`，并将下一次 `O` 切换设为后续预设。按 `P` 重新加载主配置并恢复模式 1。

更新时将三个 `wifi-crosshair*.cfg` 与主配置及 SOCD 配套文件一并复制到游戏的 `game/csgo/cfg`，执行 `exec autoexec` 重新加载绑定；也可用 `exec wifi-crosshair1`、`exec wifi-crosshair2`、`exec wifi-crosshair3` 单独加载。更改分辨率后，游戏会按比例缩放已设尺寸；重新加载预设会以当前分辨率重新应用文件中的像素值。

### Windows 脚本

| 脚本 | 用途 |
| --- | --- |
| `备份730逐个文件和CFG后启动Steam.bat` | 按 `730-Original` 清单备份主账号与 CFG，各保留最近 5 份时间戳快照，然后启动 Steam |
| `同步主账号730.bat` | 删除主账号现有 `730`，从 OneDrive 完整恢复，再覆盖共享 CFG |
| `同步所有账号730-Onedrive.bat` | 对本机全部真实 Steam 账号执行同一份 `730` 恢复 |

## 说明

仓库的 `.gitignore` 排除个人国服启动记录 `cnlauncher.txt`、物品偏好 `cs2_preferred_items.txt` 与 `workshop_saves/` 创意工坊存档。这些文件可留在私人备份中，不影响此处的按键、准星和 SOCD 配置；忽略规则不会删除现有备份或改变 Windows 脚本的恢复范围。

脚本固定使用 `C:\Steam` 与 `%OneDrive%\CS2`。

备份脚本可在 Steam 或 CS2 运行时执行，无需先退出；它逐个复制执行时磁盘上的实际文件，不会主动让游戏保存设置，尚未写入文件的设置不会包含在备份中。

两个同步脚本用于恢复配置，会永久删除目标账号原有的整个 `730`，再复制 OneDrive 恢复源，不会逐文件合并。脚本不要求 Steam 或 CS2 已退出，但建议恢复前退出游戏与 Steam，并确认 OneDrive 恢复源已同步完成，以免运行中的游戏或云同步随后重新写入并覆盖恢复的文件。

完整绑定以 `autoexec.cfg` 与 `wifi-*.cfg` 为准。`730-Original` 是备份文件的唯一清单；清单变化会自动反映到下一次备份，恢复脚本则始终复制完整恢复源。

## 版权说明

原创代码依据 [Apache License 2.0](./LICENSE) 发布。Valve 与 Counter-Strike 名称、格式、游戏内容及用户 Steam 数据不在许可范围内。

许可边界见[许可范围](./LICENSE_SCOPE.md)。
