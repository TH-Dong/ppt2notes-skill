---
name: ppt2notes
description: Convert a single PPT/PPTX/PDF lecture slide deck into faithful, elegant Chinese LaTeX course notes with meaningful cropped figures and a compiled PDF. Use for PPT-to-notes, review notes, open-book notes, or courseware整理. Preserve slide-derived content as black body text, and put all AI-added explanations, examples, intuition, and limited extensions in light-blue explanation boxes.
---

# PPT2Notes

Use this skill to convert **one lecture PPT/PPTX/PDF slide deck** into a high-quality Chinese LaTeX course note and a compiled PDF.

The goal is not to generate exam predictions, question banks, flashcards, or a full textbook chapter. The goal is to produce a clean, structured, visually useful course note that is faithful to the original slide deck and suitable for review or open-book use.

## Core Goal

Produce a Chinese LaTeX note from one PPT/PPTX/PDF slide deck.

The final note must:

- be based primarily on the original slide deck;
- begin with a concise but meaningful knowledge structure for the deck;
- explain the slide content knowledge point by knowledge point;
- include meaningful cropped figures from the slides whenever visuals materially help understanding;
- use black body text for slide-derived content, including original wording, light paraphrase, faithful condensation, and faithful structural organization;
- place all AI-added explanations, examples, intuition, and limited extensions inside light-blue `aiexplainbox` environments;
- use `examplebox` for worked examples, derivations, proofs, or problem walk-throughs that require visible reasoning;
- avoid unnecessary exam-oriented modules such as question prediction, standalone name-definition tables, error lists, or standardized answer templates unless the user explicitly asks for them;
- deliver a compilable `.tex` file, a compiled `.pdf`, and all referenced figure assets.

## Input Scope

Supported inputs:

- `.pptx`
- `.ppt`
- `.pdf`

Do not include the following by default:

- past exam papers;
- textbook chapters;
- online materials;
- external references;
- web search results;
- unrelated course documents.

If the user provides only the slide deck, use only that slide deck as the primary source.

If the user explicitly provides additional materials, use them only when the user asks to integrate them. Otherwise, ignore them for this skill.

This skill is designed for **one slide deck or one chapter at a time**, not a full multi-chapter book.

## Output Scope

Default deliverables:

- `notes.tex`
- `notes.pdf`
- `figures/` directory containing all cropped slide figures referenced by the `.tex`
- optional project archive, for example `notes_project.zip`, containing `notes.tex`, `notes.pdf`, and `figures/`

Use a reasonable Chinese filename when the slide title is clear, for example:

- `第五章_遥感图像几何处理_notes.tex`
- `第五章_遥感图像几何处理_notes.pdf`
- `第五章_遥感图像几何处理_project.zip`

The default output format is LaTeX plus compiled PDF. Do not output Markdown, HTML, Word, flashcards, quizzes, or question banks unless the user explicitly requests them.

## Required Template

Start from:

```text
assets/notes-template.tex
```

The template file is expected at `assets/notes-template.tex` relative to this skill directory.

Copy the template and fill the metadata macros:

```latex
\newcommand{\CourseName}{课程名称}
\newcommand{\SlideDeckTitle}{PPT 标题}
\newcommand{\NoteTitle}{课程笔记标题}
\newcommand{\NoteSubtitle}{基于 PPT/PDF 的图文整理笔记}
\newcommand{\NoteAuthor}{InkTH}
\newcommand{\NoteDate}{\today}
```

Then replace the placeholder body with the generated note content.

Use `ctexart`. The note should be suitable for Chinese course material and should compile with XeLaTeX.

If `assets/notes-template.tex` is unavailable, use the fallback strategy in this skill instead of stopping immediately.

## Source Boundary: Black Text vs. Blue Boxes

This distinction is the most important rule in this skill.

### Black body text

Black body text means slide-derived content.

It may include:

- original slide wording;
- light same-meaning paraphrase;
- faithful condensation of verbose slide text;
- faithful structural organization;
- local reordering of neighboring knowledge points when it improves clarity;
- explanations that are directly stated by the slide deck;
- explanations that are strongly and unambiguously implied by the slide deck.

Black body text must remain traceable to the slide deck.

Do not use black body text for:

- external textbook knowledge not present in the slides;
- your own examples;
- your own intuition-building analogies;
- extended background knowledge;
- speculative explanation;
- exam predictions;
- claims whose source boundary is uncertain.

