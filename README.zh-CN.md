# Make Paper Cut Collage 手工剪纸拼贴

**简体中文** | [English](README.md)

将照片或简短文字描述转化为干净、有质感的手工剪纸拼贴。该技能也可以保留原始照片不变，将其与紧凑的剪纸诠释并置在同一张连续的暖白色手账纸张上。

## 核心特性

- 用连贯的手工剪纸平面重构食物、宠物、物品、植物、人物、旅行场景、建筑和情绪。
- 使用大块和中块纸张、简单颜色或熟悉的纸张纹样（条纹、波点、格子、花卉），柔软手撕边缘，微妙的重叠暗化。
- 保持构图宽敞、编辑式风格，而不是用剪贴簿装饰填满页面。
- 支持忠实保留照片：不重绘、不改色、不修图、不拉伸、不生成替换保留的照片像素。
- 生成一张连续的干净暖白色纸张表面，带有可见的不规则纤维、微妙的纸浆浮雕和微弱的不连续扫描痕迹。
- 使用确定性的本地合成来处理布局、纸张、照片装裱、阴影和排版，避免生成的纸张接缝或不匹配的背景。
- 除非明确要求，否则排除票据、收据、标签、邮票、印章、伪文字、可见工具和无关的复古杂物。

## 使用方法

通过 `$make-paper-cut-collage` 显式调用该技能，或描述明确要求剪纸、cut-paper 或 剪纸拼贴的请求。

### 转换照片

```text
使用 $make-paper-cut-collage 将这张猫的照片变成手工剪纸手账拼贴。
```

```text
使用 $make-paper-cut-collage 将这张建筑照片变成安静的剪纸海报。不要文字。
```

主体由纹样纸张重构；除非您要求保留，否则源照片不会出现在最终作品中。

### 保留原始照片

```text
使用 $make-paper-cut-collage 将这张照片变成剪纸拼贴手账页面，并保留原始照片。
```

```text
使用 $make-paper-cut-collage 保留这张照片，并在剪纸图案旁边放置标题 "TOKYO"。
```

此模式将照片与剪纸诠释放在独立的布局区域中。它只生成一个透明的剪纸图案，然后在本地合成最终图像，从而控制源像素与纸张纹理。

### 从描述生成

```text
使用 $make-paper-cut-collage 创建一张关于雨天孤独感的蓝灰色剪纸海报。
```

```text
使用 $make-paper-cut-collage 创建一张关于独自在咖啡馆的温暖剪纸海报。
```

仅描述的作品默认无文字。

## 默认行为

| 属性 | 默认值 |
| --- | --- |
| 最终格式 | 基于图像的作品为 `3:4` 竖版（`宽:高`） |
| 照片转换 | 一个可识别的主体，由约 8–15 块连贯的纸张构成，必要时最多两个安静的源衍生环境呼应 |
| 保留竖版照片 | 照片在左，纸面板在右 |
| 保留横版或方形照片 | 照片在上，纸面板在下 |
| 保留照片分割 | 约 50% 照片和 50% 纸张 |
| 照片完整性 | 保留像素不变；不重绘、不改色、不修图、不拉伸、不加纹理 |
| 照片裁剪 | 最多裁剪源面积的 20%，仅在必要时；主体完整性受益时不裁剪 |
| 照片装裱 | 无边框打印，自然手撕暴露边缘，克制的环境光和接触阴影 |
| 保留模式图案 | 紧凑的图案加标题组，位于平衡角落，约占纸面板的 40% |
| 图案阴影 | 微妙的窄接触阴影（0.003），表现剪纸拼贴深度但不显立体 |
| 纸张 | 干净暖白色，可见纤维，低对比度，不重复，无污渍或严重老化 |
| 基于图像的文字 | 一到三个词的事实性英文标题，除非要求精确文字或无文字 |
| 仅描述文字 | 无 |
| 生成次数 | 默认一次图案生成调用；识别失败或纸张材质失败时最多一次针对性修订 |

明确的用户指令会覆盖这些默认值。

## 视觉系统

首选风格是**柔和的结构性场景提炼**：

- 保留一到三个识别锚点。
- 将透视或体积转化为相邻的浅、中、深色纸张平面，而不是绘制轮廓。
- 保持拼贴平坦、正面朝向，具有浅重叠深度而不是雕塑般的纸张体积。
- 使用源衍生的低饱和度调色板，约四到六种颜色，最多一个克制的暖色强调。
- 偏爱纯色纸张和微妙的纹样，如条纹、格子、网格、花卉或波点——纹样与源图像的风格和元素相协调。
- 将明显的褶皱保持在图案区域的 15% 以下。
- 保留充足的负空间，并与每个纸张边缘保持清晰的边距。

捆绑的参考图像仅指导材质、抽象度和构图。绝不能复制它们描绘的主体。

## 保留照片合成的工作原理

1. `compose_direct_split.py --plan` 解析最终比例、面板方向、裁剪限制和像素几何（自动修正 EXIF 方向）。
2. 图像生成在透明 RGBA 上创建一个孤立的、无文字的剪纸图案，使用源照片作为主体参考以获得精确的轮廓和比例。
3. 合成器为整个画布合成一张连续的手账纸张表面。
4. 原始照片像素在不进行像素级过滤的情况下装裱。
5. 手撕边缘遮罩、克制的阴影、图案放置和精确排版以确定性方式添加。
6. 检查结果的宽高比、裁剪比例、源像素完整性、透明度、纸张连续性和安全边距。

