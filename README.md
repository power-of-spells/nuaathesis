# nuaathesis

南京航空航天大学（非官方）学位论文 LaTeX 模板。

本仓库保留了原始模板结构，同时在 `论文/` 目录下整理出一套更适合当前中文学位论文写作的定制版本。对大多数使用者来说，**现在推荐直接从 `论文/` 目录开始**，而不是继续使用旧的 `demo_chs/` 示例。

[![构建状态](https://github.com/bismarckkk/nuaathesis/actions/workflows/ci.yml/badge.svg)](https://github.com/bismarckkk/nuaathesis/actions)

下载: [“稳定版”][link_rel], [最新开发分支构建][link_dev]

[link_rel]: https://github.com/nuaatug/nuaathesis/releases
[link_dev]: https://github.com/bismarckkk/nuaathesis/actions

## 当前推荐入口

- 主文件：`论文/master.tex`
- 文档类：`论文/nuaathesis.cls`
- 全局设置：`论文/global.tex`
- 图表示例：`论文/content/demo.tex`
- 参考文献数据库：`论文/bib/sample.bib`

## 相对原始类文件的主要改进

下面的说明，是以仓库原始 `nuaathesis.cls` 为基线，对当前 `论文/nuaathesis.cls` 做的整理。

### 1. 中文使用场景被单独收口

- 去掉了原模板中英文、日文的多分支类文件逻辑，当前版本聚焦中文论文写作。
- 旧文档里若仍写有 `lang=cn`，现在仍可兼容，但该选项不再参与类文件分支判断。
- 主体类文件统一走 `ctexbook`，减少多语言条件判断带来的维护成本。

### 2. 标题与英文字体管理更清晰

- 将原来容易被复用过度的标题字体逻辑拆成两套：
  - `\nuaa@font@headingchapter`：仅供 `chapter`、`section`、`subsection`、`subsubsection` 使用
  - `\nuaa@font@headother`：供定理、证明、目录编号等非章节标题位置使用
- 在 XeLaTeX 下显式设置了 `Times New Roman`，并定义了 `\nuaa@tnr`。
- 现在只有章节标题体系中的英文允许跟随黑体标题样式，其余位置的英文都回到 Roman/Times New Roman 路线。

### 3. 中英文封面、摘要页被独立出来

- 研究生中文封面和英文封面使用独立格式页，不再混用正文标题机制。
- 中文摘要、英文摘要不再依赖 `\chapter*` 直接套正文标题样式，而是通过专门的前置页辅助宏单独控制。
- 这样做以后，学校若继续调整封面或摘要格式，修改范围会更集中，不容易牵动正文排版。

### 4. 封面字体控制更接近最新模板

- 增加了封面专用字号/字族宏，分别控制五号、四号、三号、小初、一号等位置。
- 英文封面标题和英文摘要显式使用 `\nuaa@tnr`，避免只靠 `\rmfamily` 时字体落回不确定的 Roman 字体。
- 研究生中文封面的校名“南京航空航天大学”改为直接输出文字，而不是再嵌入 `pdf` 图片。
- 校名字体支持优先从项目内读取 `论文/base/迷你简启体.ttf`，若本地文件不存在，再回退到系统字体名；仍失败时最后退回 `\kaishu`。

### 5. 参考文献方案升级为 BibLaTeX

- 原来的 `masterbib.bst + natbib + BibTeX` 已替换为 `biblatex + biber + biblatex-gb7714-2015`。
- `论文/master.tex` 已改为使用 `\addbibresource` 和 `\printbibliography`。
- 保留了 `\inlinecite` / `\onlinecite` 这类旧接口，方便老文稿继续迁移。

### 6. 图表方案换成更现代的包

- 移除了旧的 `tabu`、`floatrow`、`subfig` 方案。
- 引入了 `tabularx`、`longtable`、`multirow`、`makecell`、`subcaption`、`tikz`、`pgfplots`、`tabularray` 等更现代的方案。
- 增加了统一的表格字体入口，并补充了 `Y`、`Z`、`T` 等列类型。
- `论文/content/demo.tex` 已按这套新规范更新，适合直接照抄示例。

### 7. 一些已修复的兼容性问题

- 修复了 `\subfigureautorefname already defined` 这类重定义报错，改用 `\providecommand + \renewcommand`。
- 修复了校名字体回退时使用未定义 `\kaiti` 命令的问题，统一改为 `\kaishu`。

## 快速上手

1. 直接打开 `论文/master.tex`。
2. 在 `论文/global.tex` 中填写论文信息、摘要、关键词等内容。
3. 正文各章节放在 `论文/content/` 下，图表写法可优先参考 `论文/content/demo.tex`。
4. 参考文献数据默认放在 `论文/bib/sample.bib`，主文件已经接好 `\addbibresource`。
5. 若希望封面校名严格使用“迷你简启体”，请将字体文件放在 `论文/base/迷你简启体.ttf`。

## 编译方式

推荐使用 XeLaTeX + Biber。

常见编译链：

1. `xelatex master.tex`
2. `biber master`
3. `xelatex master.tex`
4. `xelatex master.tex`

如果你使用 `latexmk`，请确认它实际调用的是 `biber` 而不是 `bibtex`。

## 目录说明

- `论文/`：当前推荐使用的中文定制版论文工程
- `demo_chs/`、`demo_en/`、`demo_ja/`：原模板自带示例
- `nuaathesis.dtx`：原始文档化源码
- `nuaathesis.pdf`：模板说明文档

## 额外说明

- `论文/base/迷你简启体.ttf` 适合本地精确复现封面效果；如果仓库要公开分发，请自行确认字体文件的授权情况。
- 如果只是想写中文硕博论文，优先使用 `论文/` 目录即可；旧示例更多用于对照原模板结构。
