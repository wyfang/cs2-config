# CS2 Config

一套按 Steam 目录组织的 CS2 个人配置，包含按键、可选 SOCD 移动、准星、画面设置与 Windows 备份同步脚本。

[English](./README.en.md)

## 功能

### Wi-Fi SOCD

首次使用仓库随附的按键配置时为普通 `W` / `A` / `S` / `D` 移动，之后沿用上一次保存的 SOCD 开关状态。按 `I` 或在控制台执行 `exec wifi-socd` 后，`wifi-socd.cfg` 会为 `A` / `D`（左右）和 `W` / `S`（前后）启用“最后输入优先”的移动逻辑，两组方向独立处理。SOCD 使用独立文件开关，重载主配置不会改变其开关选择。

启用后，例如按住 `A` 后再按 `D`，脚本将方向切换为向右；松开 `D` 时，如果 `A` 仍按住，则恢复向左；两键都松开后，停止该方向轴的输入。`W` / `S` 使用相同逻辑。

### 主要按键

| 按键 | 功能 |
| --- | --- |
| `W` / `A` / `S` / `D` | 首次使用为普通移动，此后沿用上次保存的 SOCD 开关状态 |
| `/` / `Mouse4` | 麦克风常开切换 / 按住说话 |
| `I` | 切换 SOCD 开关，替换原来的装备显示切换功能；保存后沿用所选状态 |
| `O` | 循环切换三套准星，均跟随后坐力 |
| `P` | 重新加载 `autoexec.cfg`，恢复启动时的视角、准星与 HUD 配色，保留当前 SOCD 状态 |
| `Mouse5` | 切换两套持枪视角，同时切换对应准星与 HUD 配色 |
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
4. 在新电脑上核对下节的画质文件路径与加载结果；`exec autoexec` 不会恢复 `cs2_video.txt` 中的画质设置。

主要位置：

- 游戏脚本：`steamapps/common/Counter-Strike Global Offensive/game/csgo/cfg`
- 用户配置：`userdata/<Steam userdata ID>/730`

### SOCD 配置

`autoexec.cfg` 加载 `wifi-socd-core.cfg` 准备运行所需的命令定义，但不覆盖 WASD 或 `I` 绑定。按 `I` 可交替开启和关闭 SOCD，替换原来的 `show_loadout_toggle`（装备显示切换）。也可在控制台直接开启：

```text
exec wifi-socd
```

启用后会覆盖 `W` / `A` / `S` / `D` 的绑定，并在保存配置后打印 `Wi-Fi SOCD ON!!` 图案。核心定义文件会将 `joy_side_sensitivity` 和 `joy_forward_sensitivity` 设为 `1`。配置用于个人测试与研究，实际效果以当前游戏版本和服务器行为为准。

关闭 SOCD 时执行：

```text
exec wifi-socd-off
```

关闭文件清零方向输入并恢复普通 WASD，保存配置后打印 `Wi-Fi SOCD OFF!!` 图案，不修改视角、准星或 HUD 配色。开启文件将 `I` 绑定为 `exec wifi-socd-off`，关闭文件将其绑定为 `exec wifi-socd`。两个开关文件都会执行 `host_writeconfig` 保存当前游戏配置（包含 WASD 和 `I` 绑定）；下次启动时，主配置重新准备 SOCD 命令定义，并沿用保存的绑定，从而恢复上一次选择。用户按键配置必须可写；重新覆盖 `userdata` 或云同步覆盖本地配置也可能改变保存的选择。

执行 `exec autoexec` 或按 `P` 会重载主配置，但不改变 SOCD 的开关选择。由于脚本的按键处理命令会重新初始化，启用、停用或重载前应松开所有移动键。

不需要在 Steam 启动选项中强制执行开关文件；若之前添加过 `+exec wifi-socd-off` 或 `+exec wifi-socd`，应移除以免覆盖保存的选择。如果只更新游戏 CFG、没有复制随附的账号按键配置，先执行一次 `exec wifi-socd` 或 `exec wifi-socd-off`，选择当前状态并建立 `I` 键绑定。更新游戏配置时应同时复制 `autoexec.cfg`、`wifi-socd-core.cfg`、`wifi-socd.cfg` 与 `wifi-socd-off.cfg`。

### 画质与新电脑迁移

