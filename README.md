# dsh-arc-hub · Linux 移植版

把「弧梦数字中枢 / 海拉（HELA）」接入 DeepSeek Harness 的 DSH 插件 —— **Linux/macOS 可用版**。

> **这是派生作品。** 原项目：[499596849-jpg/dsh-arc-hub](https://github.com/499596849-jpg/dsh-arc-hub)
> 原作者：**499596849-jpg** · 许可：MIT（见下方「署名与许可」）
> 本仓库只做平台移植与少量适配，不改变原项目的功能设计与人设安全边界。

## 与原版的区别

| 项 | 原版 | 本移植版 |
|---|---|---|
| 人设卡解密 | 插件内 DPAPI（Windows 专用） | 统一向中枢服务 `/api/persona` 取；Windows 保留原 DPAPI 路径 |
| 中枢地址 | `helper:8795` 独立助手 | 统一 `127.0.0.1:8799`（中枢服务本身） |
| 打开目录 | `helper/打开中枢助手.cmd` | 中枢服务 `xdg-open`（Linux）/ `explorer.exe`（Windows） |
| 中枢根目录 | `%LOCALAPPDATA%\ArcDreamHub` | `ARC_HUB_ROOT` → `ARC_HUB_DATA` → `data-dir.txt` → `~/ArcDreamHub` |
| 依赖 | Node 22+ | 同左；新增 `engines.dsh >=0.2.2-rc.1` |

工具名与语义保持原样：`arc_hub_status` / `arc_hub_persona` / `arc_hub_mode` / `arc_hub_open_dir`。

## 安装

```sh
dsh plugin --profile web add <本仓库地址或本地路径>
```

装完重载 profile（或重启 DSH），设置页会出现「弧梦中枢」，右下角出现数字中枢圆。

中枢服务需先跑起来（见配套的 [arcdream-hub Linux 移植](https://github.com/<你的账号>/arcdream-hub-linux)）：

```sh
ARC_HUB_DATA=~/ArcDreamHub bin/arc-hub.sh start   # 中枢服务 127.0.0.1:8799
```

## 配置

| 变量 / 配置 | 作用 |
|---|---|
| `ARC_HUB_ROOT` | 中枢数据目录（优先级最高） |
| `ARC_HUB_DATA` | 同上（兼容中枢服务的变量名） |
| `ARC_HUB_API` | 中枢地址，默认 `http://127.0.0.1:8799` |
| `config.hubRoot` | 插件配置里的显式覆盖 |

## 安全边界（沿用原设计）

- 完整人设卡加密存储，工具**只返回摘要**，任何情况下不输出原文或密钥。
- 只访问 `127.0.0.1`，不上传数据。

## 署名与许可

- 原项目：`dsh-arc-hub` © 499596849-jpg，MIT License（原文见 [LICENSE](./LICENSE)）。
- 本移植：仅修改平台适配相关代码，同样以 MIT 发布。
- 人设卡与截图等素材版权归原作者；本仓库不含任何真实数据、密钥或人设。
