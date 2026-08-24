# 《Reco Love: Gold Beach》中文化补丁

> 本补丁仅用作技术交流，请支持正版。

## 基本说明

本项目是基于PS Vita 游戏《レコラヴ Gold Beach》的非官方中文化补丁，基于日版 1.07（本体 1.00 + 1.07 升级补丁）。

请自行获取游戏和解密档案。

汉化范围覆盖：

- 主线剧情对话（Script.cpk）
- 系统与UI文本（主菜单、系统设置、游玩履历等）
- Table文本（提示、名称、筛选、姿势列表等）
- UI贴图（标题、字幕、加载图等）
- 角色名字栏的汉字少部分完成简体中文转化，部分字基于freetype锁定基线重绘之后仍为乱码只能保留了。 
- 部分DLC的剧情完整脚本和道具名/活动标题/描述

发布物xdelta补丁仅包含汉化所需的极小部分文件、补丁清单与构建工具，不包含完整游戏文件，仅对你自己拥有的游戏生效。

## 兼容性

- PS Vita（需安装 rePatch 与 reAddcont 插件）
- 仅支持日版1.07；基准文件不一致时补丁会校验失败
- PS:Vita3K 模拟器未测试

## 使用方式

### 一、准备游戏

补丁以 xdelta 形式发布，需要你自己拥有日版 1.07 的合法安装获取的解密副本；
PSV需要安装Repatch插件；
在VitaShell中对 `ux0:app`、`ux0:patch`、`ux0:addcont` 打开 `OpenDecrypted` 后，导出 `ux0:app/PCSG00782/`、`ux0:patch/PCSG00782/`（1.07 升级补丁）以及已安装的 DLC `ux0:addcont/PCSG00782/`到电脑上。

请先确认本体与全部 DLC 均已安装、并已更新到 1.07，再提取文件。

### 二、应用主文件补丁

使用 DeltaPatcher 或 xdelta3 命令行，将 `patches/` 中的补丁逐一对准下表基准文件应用：

命令行示例：`xdelta3 -d -f -s <基准文件> <补丁文件> <输出文件>`

|      补丁        |           应用到                   |        得到       | 
|-----------------|------------------------------------|------------------|
| `eboot.xdelta`  | 1.07 升级的 `eboot.bin`             | 汉化 `eboot.bin`  |
| `Common.xdelta` | 本体的 `media/cpk/Common.cpk`       | 汉化 `Common.cpk` |
| `Script.xdelta` | 1.07 升级的 `media/cpk/Script.cpk`  | 汉化 `Script.cpk` |
| `Table.xdelta`  | 1.07 升级的 `media/cpk/Table.cpk`   | 汉化 `Table.cpk`  |
| `UI.xdelta`     | 1.07 升级的 `media/cpk/UI.cpk`      | 汉化 `UI.cpk`     |

注意：

- `Common.cpk` 取自本体（1.00），`eboot.bin`、`Script.cpk`、`Table.cpk`、`UI.cpk` 取自 1.07 升级补丁，两边都需要有。
- 将应用后的文件按相同目录结构放入 `ux0:rePatch/PCSG00782/`。

### 三、应用 DLC 补丁

每个 DLC 对应一个内容 ID 文件夹，补丁按 `内容ID_Script` / `内容ID_Table` 命名。

1. 找到该 DLC 合法获取安装后的完整解密文件夹；
2. 将整个文件夹放入 `ux0:reAddcont/PCSG00782/<内容ID>/`（PSV）；
3. 把对应补丁应用到文件夹内 `media/cpk/` 下的同名文件；
4. 其余文件保持原版原样，不要删除。

> 重要：DLC 补丁只替换 `Script.cpk` / `Table.cpk`。语音（`media/sound/voice/`）、模型、UI 等文件必须来自合法获取安装的完整解密DLC文件夹，缺失会导致加载或进剧情时崩溃。安装了哪些DLC就打哪些，没安装的不需要打。

### 四、校验

- `patch_manifest.txt` 记录了每个补丁的大小与 SHA256（前 16 位），可用于核对下载完整性。
- 应用补丁时若提示源文件校验失败（checksum mismatch），说明基准文件不是日版 1.07，请核对本体与升级补丁版本后重新获取解密文件。

## 已知限制

- 受 PSV 字库容量上限影响，个别生僻字以最接近的简体字或对应日文原字显示；
- Vita3K 对部分 3D 演出场景兼容性有限（可能不显示角色模型，文字正常）；
- 本补丁仅针对日版 1.07，不适用于其他版本或区域。

## 构建方式

本项目使用自有工具链（`tools/`）完成：CPK 解析/重建、CRILAYLA 解压/重压缩、ADV v3 脚本解析与构建、eboot 文本补丁、GXT 贴图润色，以及 xdelta 生成与还原校验。

重新生成补丁所需条件：

- 自己合法获取的正版游戏的解密文件；
- Python 3；
- 依赖安装：`python -m pip install -r requirements.txt`；
- xdelta3（发布包已附带生成脚本，指向本地 xdelta3 可执行文件即可）。

主要步骤：解包 CPK → 解析 ADV 脚本与表格 → 套用译文 → 重建 CPK → 生成 xdelta。`tools/build_xdelta_v100.py` 会批量生成全部补丁，并在每个补丁生成后自动执行一次还原校验；脚本内的 xdelta3 路径改为你自己的 xdelta3 可执行文件位置即可。

## 致谢

- 感谢 [gothgirllover67/recolovetr](https://github.com/gothgirllover67/recolovetr) 的英化工程提供参考与完善思路；
- 感谢所有参与测试、校对的朋友。

## 免责声明

- 本项目仅用作技术交流，仅包含汉化所必需的最小资源（译文文本、xdelta 差异补丁与工具脚本），不包含完整游戏 ROM、DLC、升级补丁或任何版权素材；
- 补丁只能应用于你自己合法拥有的正版游戏解密文件；
- 请支持正版，请勿用于商业用途；
