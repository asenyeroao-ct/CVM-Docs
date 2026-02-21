# Config 标签参数与流程说明

返回索引: [CVM.md](./CVM.md)

## 配置档操作

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Profile Selector | `N/A` | UI state | 来自 `configs/*.json` 的配置名 | 首个可用配置 | 选择 LOAD/SAVE 作用的目标配置。 | 列表会从 `configs` 目录刷新。 |
| SAVE | `N/A` | action | 按钮点击 | idle | 将当前运行参数保存到当前选中配置文件。 | 会覆盖该配置对应的 JSON 文件。 |
| LOAD | `N/A` | action | 按钮点击 | idle | 将选中配置加载到当前 UI/运行时。 | 加载后会立即应用到界面与运行状态。 |
| DEL | `N/A` | action | 按钮点击 + 确认弹窗 | idle | 删除当前选中的配置文件。 | 需要用户确认；当仅剩一个配置时会阻止删除。 |
| NEW | `N/A` | action | 按钮点击 + 输入新名称 | idle | 基于当前参数创建新的配置文件。 | 文件名来自用户输入，保存到 `configs/`。 |
| EXPORT | `N/A` | action | 按钮点击 | idle | 将当前选中配置的 JSON 内容复制到剪贴板。 | 复制完成后会弹出英文提示。 |
| IMPORT | `N/A` | action | 按钮点击 + 文件选择器 | idle | 选择 JSON 文件并导入为配置。 | 导入后会保存为配置并立即应用。 |
| Clipboard Import Prompt | `N/A` | automatic dialog（仅 Config 标签） | 检测剪贴板中的 CVM 配置 + Yes/No 确认 + Yes 时输入配置名 | idle | 当 Config 标签处于激活状态时，若剪贴板存在有效配置内容，会触发导入确认。 | 选择 No 后，除非剪贴板内容变化或当前配置值变化，否则不会再次询问。 |

## 存储路径与命名规范

| UI Name | Config Key | Type | Range/Options | Default | Description | Notes |
|---|---|---|---|---|---|---|
| Runtime Config File | `config.json` | file path | 仓库根目录 JSON 文件 | `config.json` | 主运行时配置文件。 | 由 `Config` 类负责读写。 |
| Profile Folder | `configs/*.json` | file path pattern | JSON 配置文件集合 | `configs/` | 用户配置文件目录。 | 非默认用户配置通常被 git 忽略。 |
| Default Profile Template | `configs/default.json` | file path | JSON 文件 | `configs/default.json` | 仓库内置默认配置模板。 | 建议保持稳定用于新用户初始化。 |
| Profile Naming Rule | `N/A` | convention | 文件系统安全名称 | 后缀 `_cvm.json` | 在 Config 标签中 SAVE/IMPORT 的配置会保存为 `*_cvm.json`。 | 下拉列表不显示末尾 `_cvm`；旧版 `.json` 配置仍可加载。 |


## 安全配置流程

1. 修改前先 LOAD 最接近的基线配置。
2. 每次只改一个标签页，然后用新名字 SAVE（不要立刻覆盖基线）。
3. 实测通过后再保存一次，确认该版本可回放。
4. 长期保留一个可回滚配置（例如 `stable_backup`）。
5. 在有回滚选项前，不要覆盖高频使用的主配置。

## 命名与版本建议

| 命名模式 | 示例 | 作用 |
|---|---|---|
| 日期 + 模式 | `2026-02-18_normal_balanced` | 便于按时间追踪 |
| 场景标签 | `scrim_trigger_safe` | 快速识别用途 |
| 迭代后缀 | `ncaf_v3` | 清晰记录调参迭代 |

## 回滚与差异管理

- `configs/default.json` 建议作为入门基线，尽量少改。
- 高风险调参前先复制配置（如 `*_test`），验证通过后再替换稳定版。
- UI 更新后若行为异常，先 LOAD 已知稳定配置，再按当前结构重新 SAVE。
- 团队协作时统一命名规范，可显著减少误覆盖和错载配置。