这种分离工作流程防止生成的白色矩形、棋盘格、污渍和色偏泄漏到纸面板中。

## 手动使用合成器

合成器主要由技能使用，但也可以直接使用 Python 和 Pillow 运行。

预览解析的布局：

```bash
python scripts/compose_direct_split.py \
  --photo path/to/photo.png \
  --plan
```

从透明图案组装保留照片合成：

```bash
python scripts/compose_direct_split.py \
  --photo path/to/photo.png \
  --motif path/to/motif-rgba.png \
  --output path/to/final.png \
  --caption "QUIET MORNING"
```

运行 `python scripts/compose_direct_split.py --help` 查看布局、宽高比、裁剪、图案、纸张、阴影和排版选项。

## 安装

### 1. 克隆仓库

```bash
git clone https://github.com/casperrr0706-maker/make-paper-cut-collage.git
```

或者从 GitHub 下载 ZIP 并解压。

### 2. 放入技能目录

将整个 `make-paper-cut-collage` 文件夹复制到您的 agent 技能目录中：

| 平台 | 路径 |
| --- | --- |
| **豆包 (agent-mode)** | `~/.doubao/agent_mode/workspace/.user_skills/make-paper-cut-collage/` |
| **Codex** | `~/.codex/skills/make-paper-cut-collage/` |
| **Claude Desktop** | `~/.claude/skills/make-paper-cut-collage/` |
| **其他 agent** | 请参考对应 agent 的技能安装文档 |

Windows 系统上，豆包路径通常为：

```text
%USERPROFILE%\.doubao\agent_mode\workspace\.user_skills\make-paper-cut-collage\
```

### 3. 重启 / 重新加载

如果技能没有立即出现，请重启或重新加载 agent，以便技能目录发现它。

### 依赖

- **图像生成**：默认使用 agent 内置的图像生成工具，无需额外配置。
- **手动合成器**（可选）：如果需要直接运行 `scripts/compose_direct_split.py`，需要 Python 3 和 Pillow：

```bash
pip install Pillow
```

## 使用方法

### 调用技能

有两种方式触发技能：

**显式调用：**
```text
使用 $make-paper-cut-collage 处理这张照片。
```

**自然语言：**
```text
把这张照片改成剪纸风格
Turn this photo into a paper-cut collage
做一个剪纸拼贴效果
```

当请求中提到剪纸、paper-cut、cut-paper、paper collage 等关键词时，技能会自动激活。

### 转换照片（仅剪纸）

主体由手工纹样纸张重构，源照片不出现在最终作品中。

```text
使用 $make-paper-cut-collage 将这张猫的照片变成手工剪纸拼贴。
```

```text
把这张建筑照片改成剪纸风格，不要文字。
```

### 保留原始照片（原图+剪纸并置）

在一侧保留原始照片不变，另一侧放置紧凑的剪纸诠释。这是照片输入的默认模式。

```text
使用 $make-paper-cut-collage 保留这张照片，并在旁边添加剪纸版本。
```

```text
用剪纸风格处理这张照片，保留原图。
```

### 从描述生成

```text
使用 $make-paper-cut-collage 创建一张关于雨天孤独感的蓝灰色剪纸海报。
```

### 调整输出

您可以在请求中指定以下选项：

| 选项 | 说明 | 示例 |
| --- | --- | --- |
| **比例** | 最终画布宽高比 | "用3:4竖版" / "1:1方形" |
| **文字** | 添加或移除标题文字 | "不要文字" / "加标题 SEASIDE" |
| **图案大小** | 调整剪纸图案的大小 | "图案大一点" |
| **图案位置** | 移动图案到不同角落 | "图案放左下角" |
| **阴影** | 调整接触阴影强度 | "阴影少一点" / "不要阴影" |
| **照片裁剪** | 控制源照片的裁剪方式 | "不要裁剪照片" |

### 完整请求示例

```text
使用 $make-paper-cut-collage 处理这张旅行照片。保留原图在左侧，剪纸版本在右侧。3:4比例。标题："SEASIDE"。
```

```text
把这张照片改成剪纸拼贴风格，保留原图，竖版4:3，不要文字，图案小一点。
```

## 项目结构

```text
make-paper-cut-collage/
|-- SKILL.md
|-- README.md
|-- README.zh-CN.md
|-- LICENSE
|-- agents/
|   `-- openai.yaml
|-- assets/
|   |-- style-references/
|   `-- examples/
|-- references/
|   |-- prompt-recipes.md
|   `-- style-system.md
`-- scripts/
    `-- compose_direct_split.py
```

## 自定义

- 编辑 [`references/style-system.md`](references/style-system.md) 调整视觉语言、比例、调色板、纸张、排除项和保留照片规则。
- 编辑 [`references/prompt-recipes.md`](references/prompt-recipes.md) 优化生成模板和示例。
- 更新 [`scripts/compose_direct_split.py`](scripts/compose_direct_split.py) 处理确定性几何、照片装裱、纸张合成、阴影和排版。
- 在 [`assets/style-references/`](assets/style-references/) 下添加精选图像，并在样式系统中记录它们的确切作用。将参考视为风格证据，绝不是可重用的主体模板。
- 保持 [`SKILL.md`](SKILL.md) 专注于路由、基本工作流和不可协商的不变量。

## 已知边界

- 在仅剪纸转换中不保留面部身份。
- 复杂场景被有意简化为几个锚点和支撑呼应。
- 在保留照片模式中以确定性方式添加精确文字；避免生成的自由字体。
- 仅当请求明确要求保留或显示原始照片时，才应用源照片保留。
