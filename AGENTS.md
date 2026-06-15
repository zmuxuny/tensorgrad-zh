# AGENTS.md

本文件适用于整个仓库。所有后续改动都要优先遵守这里的项目规则；如果子目录以后出现更具体的 `AGENTS.md`，以更深层的文件为准。

## 项目目标

这个 fork 的主要目标是把 Tensorgrad / The Tensor Cookbook 完整翻译成高质量中文版，同时尽量保持原项目的代码、公式、图示、交叉引用和可编译性。

翻译工作的主要对象是：

- `paper/cookbook.tex`
- `paper/chapters/*.tex`
- 必要时同步更新 `README.md`、文档说明和图中文字资源

除非用户明确要求，不要改动 `tensorgrad/`、`tests/`、`examples/` 等 Python 代码的行为。

## 翻译原则

- 以忠实、清晰、自然的中文为第一目标。不要为了直译牺牲数学含义，也不要擅自扩写原文没有表达的结论。
- 保留 LaTeX 结构：不要随意删除或改名 `\label{}`、`\ref{}`、`\cite{}`、`\input{}`、数学环境、TikZ 宏、图表路径和自定义命令。
- 保留代码块、Python API 名称、变量名、文件路径、URL、BibTeX key、LaTeX 命令和数学符号的英文原样。
- 章节标题、正文、图注、表注、练习说明、定理/引理/证明等可翻译为中文；术语第一次出现时可采用“中文（English）”形式，后续使用统一中文译名。
- 不要翻译人名、论文标题、包名、函数名、类名和专有项目名，例如 `Tensorgrad`、`PyTorch`、`SymPy`、`einsum`。
- 对公式附近的解释要特别保守：先理解上下文，再翻译文字；不要改公式来“顺手修正”，除非用户明确要求校对数学内容。
- 图像中的英文文字暂时可以保留；若要翻译图中文字，应优先编辑源 SVG/TikZ，而不是只替换导出的 PDF/PNG。
- 保持中英文标点风格统一：中文正文使用中文标点；数学表达、代码、英文术语周围保持可读空格。
- 如果发现原文可能有笔误，用中文注释或 TODO 标记前先确认上下文；不要在翻译任务里悄悄改变原作者含义。

## 术语约定

优先使用下列译名；如果上下文已有更合适的惯用译法，保持全书一致并更新本表。

| English | 中文 |
| --- | --- |
| tensor | 张量 |
| tensor diagram | 张量图 |
| tensor network | 张量网络 |
| edge | 边 |
| named edge | 命名边 |
| variable | 变量 |
| scalar | 标量 |
| vector | 向量 |
| matrix | 矩阵 |
| derivative | 导数 |
| gradient | 梯度 |
| Hessian | Hessian 矩阵 |
| Jacobian | Jacobian 矩阵 |
| contraction | 缩并 |
| trace | 迹 |
| transpose / transposition | 转置 |
| symmetry | 对称性 |
| symmetrization | 对称化 |
| covariance | 协方差 |
| contravariance | 逆变性 |
| covariance and contravariance | 协变与逆变 |
| expectation | 期望 |
| moment | 矩 |
| cumulant | 累积量 |
| Gaussian | 高斯 |
| Kronecker product | Kronecker 积 |
| Hadamard product | Hadamard 积 |
| Khatri-Rao product | Khatri-Rao 积 |
| flattening | 展平 |
| vectorization / vec operator | 向量化 / vec 算子 |
| copy tensor | 复制张量 |
| chain rule | 链式法则 |
| broadcasting | 广播 |
| simplification | 化简 |
| isomorphism | 同构 |

## LaTeX 与构建注意事项

- 中文版最终应支持中文编译。大规模翻译前，优先把 `paper/cookbook.tex` 迁移到适合中文的编译方式，例如 `ctexbook` 或 XeLaTeX + `xeCJK`。
- 不要盲目保留只适合英文的字体设置。如果中文编译失败，重点检查 `\documentclass`、`\usepackage[T1]{fontenc}`、字体包和编译引擎。
- 翻译后尽量运行一次 PDF 构建。推荐目标命令为 `cd paper && latexmk -xelatex cookbook.tex`；如果仓库尚未完成 XeLaTeX 迁移，应先记录当前阻塞点。
- 对代码或库行为有改动时，运行 `uv run pytest`；纯翻译改动通常不需要跑 Python 测试，但需要尽量验证 LaTeX 可编译。
- 不要提交 LaTeX 临时文件、编译缓存或无关生成物。`paper/cookbook.pdf` 是否更新，按用户当次要求决定。