When unsure whether a statement is slide-derived or AI-added, put it into `aiexplainbox` instead of black body text.

Paraphrasing and condensing are expected. The note must not read like a sentence-by-sentence transcript of the slides.

Apply same-meaning rewriting:

- merge near-duplicate slide sentences into a single concise statement;
- drop filler, throat-clearing, and ceremonial phrases that the slide deck used to fill space;
- compress a long descriptive sentence into a short one when the meaning is preserved;
- split a dense slide sentence into a clean numbered or bulleted list when the slide conveys a sequence, a set of conditions, or a classification.

Do not:

- replace the slide's meaning;
- introduce new claims, parameters, or conclusions not present on the slide;
- rewrite a precise technical definition into vague language;
- aggressively reword domain terminology that the course is teaching the student to recognize.

The target is a clean note that a reader can skim and review, not a verbatim retyping of the deck.

### Light-blue AI explanation boxes

Use `aiexplainbox` for all AI-added content.

Allowed AI-added content:

- short explanations for difficult or overly compressed slide content;
- symbol explanations when a formula appears without enough context;
- simple examples that help understand the current concept;
- local intuition for algorithms, geometric structures, architecture diagrams, mathematical graphs, or process flows;
- limited background knowledge that directly supports the current slide concept;
- local reminders when a concept is easily misunderstood.

AI-added content must stay close to the current knowledge point. It must not become a separate lecture, a textbook chapter, or a long external extension.

There is no rigid word limit for blue boxes, because some slides are extremely compressed. The guiding rule is:

> Supplement only as much as needed to understand the current knowledge point. Do not let the blue box dominate the slide-derived black content.

Use short English box titles according to content:

```latex
\begin{aiexplainbox}[Explanation]   % 直觉解释、原理说明
...
\end{aiexplainbox}

\begin{aiexplainbox}[Tips]          % 符号说明、局部提醒、易混淆点
...
\end{aiexplainbox}

\begin{aiexplainbox}[Note]          % 补充背景、延伸说明
...
\end{aiexplainbox}
```

For worked examples, math derivations, algorithm trace-throughs, and problem walk-throughs, use the dedicated `examplebox` environment instead.

Do not use Chinese titles for boxes. Keep titles to one word when possible.

Do not represent AI-added content merely by changing the text color. Use the light-blue box environment so the boundary is visually clean and printable.

## Document Organization

The note must follow this overall structure:

1. Title page from the template.
2. Table of contents.
3. Starred opening section: `\section*{本节知识体系}`.
4. Slide-based knowledge point sections.
5. Optional local comparisons, formula explanations, code explanations, or visual explanations placed near the relevant knowledge point.
6. A short ending section only if the slide deck itself has a meaningful ending or if a brief closure helps the note. Do not force a generic summary.

### Table of contents depth

Set:

```latex
\setcounter{tocdepth}{2}
```

The table of contents should show `\section` and `\subsection` only. Do not show `\subsubsection` or deeper levels.

The opening knowledge-structure section must not appear in the TOC. Use:

```latex
\section*{本节知识体系}
\begin{structurebox}
...
\end{structurebox}
```

Do not use numbered `\section{本节知识体系}`.

### Opening knowledge structure

The first section must not be a mechanical list of slide titles.

It should explain:

- what central topic this deck is about;
- what problem or learning goal the deck addresses;
- the main conceptual blocks;
- how the blocks depend on or lead to one another;
- where formulas, methods, diagrams, examples, or applications fit into the structure.

This part may reorganize the deck conceptually, but it must still remain faithful to the slide content.

Use:

```latex
\section*{本节知识体系}
\begin{structurebox}
...
\end{structurebox}
```

### Section hierarchy

The LaTeX heading levels should generally reflect the PPT's own outline.

Default mapping:

- PPT top-level section → `\section{}`
- PPT sub-section → `\subsection{}`
- Finer knowledge points within a subsection → `\subsubsection{}` or a bold run-in heading

Do not promote PPT sub-sections into `\section` just because they cover several topics. If the PPT has three top-level sections, the note should typically have around three `\section` entries, plus the starred knowledge-structure section, not seven or eight.

Light restructuring is allowed when the PPT's own outline is weak:

