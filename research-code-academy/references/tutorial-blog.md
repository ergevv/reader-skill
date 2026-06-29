# Tutorial Blog Mode

Use this reference when the user asks to read source code and generate a Markdown blog, tutorial, learning note, course article, or concept-first explanation. This mode is different from normal code analysis: read the code to discover all concepts, but write the final article as a natural learning path rather than a line-by-line code walkthrough.

## Trigger Examples

- "阅读 `src/vio.cpp`，在 `fast_livo2_learning` 生成一个基于 Markdown 的 blog。"
- "写一篇别人不了解 FAST-LIVO2 也能学会 VIO 的教程。"
- "不要讲哪个代码做了什么，脱离代码讲清楚原理。"
- "数学推导详细，每个变量第一次出现都解释。"
- "内容层层递进，不要写成知识点清单。"

## Workflow

1. Clarify only blocking ambiguities before writing:
   - output directory or filename is missing and cannot be inferred;
   - expected depth, audience, or topic boundary conflicts;
   - the target source file does not exist.
2. Read the requested source file and nearby dependencies enough to extract the complete concept graph.
3. Build a private knowledge inventory:
   - algorithms;
   - states and variables;
   - coordinate frames;
   - sensor models;
   - residuals and optimization targets;
   - initialization and update logic;
   - assumptions, failure modes, and engineering motivations.
4. Convert the inventory into a learning route:
   - start from the problem the module solves;
   - introduce prerequisite concepts only when needed;
   - explain each new variable at first appearance;
   - derive formulas before using them;
   - connect each section to the next one with cause-and-effect.
5. Write the Markdown file in the requested directory. If the user gives only a directory, infer a clear filename from the source file and topic, such as `vio_blog.md`.
6. Verify the article does not fall back into code narration.

## Blog Requirements

Follow these rules unless the user explicitly overrides them:

- Write in Chinese when the user writes Chinese.
- Treat the article as a tutorial for readers who may not know FAST-LIVO2, VIO, SLAM, or the source project.
- Explain purpose first: what problem is being solved, why it matters, and what failure it prevents.
- Do not structure the article by code order, functions, classes, or line numbers.
- Do not say "this function does..." as the main explanation style. Mention code only when necessary as a short evidence note or appendix.
- Cover all knowledge points discovered from the target file; do not omit important concepts just because they are hard.
- Make concepts clear before formulas.
- Make mathematical derivations detailed enough that the reader understands where each equation comes from.
- Explain every variable the first time it appears: physical meaning, dimension, unit if relevant, coordinate frame, and why it is needed.
- Use examples when a concept is abstract or easy to misunderstand.
- Organize content as a natural progressive learning path, not a flat checklist.
- End with a concise recap and a recommended next learning step.

## Formula Rules

- Use `$...$` for inline formulas and `$$...$$` for display formulas.
- Keep operators such as `+`, `-`, `\times`, `/`, and `=` on the same line as their operands; do not place them alone on a separate line.
- Define symbols before or immediately after the formula.
- Prefer derivation blocks with short explanatory paragraphs between formulas.
- For coordinate transforms, state the transform direction explicitly.

Good display formula shape:

```markdown
$$
\mathbf{p}_w = \mathbf{R}_{wb}\mathbf{p}_b + \mathbf{t}_{wb}
$$

其中，$\mathbf{p}_b$ 表示点在机体系下的坐标，$\mathbf{R}_{wb}$ 表示从机体系到世界系的旋转，$\mathbf{t}_{wb}$ 表示机体系原点在世界系下的位置。
```

Avoid:

```markdown
$$
\mathbf{p}_w
=
\mathbf{R}_{wb}
\mathbf{p}_b
+
\mathbf{t}_{wb}
$$
```

## Recommended Article Structure

Adapt the headings to the target topic, but keep the learning progression:

```markdown
# 从问题出发理解 VIO

## 1. 为什么需要 VIO

## 2. 单靠相机或 IMU 会遇到什么问题

## 3. VIO 要估计哪些状态

## 4. 坐标系和变量约定

## 5. IMU 如何提供短时间运动预测

## 6. 图像观测如何提供几何约束

## 7. 误差、残差和优化目标从哪里来

## 8. 视觉和惯性信息如何融合

## 9. 初始化、退化和常见失败原因

## 10. 学完这部分后应该继续读什么
```

## Quality Checklist

Before finishing, check:

- The blog can be read without opening the source file.
- The article teaches the domain concept, not the implementation order.
- Every important variable is introduced before use.
- Every formula uses `$` or `$$`.
- No formula operator is stranded on a separate line.
- The article explains purpose, problem, and examples where useful.
- The content forms a coherent route from motivation to complete understanding.
- The output file path is reported to the user.
