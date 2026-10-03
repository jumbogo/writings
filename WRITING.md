# Writing a Post

Each post is a folder under `jumbo-space/content/`, holding an `index.md` with the
post's images and videos next to it.

| Section | Folder | Language | Categories in use |
| --- | --- | --- | --- |
| Tech | `content/tech/<slug>/` | English | — (none yet) |
| Life | `content/life/<slug>/` | Chinese | `travel`, `running`, `journal` |

Keep each post in one language. Mixed-language pages read strangely.

## Quick checklist

1. `git checkout main && git pull`, then `git checkout -b feat/<slug>`
2. `cd jumbo-space && hugo new content <section>/<slug>/index.md`, then fill in the title, description and tags
3. Drop the photos into the same folder (resize them first, see below)
4. `cd jumbo-space && hugo server -D` and write while watching http://localhost:1313
5. Remove `draft: true`, then commit, push and open a PR
6. Merge the PR. The site deploys by itself, and GitHub deletes the branch

## 1. Create the post

The slug becomes the URL (`/life/oregon-trip/`). Use short lowercase English
words with hyphens, even for Chinese posts, because Chinese characters in URLs
turn into long `%E5%...` strings when shared.

```bash
cd jumbo-space
hugo new content life/my-new-post/index.md
```

This creates the folder and an `index.md` with all the front matter filled in
from `archetypes/life.md` (or `archetypes/tech.md`). Replace the title, which
starts as the slug in title case, and fill in the rest:

```yaml
---
title: '标题'
date: 2026-10-04T09:00:00-07:00
description: "One sentence shown in post lists and link previews."
author: jumbo chow
tags: ["travel"]
categories: ["travel"]
# feature: cover.jpg
draft: true
---
```

- **date** controls the post's order on the site. A future date hides the post
  until that time.
- **draft: true** keeps the post off the live site. `hugo server -D` still shows it.
- Reuse existing categories where they fit, so the lists stay tidy.
- **Cover image:** name a photo `feature.jpg` (any `feature*` name works) and
  it becomes the post's cover and its thumbnail in lists. To use a different
  file, set `feature: <file>`.

## 2. Images

Use plain Markdown with a path relative to the post folder. The text in
brackets is the caption:

```markdown
![落日](sunset.jpg)
```

**Photo rows:** images on consecutive lines (no blank line between them) show
side by side in one row. A blank line starts a new row.

```markdown
![石拱](arch.jpg)
![海象](rock00.jpg)
![棕榈树](palm00.jpg)

![路线图](map.png)
```

**Resize before committing.** Phone photos are 3–8 MB each and every one stays
in git history forever. About 2000 px on the long side is plenty for the web:

```bash
mogrify -resize '2000x2000>' -quality 85 *.jpg   # ImageMagick, edits in place
```

Prefer lowercase file names with no spaces (`hidden-valley.jpg`, not
`Hidden Valley.JPG`).

## 3. Videos

- **Short clips (a few MB):** put the MP4 in the post folder and embed it with
  HTML:

  ```html
  <video src="clip.mp4" controls playsinline width="100%"></video>
  ```

  Convert iPhone `.MOV` files to H.264 MP4 first, since not every browser plays
  them:

  ```bash
  ffmpeg -i IMG_1234.MOV -vcodec libx264 -crf 28 -vf "scale=-2:1080" -acodec aac clip.mp4
  ```

- **Anything larger:** upload it to YouTube (unlisted is fine) and embed it:

  ```markdown
  {{< youtube VIDEO_ID >}}
  ```

  GitHub refuses files over 100 MB and limits the Pages site to 1 GB, so long
  videos don't belong in the repo.

## 4. Preview

```bash
cd jumbo-space && hugo server -D
```

The page reloads on every save. Check the post on a narrow window too, since
most readers are on phones.

## 5. Publish

```bash
git add jumbo-space/content/<section>/<slug>
git commit -m "feat(life): add post about <topic>"
git push -u origin HEAD
gh pr create --fill
```

Merge the PR on GitHub. The **Actions** tab shows the deploy, and the post is
live at `https://jumbochow.com/<section>/<slug>/` about a minute later.

## Fixing a published post

Same flow: branch, edit, PR, merge. To change a post's slug after it's been
shared, add the old URL to the front matter so existing links keep working:

```yaml
aliases: ["/life/old-slug/"]
```