- merge two PPT sub-sections that cover the same concept into one `\subsection`;
- split a single overloaded sub-section into two when it clearly contains two unrelated topics;
- rename a heading for clarity when the original is too vague to be useful;
- drop a trivial PPT heading that contains almost no content and fold it into the neighboring section.

Restructuring should be modest. The reader should still be able to map the note's outline back to the PPT without confusion.

Do not invent a new top-level structure that the PPT does not support, and do not flatten everything into a single list.

### Knowledge-point writing

After the knowledge structure, explain the deck knowledge point by knowledge point.

Rules:

- Respect the slide deck's content boundary.
- Do not convert the note into a free textbook chapter.
- Do not generate standalone exam modules by default.
- Do not blindly summarize page by page.
- Organize by coherent knowledge points.
- Local reordering of neighboring slide content is allowed when it makes the logic clearer.
- Large-scale reordering across unrelated sections is not allowed.
- If a local comparison is necessary, place it near the relevant concept rather than in a separate global comparison chapter.
- Whenever a slide presents enumerable content, such as types, properties, steps, conditions, components, advantages, disadvantages, or factors, present it as a numbered or bulleted list rather than a running paragraph.
- Write the knowledge directly. Do not narrate the slide deck.

Avoid phrases such as:

- “课件列举”
- “课件指出”
- “课件提出”
- “课件中说明”
- “PPT 中提到”
- “PPT 指出”
- “本节课件介绍”
- “幻灯片展示”
- “幻灯片中”
- “如课件所示”

The reader should feel they are reading a note about the subject, not a description of someone else's slides.

## Figure Handling

Figures are essential for this skill. Many slide decks carry their most important meaning in diagrams, plots, UML diagrams, remote-sensing images, algorithm visualizations, mathematical geometry, charts, screenshots, and software architecture figures.

### Figure selection principles

Include a figure when it materially improves understanding.

Prefer including:

- algorithm visualizations;
- mathematical diagrams;
- geometric diagrams;
- UML diagrams;
- software architecture diagrams;
- workflow or pipeline diagrams;
- remote-sensing image examples or processing comparisons;
- plots, curves, benchmark charts, or ablation charts;
- tables or diagrams that encode relationships better than prose;
- dense visual slides whose meaning cannot be captured by text alone.

Avoid including:

- pure title slides;
- decorative images;
- repeated images;
- low-information icons;
- full-slide screenshots when only one region matters;
- unreadable images that cannot be improved by cropping or scaling.

### Figure accuracy

Cropping accuracy is critical. A wrong crop that cuts off labels, axes, legends, or key regions is worse than no figure.

Before saving any crop:

- visually verify that the crop contains the complete intended region;
- confirm no label, axis tick, legend entry, arrow, formula, or key annotation is cut off at the edge;
- if the crop looks wrong or ambiguous, re-crop rather than inserting a bad figure;
- do not infer crop coordinates from text extraction alone;
- always verify against the rendered slide image.

### Visual inspection rule

Figure understanding must come from actual visual inspection of the rendered slide page, not only from extracted text or filenames.

Do not rely solely on OCR or extracted text to decide whether a figure is useful. Inspect the slide image, crop candidates, and final crop before inserting it.

### Avoid inline image placeholders

Do not insert raw image references such as `[Image #1]`, `[Figure]`, `[Insert figure here]`, or similar placeholder text into the note body.

Every figure must be inserted with a proper `\notefigure` command using a real file path and a meaningful caption.

If a figure file is not ready, omit it entirely rather than leaving a placeholder.

Prefer cropping the key region over inserting the entire slide.

Use full-slide screenshots only when the whole slide layout is necessary for understanding.

When cropping:

- keep all labels, legends, axes, arrows, formulas, and contextual annotations needed for comprehension;
- avoid cutting off context that makes the figure understandable;
- crop tightly enough to avoid wasting page space;
- enlarge the figure in LaTeX if labels are small;
- reject figures that remain unreadable after cropping.

### Figure naming

Figure files may preserve slide numbers for traceability, but the PDF body should not display slide page references unless the user explicitly asks for them.

Recommended naming:

```text
figures/slide_08_geometry_model.png
figures/slide_15_algorithm_flow.png
figures/slide_23_uml_architecture.png
```

### Figure insertion

Use the template helper:

```latex
\notefigure[0.72]{figures/slide_08_geometry_model.png}{几何模型示意图}
```