画质文件位于 `userdata/<Steam userdata ID>/730/local/cfg/cs2_video.txt`，与游戏目录中的 `autoexec.cfg` 分开。当前预设包含 `1920×1080`、无边框窗口、关闭垂直同步、`4` 倍 MSAA，以及阴影、纹理、粒子等独立参数，是混合画质配置。

该文件同时记录 `VendorID`、`DeviceID`、`Version` 和 `Autoconfig`；更换显卡或游戏版本后，游戏可能重新检测硬件并改写设置。迁移时完全退出游戏与 Steam，确认复制到实际登录账号和实际 Steam 安装目录下的 `730/local/cfg`，再比较首次启动前后的 `cs2_video.txt`。脚本默认使用 `C:\Steam`，其他安装位置需要自行调整；只覆盖 `game/csgo/cfg` 无法恢复此画质文件。

不要把旧显卡标识作为跨电脑通用值强行保留。新电脑完成硬件检测后，在游戏中核对并应用所需画质，再备份该电脑生成的文件。设为只读只能限制写入，不能保证游戏接受旧电脑的参数，也会妨碍后续保存画质调整。

### O 键准星

启动时加载 `wifi-crosshair1.cfg`，之后按 `O` 依次切换 `2 → 3 → 1`。三套均跟随后坐力，并使用新版像素命令。当前预设以文件中的实际参数为准：

| 预设 | 样式与颜色 | 长度 / 粗细 / 间距（像素） | 不透明度 / 描边 |
| --- | --- | --- | --- |
| `wifi-crosshair1.cfg` | Dynamic Quad，黄色（RGB `255/225/0`） | `120 / 31 / 999` | `180` / 完整描边 |
| `wifi-crosshair2.cfg` | Dynamic Quad，白色，当前完全透明 | `120 / 31 / 128` | `0` / 无描边 |
| `wifi-crosshair3.cfg` | 白色中心点 | `0 / 4 / 0` | `255` / 完整描边 |

`Mouse5` 的模式 1 加载准星 1 与 HUD 配色 `11`，模式 2 加载准星 2 与 HUD 配色 `0`，并将下一次 `O` 切换设为后续预设。`P` 会重新加载主配置并恢复模式 1。

更新时，将三个 `wifi-crosshair*.cfg` 与上节列出的主配置及 SOCD 配套文件一并同步到游戏的 `game/csgo/cfg`，在控制台执行 `exec autoexec` 重新加载绑定。尺寸在加载时按当前分辨率的像素值设置；之后更改分辨率，游戏会自动按比例缩放，再次加载预设则重新应用文件中的像素值。原始文件的部分注释尚未与数值一致，以上表格按实际配置整理；准星 2 的不透明度 `0` 保留自最新备份。

### Windows 脚本

| 脚本 | 用途 |
| --- | --- |
| `备份730逐个文件和CFG后启动Steam.bat` | 按 `730-Original` 清单备份主账号与 CFG，各保留最近 5 份时间戳快照，然后启动 Steam |
| `同步主账号730.bat` | 删除主账号现有 `730`，从 OneDrive 完整恢复，再覆盖共享 CFG |
| `同步所有账号730-Onedrive.bat` | 对本机全部真实 Steam 账号执行同一份 `730` 恢复 |

## 说明

仓库的 `.gitignore` 排除个人国服启动记录 `cnlauncher.txt`、物品偏好 `cs2_preferred_items.txt` 与 `workshop_saves/` 创意工坊存档。这些文件可留在私人备份中，不影响此处的按键、准星和 SOCD 配置；忽略规则不会删除现有备份或改变 Windows 脚本的恢复范围。

同步脚本固定使用 `C:\Steam` 与 `%OneDrive%\CS2`。运行前必须完全退出 Steam，并确认 OneDrive 已同步完成；两个同步脚本会永久删除目标账号原有的整个 `730`，不会逐文件合并。

完整绑定以 `autoexec.cfg` 与 `wifi-*.cfg` 为准。`730-Original` 是备份文件的唯一清单；清单变化会自动反映到下一次备份，恢复脚本则始终复制完整恢复源。

## 版权说明

原创代码依据 [Apache License 2.0](./LICENSE) 发布。Valve 与 Counter-Strike 名称、格式、游戏内容及用户 Steam 数据不在许可范围内。

许可边界见[许可范围](./LICENSE_SCOPE.md)。