## 翻译执行计划

1. 建立中文构建基础
   - 确认 PDF 的推荐编译命令。
   - 将主文档改为支持中文的 LaTeX 配置。
   - 编译最小中文样例，确保正文、数学公式、图、引用都能正常生成。

2. 固化术语表和风格
   - 先通读目录、引言和核心符号说明。
   - 根据实际文本补充本文件的术语约定。
   - 遇到同一术语多种译法时，优先统一而不是局部发挥。

3. 翻译当前主文档会编译到的章节
   - `paper/cookbook.tex`
   - `paper/chapters/intro.tex`
   - `paper/chapters/statistics.tex`
   - `paper/chapters/appendix.tex`
   - 每完成一个文件，至少做一次结构检查；条件允许时编译 PDF。

4. 翻译暂未启用但属于书稿的章节
   - `simple_derivatives.tex`
   - `kronecker.tex`
   - `functions.tex`
   - `determinant.tex`
   - `advanced_derivatives.tex`
   - `special_matrices.tex`
   - `decompositions.tex`
   - `ml.tex`
   - `tensor_algos.tex`
   - `tensorgrad.tex`

5. 处理配套内容
   - 翻译或重写 `README.md` 中面向读者的介绍。
   - 检查图注、表格、练习、脚注和参考文献前后的中文语境。
   - 评估是否需要翻译 SVG/TikZ 图中文字。

6. 全书校对
   - 检查术语一致性、错别字、英文残留、交叉引用、目录和 PDF 排版。
   - 对公式说明做逐段复核，避免中文改变数学含义。
   - 最终生成可阅读的中文版 PDF，并记录构建方式。

## 协作规则

- 每次翻译尽量按章节或小节推进，避免一次性修改过多文件导致难以审阅。
- 开始翻译新章节前，先快速检查该章已有中文化程度和相关术语。
- 修改前后可用 `git diff --stat` 和 `git diff -- <file>` 检查范围，确保没有无关改动。
- 如果用户正在同时编辑同一文件，必须保留用户改动，不要回滚或覆盖。
- 对不确定的术语或数学含义，优先保留英文括注并在回复中说明，而不是强行定译。

## Git 管理与发布

- 翻译工作必须用 git 管理。开始工作前检查 `git status --short`，结束前检查 `git diff --stat` 和 `git status --short`。
- 每完成一个稳定阶段就提交一次，例如中文构建基础、一个完整章节、README 中文化、全书校对。提交粒度要便于回滚和审阅。
- 提交信息使用简洁中文或英文均可，但要能说明范围，例如 `翻译引言章`、`Enable Chinese LaTeX build`。
- 提交前确认没有 LaTeX 临时文件、缓存文件或无关生成物混入。
- 不要改写历史、不要强推、不要使用破坏性 git 命令。
- 最终全书翻译完成、构建验证通过、工作区干净后，直接推送到远程主分支：`git push origin main`。
- 推送前必须确认当前分支是 `main`，远程是预期的 fork；如果当前分支不是 `main`，先停止并说明。

## 工作进度账本

为了避免长目标在多轮执行中失去上下文，必须维护一个轻量的进度账本。

- 账本文件使用仓库根目录的 `translation-progress.md`。
- 账本只记录会影响下一轮继续工作的状态，不记录过程性碎碎念。
- 账本内容必须短小、稳定、可截断，禁止无限增长。
- 账本建议始终只保留这四块内容：
  - `Current`：当前正在处理的文件或小节，最多 1 项。
  - `Done`：已完成内容，最多保留 12 项；超出时按章节合并摘要。
  - `Next`：下一步计划，最多 3 项。
  - `Open`：未解决问题或阻塞，最多 5 项；一旦解决立刻删除。
- 每完成一个文件、一个小节，或一次构建验证后，更新账本一次。
- 每次开始新的 `/goal` 或重新接手工作时，先读账本，再决定从哪里继续。
- 如果账本变长，先合并旧条目，再追加新条目，不要让它持续膨胀。