The optional first argument controls width as a fraction of `\linewidth`. The template default is `0.72`.

Adjust figure width based on density and importance:

- `0.50–0.65`: small diagrams, simple flowcharts, icons, or figures that are clearer when compact;
- `0.68–0.78`: most algorithm visualizations, UML diagrams, mathematical plots, and architecture diagrams;
- `0.82–0.92`: dense figures with many labels, wide tables, or full-slide layouts that need space to remain readable.

Do not default to `0.90+` for every figure. Large figures waste vertical space and make the note feel sparse. When in doubt, start at `0.72` and increase only if labels become unreadable.

Figures should use `[H]` placement through the template so they stay close to the relevant explanation.

Captions should describe what the figure supports, not merely repeat the filename.

## PPT/PDF Processing Workflow

Normalize all inputs to PDF first.

Recommended workflow:

1. Inspect the input filename and metadata.
2. If the input is PPT/PPTX, convert it to PDF.
3. Extract slide text page by page from the PDF when possible.
4. Render each PDF page into a high-resolution image.
5. Inspect rendered pages to identify meaningful visual regions.
6. Crop important figures and save them into `figures/`.
7. Build the LaTeX note using the template.
8. Compile with XeLaTeX.
9. Deliver `.tex`, `.pdf`, and referenced figure assets.

Recommended tools when available:

- Use LibreOffice headless to convert PPT/PPTX to PDF.
- Use PyMuPDF, `pdftoppm`, or a comparable renderer to render PDF pages into high-resolution PNG images.
- Use Python/Pillow, PyMuPDF clipping, or another precise image tool for figure crops.
- Save crop previews and inspect final crop outputs before inserting them.

When the PDF is image-based or text extraction is poor, rely on visual understanding from rendered page images. Do not assume the text extraction contains all important slide content.

## Long Deck Strategy

For long slide decks, do not rely on one monolithic pass.

If the deck has many pages or dense visual content:

- internally split the deck into coherent slide ranges;
- process each range for text, figures, and knowledge points;
- integrate the results into one unified note;
- remove duplicate explanations;
- keep the final document coherent rather than stitched together from chunk summaries.

Do not ask the user to manually choose chunks unless the deck is ambiguous, corrupted, password-protected, or too large to process in the current environment.

## Writing Style

Write in Chinese by default.

The note should feel like an elegant course note, not an AI-generated outline.

Follow these rules:

- Use clear sections and subsections.
- Break dense slide content into bullet lists, numbered steps, or short labeled paragraphs whenever the content is enumerable.
- Avoid long unbroken paragraphs.
- Use compact paragraphs only for true narrative explanation of relationships or reasoning.
- Within a paragraph, insert line breaks at natural logical seams rather than letting one block of text run for many lines.
- A single visual chunk should typically be no more than three or four lines.
- Use tables only for local comparisons or structured slide content.
- Avoid filler such as “本文主要介绍了”, “综上所述”, and vague generic summaries.
- Avoid repetitive AI-style contrast patterns such as overusing “不是……而是……”.
- Avoid empty high-level statements that do not mention concrete slide concepts.
- Use `\textbf{}` to bold key terms, definitions, or the single most important phrase in a sentence.
- Bold sparingly: at most one or two bolded phrases per paragraph; never bold a whole sentence for decoration.
- Do not over-expand beyond the slides.
- Do not invent facts, results, parameters, conclusions, or course requirements.
- Do not insert citation placeholders.
- Do not include web links or external references unless the user explicitly asks.

## Formula Handling

When the slide deck contains formulas:

- preserve the formula accurately;
- explain what problem the formula expresses when needed;
- if the slide already explains the formula sufficiently, keep the note concise;
- if the formula appears abruptly or lacks symbol explanations, add a nearby `aiexplainbox` with title `Tips` or `Explanation`;
- do not add long derivations unless understanding the formula depends on a missing step;
- do not introduce advanced theory not needed for the current formula.

Use stable LaTeX display math:

```latex
\[
  ...
\]
```

Avoid single-dollar inline math. Prefer `\( ... \)` for inline formulas and `\[ ... \]` for display formulas.

## Code Handling

For coding-heavy courses such as 算法设计与分析, 数据库原理, 面向对象程序设计, 数据结构, 编译原理, 操作系统, 计算机网络, and similar courses, the note should include key code when the slide deck is teaching code or algorithmic procedure.

