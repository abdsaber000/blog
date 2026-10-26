# Personal notes site (GitHub Pages)

Minimal Jekyll site for mathematics notes, programming posts, PDF handouts, YouTube videos, and an about page. Math is written as LaTeX in Markdown and rendered with [KaTeX](https://katex.org/).

## One-time setup

1. Create a GitHub repository named **`YOUR_USERNAME.github.io`** (must match your GitHub username exactly).
2. In [`_config.yml`](_config.yml), set:
   - `url: "https://YOUR_USERNAME.github.io"`
   - `author: "Your Name"`
3. Push this project to that repository (`main` branch, site at repo root).
4. GitHub → **Settings → Pages** → Source: **GitHub Actions** (the workflow in `.github/workflows/pages.yml` builds and deploys).
5. After the Actions run finishes, open `https://YOUR_USERNAME.github.io`.

### Local preview

Needs Ruby 3+ (on macOS: `brew install ruby`, then put Homebrew Ruby on your `PATH`).

```bash
bundle install
bundle exec jekyll serve
```

Open [http://127.0.0.1:4000](http://127.0.0.1:4000).

## Add a note or blog post

Create a file under `_posts/` named `YYYY-MM-DD-slug.md`:

```markdown
---
title: "My note title"
date: 2026-10-26
tags: [math]          # or [programming], or both
excerpt: "One-line summary for the home page."
---

Your Markdown here. Inline math $E = mc^2$ and display math:

$$
\int_0^1 x^2\, dx = \frac{1}{3}.
$$
```

Images go in `assets/images/...` and are referenced like:

```markdown
![Caption]({{ '/assets/images/my-note/figure.png' | relative_url }})
```

## Convert LaTeX → Markdown (Pandoc)

If you already write `.tex` notes:

```bash
pandoc note.tex -o _posts/2026-10-26-note.md \
  --extract-media=assets/images/note \
  -t markdown+tex_math_dollars
```

Then open the Markdown file and:

1. Add Jekyll front matter (`title`, `date`, `tags`, optional `excerpt`).
2. Check image paths (Pandoc writes relative paths under `assets/images/note/`).
3. Prefer `$...$` / `$$...$$` for math (KaTeX on this site).

**What converts well:** equations, sections, lists, most figures (as images), basic tables.

**What does not match a PDF 1:1:** TikZ, custom theorem environments, exotic packages. Compile those to PDF and add them under **Handouts** instead.

## Add a handout (PDF)

1. Put the file in `assets/handouts/my-file.pdf`.
2. Add an entry in [`_data/handouts.yml`](_data/handouts.yml):

```yaml
- title: Midterm solutions
  file: /assets/handouts/my-file.pdf
  date: 2026-03-15
  description: Worked solutions for the practice midterm.
```

## Add a YouTube video

Add an entry in [`_data/videos.yml`](_data/videos.yml). Use the id from `youtube.com/watch?v=VIDEO_ID`:

```yaml
- title: My first video
  youtube_id: VIDEO_ID
  date: 2026-11-01
  description: Optional short blurb.
```

## About / resume

- Edit [`about.md`](about.md) for your bio.
- Replace [`assets/resume.pdf`](assets/resume.pdf) with your resume.

## Site map

| Path | Purpose |
|------|---------|
| `_posts/` | Notes and blog posts |
| `handouts/` | Handouts listing page |
| `assets/handouts/` | PDF files |
| `videos.md` | YouTube page |
| `about.md` | About + resume link |
| `_data/handouts.yml` | Handout metadata |
| `_data/videos.yml` | Video metadata |
