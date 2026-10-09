# Photo Poster Redesign

> 把真实摄影照片再设计为现代艺术展览级海报的 **AI Agent Skill**。
> 上半部分完整保留原片（仅轻微调色），下半部分在暖象牙白艺术纸上转译为现代主义编辑海报。

## 特性

- **上下 50% 分区**：上半部分逐像素保真，主体身份/数量/动作/比例/空间关系/材质/光影不变；下半部分做视觉抽象。
- **四种抽象手法**：高反差版画、丝网印刷、水墨线描、半透明影像拼贴（可组合）。
- **专属视觉符号**：由照片的视觉锚点推导（风、花、人物、运动、空间），几何元素均源自原图，不做随机装饰。
- **克制的色彩**：暖白 + 墨黑为基础，仅一种强调色（朱砂红 / 青蓝 / 橙黄 / 深绿），低饱和。
- **手工印刷质感**：纸张纤维、丝网颗粒、局部缺墨、轻微套色偏移、自然手工痕迹。
- **标准 SKILL.md**：模型无关，任何支持“参考图生图/图片编辑”并能从目录加载技能的 AI Agent 均可安装调用。

## 效果预览（输出格式）

以下内容与 `SKILL.md` 中的“输出格式”模块完全一致：

`````markdown
````markdown
**生成图**

![photo-poster-redesign](图片路径或直接渲染的图片)

**最终 Prompt**

```text
[本次实际使用的完整提示词]
```

**说明**

- 强调色 / 标题 / 抽象手法：[本次取值]
- [一句话说明视觉锚点与专属符号的选取理由]
````
`````

## 目录结构

```
photo-poster-redesign/
├── SKILL.md                    # 技能主文件：frontmatter + 工作流程 + 质量门
├── README.md                   # 本说明文档
└── references/
    └── master-prompt.md        # 可复用主提示词模板（含变量插槽）
```

## 安装

### 前置要求

- AI Agent 支持从**技能目录**加载 Skill（读取 `SKILL.md` 的 YAML frontmatter）。
- Agent 提供**参考图生图 / 图片编辑**能力（可传入一张参考图并按提示词生成）。
- 无需 API Key、无需额外依赖；技能本身不包含任何脚本或私有凭证。

### 方式一：克隆到技能目录（推荐）

将仓库直接克隆为 Agent 技能目录下的 `photo-poster-redesign` 文件夹：

```bash
git clone https://github.com/yuzhiboyao015-lgtm/photo-try.git <skills-root>/photo-poster-redesign
```

以 Windows / 豆包环境为例，`<skills-root>` 可取以下任一目录：

```powershell
# 用户自定义技能目录
git clone https://github.com/yuzhiboyao015-lgtm/photo-try.git "$env:USERPROFILE\Doubao\skills\photo-poster-redesign"

# 或工作区 .user_skills 目录
git clone https://github.com/yuzhiboyao015-lgtm/photo-try.git "$env:LOCALAPPDATA\Doubao\User Data\Default\.doubao\agent_mode\workspace\.user_skills\photo-poster-redesign"
```

### 方式二：手动复制

1. 下载本仓库全部文件；
2. 放入一个名为 `photo-poster-redesign` 的文件夹；
3. 将该文件夹移动到 Agent 的技能目录（如 `~/Doubao/skills/`、`workspace/.user_skills/`、`.agents/skills/` 等）。

> 安装后请确认 `SKILL.md` 位于该文件夹的**根目录**，且 `references/master-prompt.md` 路径不变。

### 验证安装

重新加载 Agent 后，技能目录中应出现 `photo-poster-redesign`；上传一张照片并说“用这个技能做海报”，Agent 能识别即表示安装成功。

## 调用

### 自动触发

Agent 依据 `SKILL.md` frontmatter 中的 `description` 自动匹配。当用户上传真实照片（人物 / 动物 / 花卉 / 建筑 / 运动 / 风景等）并要求做艺术展海报、独立杂志封面、现代东方艺术展海报、实验印刷作品或摄影再设计时，技能自动启用。

### 显式调用

- `$photo-poster-redesign`
- “用 photo-poster-redesign 把这张照片做成海报”
- “用这个 skill，把这张照片做成现代艺术展海报”

### 必需输入

- **一张真实摄影图片**。未提供图片时，Agent 会先请用户上传，不会凭空生成。

### 可选参数

| 参数 | 取值 | 默认策略 |
|---|---|---|
| 强调色 | 朱砂红 / 青蓝 / 橙黄 / 深绿 | 按主题情绪选一种，全图仅一种 |
| 英文标题 | 1–3 个单词 | 据主题拟定，拼写正确 |
| 辅助文字 | 年份、媒介、地点、作者等 | 可省略，宁少勿多 |
| 抽象手法 | 高反差版画 / 丝网印刷 / 水墨线描 / 半透明影像拼贴 | 选 1–2 种最贴合的组合 |

用户未指定时，由 Agent 按照片主题自行选定（低影响选择）。

## 视觉锚点 → 专属符号

| 锚点 | 专属符号 |
|---|---|
| 风 | 流动线条、飘带 |
| 花 | 植物轮廓、花瓣扩散 |
| 人物 | 剪影、姿态线 |
| 运动 | 弧形轨迹、速度纹理 |
| 空间 | 几何色块、留白 |

## 工作流程

1. 读取照片，提取视觉锚点（姿态、服装、花卉、建筑、轨迹、光影、空间）。
2. 确定设计参数（强调色、标题、辅助文字、抽象手法）。
3. 依据 `references/master-prompt.md` 编译提示词，替换变量。
4. 以照片为参考图，生成 **1536×2048（3:4）** 海报。
5. 对照质量门检查，不合格则收紧提示词重生成（最多 2 次）。
6. 交付海报，并附最终提示词与参数说明。

## 质量门 / 硬性约束

- 上下严格 50% / 50%，3:4 竖版。
- 上半部分保真，仅轻微调色，无新增元素。
- 下半部分抽象但保留核心轮廓、仍可识别；几何元素源自原图。
- 仅暖白 + 墨黑 + 1 种强调色，低饱和、克制。
- 小字号、宽字距、非对称排版，文字不遮挡主体、无乱码。
- 禁止：商业广告风、巨大标题、复古宣传画、随机几何拼贴、过度鲜艳颜色、人物变形、改变动作、增加人物、AI 塑料质感、3D 效果、水印、Logo、乱码文字。

## 兼容性说明

- **模型无关**：技能仅使用标准 `SKILL.md`（YAML frontmatter + Markdown），不绑定特定厂商或模型。
- **工具解耦**：提示词以“参考图生图/图片编辑工具”泛指宿主能力，不写死任何工具名或 API。
- **可移植**：任意支持目录式技能加载与参考图生图的 Agent（豆包、类 Claude Code / Codex 的 Agent 等）均可按上文安装。

## 示例请求

- “用 photo-poster-redesign 把这张照片做成艺术展海报”
- “用这个 skill，强调色用青蓝，标题用 QUIET LIGHT，做一张杂志封面”
- “把这张运动照片做成实验印刷风海报”