### When to include code

Include:

- core algorithm bodies, such as the loop, recursion, or key state update;
- canonical data structure forms, such as class skeletons, structs, and key member functions;
- representative SQL statements, query plans, or schema definitions;
- minimal but complete examples that demonstrate a language feature being taught;
- the few lines that the slide is actually teaching, not surrounding boilerplate.

Omit:

- full file headers, includes, and main wrappers when they distract from the concept;
- repeated near-identical snippets;
- generated or framework-only scaffolding code that is not part of the lesson;
- entire programs when only a handful of lines carry the slide's meaning.

### Formatting rules

Use the template's `lstlisting` style. Do not redefine the style inline.

Use:

```latex
\begin{lstlisting}[language=C++,caption={快速排序的核心划分过程}]
...
\end{lstlisting}
```

Set `language=` to match the slide content, for example `C`, `C++`, `Java`, `Python`, `SQL`, `Matlab`, or `Pascal`.

Always provide a short Chinese `caption=` that names what the snippet is, not what file it came from.

For pseudo-code that does not match a real language, use `language={}` and preserve the original notation from the slide.

If a piece of code only needs a one-line mention, use `\lstinline|...|` instead of opening a full block.

### Commenting rules

Every non-trivial code block should include concise, necessary inline comments that explain:

- the purpose of non-obvious variables or states;
- the invariant maintained by a loop;
- the meaning of a boundary condition or edge case;
- why a particular operation is performed when it is not self-evident from the code.

Do not comment every line. Do not restate what the code literally does. Comment the why and the conceptual role, not the syntax.

If the slide's original code already has good comments, preserve them. If the original code has no comments or unclear comments, add the minimum necessary comments to make the logic followable.

If the original code appears unclear or possibly wrong, explain the issue in an `aiexplainbox` rather than silently changing the meaning.

### After-code explanation

After a non-trivial code block, add an `aiexplainbox` with title `Explanation` only when the code's logic, invariants, or key variables are not obvious from the code itself.

Do not narrate every line. Do not turn the note into a full programming tutorial.

## Worked Examples

For courses that require visible reasoning, such as 算法设计与分析, 离散数学, 高等数学, 线性代数, 概率论, 数理统计, 信号处理, 机器学习，强化学习 and similar courses, when the slide presents a problem, the note must walk through the thought process clearly. A bare answer is not enough.

Use the `examplebox` environment for every worked problem:

```latex
\begin{examplebox}[Example]
\textbf{题目.} 在此完整复述题目条件，不要只写“如上题”。

\textbf{思路.} 用一两句话点明本题的关键观察、所属方法或转化方向。

\textbf{推导.}
\begin{enumerate}
  \item 第一步——给出第一步的操作和它的依据；
  \item 第二步——把上一步的结果代入或继续变形；
  \item 第三步——逼近答案的最后一步。
\end{enumerate}

\[
  \text{关键中间式} \quad \Rightarrow \quad \text{下一式}
\]

\textbf{答案.} 给出最终结果，并在必要时附一句结果的物理意义或边界检查。
\end{examplebox}
```

Rules for worked examples:

- the box must be self-contained;
- restate the problem so the reader does not need to flip back;
- explain why each step is taken, not only what is done;
- show key intermediate expressions in display math;
- keep prose concise but complete;
- do not skip the hard step;
- do not invent problems;
- use only examples that the slide deck actually provides.

If the slide gives a problem but no solution, the note may include AI-supplemented reasoning, but it must be explicitly marked at the beginning of the box:

```latex
\begin{examplebox}[Derivation]
\textbf{说明.} 本题解答为补充推导，原课件仅给出题目条件。
...
\end{examplebox}
```

Use `\begin{examplebox}[Proof]`, `[Derivation]`, `[Case]`, or `[Problem]` when the content fits a more specific label.

## Algorithm and Process Handling

When the slide deck contains algorithms, workflows, or processes:

- preserve the original algorithm logic;
- organize the content into input, output, and key steps only when that helps readability;
- explain difficult variables or state transitions in a nearby `aiexplainbox`;
- do not add complexity analysis unless the slide mentions it or it is necessary for understanding;
- include the slide's algorithm visualization or flowchart when it is useful.

For algorithm descriptions, prefer a compact structure:

