# Publish to GitHub

这份仓库已经适合以“单 Skill 仓库”的形式公开发布。建议公开的内容只有：

- `ppt2notes/`
- `docs/images/`
- `example/`
- `README.md`
- `LICENSE`

## Before You Publish

发布前检查：

1. 确认 [`ppt2notes/SKILL.md`](../ppt2notes/SKILL.md) 中的描述已经是你希望公开暴露的最终版本。
2. 确认 [`example/`](../example/) 中的 PDF 不包含不适合公开的课程内容或版权风险。
3. 确认 README 中展示图和示例名称都是你希望外部看到的版本。
4. 如果你想用别的许可证，先替换根目录 `LICENSE`。

## Recommended Repository Settings

GitHub 仓库建议：

- Repository name: `ppt2notes-skill`
- Description: `Convert a single PPT/PPTX/PDF lecture deck into faithful Chinese LaTeX course notes and a compiled PDF.`
- Topics: `skill`, `codex`, `latex`, `ppt`, `pdf`, `notes`, `education`

## First Publish

如果本地目录还不是一个 Git 仓库，可以在当前目录执行：

```bash
git init
git add .
git commit -m "Initial release: ppt2notes skill"
```

然后去 GitHub 创建一个空仓库，例如 `ppt2notes-skill`，不要勾选自动生成 `README`、`.gitignore` 或 `LICENSE`。

创建完后，把远端地址接到本地：

```bash
git remote add origin git@github.com:<your-name>/ppt2notes-skill.git
git branch -M main
git push -u origin main
```

如果你使用 HTTPS，也可以把远端换成：

```bash
git remote add origin https://github.com/<your-name>/ppt2notes-skill.git
```

## Recommended Release

首次推送后，建议再打一个 release tag：

```bash
git tag -a v1.0.0 -m "First public release"
git push origin v1.0.0
```

然后在 GitHub 页面进入 `Releases`:

1. 点 `Draft a new release`
2. 选择 `v1.0.0`
3. Title 填 `v1.0.0`
4. 描述里简要写：

```text
- Initial public release of the ppt2notes skill
- Includes SKILL.md, LaTeX template, preview images, and example PDFs
```

## After Publish

公开后建议补这几项：

1. 在仓库右侧编辑 About，补 description 和 topics。
2. 把 README 顶部截图确认一遍，检查图片是否正常加载。
3. 点开 `example/` 下 PDF，确认 GitHub 可正常预览或下载。
4. 如果后续会持续改动，按 `v1.0.1`、`v1.1.0` 这类版本号继续发 release。