```latex
\begin{notebox}[Process]
\textbf{输入：} ...

\textbf{输出：} ...

\textbf{步骤：}
\begin{enumerate}
  \item ...
  \item ...
\end{enumerate}
\end{notebox}
```

Use black text or `notebox` for slide-derived algorithm structure. Use `aiexplainbox` only for AI-added clarification.

## Local Comparisons and Reminders

Do not create global modules such as:

- 名词解释表;
- 易错点总表;
- 简答题规范答案;
- 高频考点预测;
- 选择题陷阱;
- 题库;
- 背诵版.

Local comparisons or reminders are allowed when they are needed to understand the current slide concept.

Rules:

- place them near the relevant knowledge point;
- keep them short;
- if the comparison is slide-derived, use black text or `notebox`;
- if the comparison includes AI-added clarification, put that part in `aiexplainbox`.

Use:

```latex
\begin{notebox}[Tips]
...
\end{notebox}
```

or:

```latex
\begin{notebox}[Compare]
...
\end{notebox}
```

Use short English titles. Do not use Chinese titles for boxes.

## Fallback Strategy

Use fallback behavior when the ideal workflow cannot be completed.

### If PPT/PPTX conversion fails

- Report the conversion issue.
- Use a PDF version when the user provides one.
- Do not invent slide content from the filename or topic.
- If partial conversion succeeds, process the converted pages and clearly state the limitation.

### If text extraction is poor

- Rely on rendered page images and visual inspection.
- Preserve uncertain content conservatively.
- Avoid hallucinating formulas, labels, numbers, or definitions.
- If a label is unreadable, do not guess it; omit the crop or state uncertainty in the note only when necessary.

### If the template is missing

- Use a minimal fallback `ctexart` template.
- Preserve required environments: `structurebox`, `aiexplainbox`, `examplebox`, `notebox`, `\notefigure`, and `lstlisting` style.
- Keep the visual design simple and printable.
- Do not stop solely because `assets/notes-template.tex` is unavailable.

### If XeLaTeX compilation fails

- Preserve the `.log` file for debugging.
- Fix missing packages, image paths, Unicode issues, and syntax errors when possible.
- Compile enough times to resolve the table of contents and references.
- If PDF generation still cannot be completed, deliver the `.tex`, figure assets, and a concise explanation of the compile error.

### If the deck is too long or dense

- Process the most coherent complete slide ranges possible.
- Avoid generating a fragmented or half-structured note.
- State clearly which parts were completed and which were not.
- Prefer partial but faithful completion over over-compressed hallucinated completion.

## Technical Delivery Requirements

Before final delivery, perform technical checks:

- the `.tex` file exists;
- all referenced figure paths exist;
- the `.tex` compiles with XeLaTeX when possible;
- the `.pdf` is generated successfully when possible;
- figures appear near the relevant explanations;
- no missing image errors remain;
- the opening knowledge structure is starred and not included in the TOC;
- `tocdepth` is set to `2`;
- there are no raw image placeholders such as `[Image]` or `[Figure]`;
- no forbidden slide-narration phrases remain unless they appear inside quoted original slide text.

Recommended compile command:

```bash
latexmk -xelatex -interaction=nonstopmode notes.tex
```

If `latexmk` is unavailable, compile with XeLaTeX enough times to resolve the table of contents and references:

```bash
xelatex -interaction=nonstopmode notes.tex
xelatex -interaction=nonstopmode notes.tex
```

After successful compilation, delete intermediate build artifacts:

- `.aux`
- `.log`
- `.toc`
- `.out`
- `.fls`
- `.fdb_latexmk`
- `.synctex.gz`
- other temporary files produced by XeLaTeX or latexmk

Do not delete the `.log` file if compilation fails.

## Final Delivery

Deliver:

- the final `.tex` file;
- the compiled `.pdf` file when compilation succeeds;
- the `figures/` directory containing all referenced figures;
- preferably a zipped project folder containing `.tex`, `.pdf`, and `figures/`.

Do not deliver unnecessary intermediate build artifacts after successful compilation.

## Non-goals

This skill is not for:

- generating exam predictions;
- writing standardized exam answers;
- producing flashcards;
- creating question banks;
- writing a textbook chapter;
- producing a pure slide-by-slide transcript;
- copying slides without restructuring;
- replacing the original course material;
- adding extensive external knowledge.