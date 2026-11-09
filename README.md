# Electric Soup

My website, <https://ntouchet.github.io>. It's a [Jekyll](https://jekyllrb.com) site
hosted on GitHub Pages: **anything pushed to `main` goes live in a minute or two.**

---

## Writing a new post

```sh
./new-post "Chapter 4"
```

It asks where the post goes (home page, Academics, a series like the DSP book,
Projects…), creates the file with today's date, and makes a folder for its
images. Then write in the file it prints, preview, and push.

Skip the question by naming the place directly:

```sh
./new-post "Chapter 4" --in dsp-smith      # into a series
./new-post "Cool idea" --in projects       # into a section
./new-post --list                          # see every place a post can go
./new-post "Chapter 4" --in dsp-smith -e   # ...and open it in $EDITOR
```

### Or by hand

A post is just a markdown file in a `_posts` folder, named `YYYY-MM-DD-some-title.md`,
starting with:

```yaml
---
title: "Chapter 4"
---
```

That's the only thing you *have* to write. Where the file lives decides
everything else, including its URL:

| File                                                  | Shows up on              | URL                              |
|-------------------------------------------------------|--------------------------|----------------------------------|
| `_posts/2026-01-10-hello.md`                          | Home page                | `/hello/`                        |
| `academics/_posts/2026-01-10-notes.md`                | Home + Academics         | `/academics/notes/`              |
| `academics/dsp-smith/_posts/2026-01-10-chapter-4.md`  | Home + Academics + series page | `/academics/dsp-smith/chapter-4/` |
| `projects/_posts/2026-01-10-synth.md`                 | Home + Projects          | `/projects/synth/`               |

### Optional front matter

```yaml
---
title: "Chapter 4"
description: One sentence; shown under the title in lists and in link previews.
favorite: true        # shows the post under "Favorite posts" on the home page (with a ★)
math: true            # force math on (normally detected automatically)
permalink: /custom/url/   # only if you really want a specific URL
---
```

Don't add `categories:`; the folder takes care of that.

---

## Cheat sheet

```markdown
## Big heading            ### Smaller heading         #### Question-sized heading
**bold**   *italic*   [a link](https://example.com)   `inline code`
```

**Images**: put them in `assets/img/<post-name>/` (the script makes this folder), then:

```markdown
![What the image shows](/assets/img/chapter-4/plot.svg)
```

…or with a caption underneath:

```liquid
{% include figure.html src="/assets/img/chapter-4/plot.svg" caption="Figure 1: The decimated signal" %}
```

Images automatically shrink to fit phones. Plain `<img src="..." width="500">` also
still works.

**Math** (rendered by MathJax): `$x^2$` inline, and on its own line:

```markdown
$$
X[k] = \sum_{n=0}^{N-1} x[n] \, e^{-j 2\pi kn/N}
$$
```

To write a literal dollar sign, type `\\$`.

**Code**: put the language after the backticks for colors:

````markdown
```c
for (int i = 0; i < n; i++) sum += x[i];
```
````

**Link to another post**: use its folder + filename (without `_posts` or `.md`):

```liquid
[Chapter 3]({% post_url academics/dsp-smith/2025-11-04-Chapter-Three-Assignment %})
```

If you get the name wrong the build fails with a clear error, so broken links
can't sneak onto the site.

---

## Drafts

```sh
./new-post "Half-baked idea" --in projects --draft
```

Drafts live in a `_drafts` folder and **never go live**, even if you push them.
See them locally with `bundle exec jekyll serve --drafts`. When it's ready:

```sh
./new-post --publish projects/_drafts/half-baked-idea.md
```

---

## Series and sections

A **section** is a top-level area with its own menu link (Academics, Projects).
A **series** is a group of posts inside a section: a textbook, a course, a
multi-part project. Each one is just a folder with an `index.md`:

```
academics/                 ← section   (index.md has  layout: section)
├── index.md
├── _posts/                ← loose posts in this section
└── dsp-smith/             ← series    (index.md has  layout: series)
    ├── index.md
    └── _posts/
```

Section and series pages list their posts automatically. Posts in a series get
"← previous / next →" links.

**New series:**

```sh
./new-post --new-series projects/open-hydrophone "Open Hydrophone"
```

Then edit the `description` in the `index.md` it made. Add `short_title: Hydrophone`
there too if the title is long; that's what shows in the little black tags.

**New section** (rare): make a folder with an `index.md` like `projects/index.md`,
and add it to the menu in `_data/navigation.yml`.

---

## Changing the look

| What                                   | Where                                         |
|----------------------------------------|-----------------------------------------------|
| Menu links                             | `_data/navigation.yml`                        |
| The side GIFs                          | `_data/sidebar_gifs.yml` (files in `assets/img/`) |
| Colors, fonts, sizes                   | top of `assets/css/main.css`                  |
| Site title, description, # of recent posts | `_config.yml`                             |
| Home page intro text                   | `index.markdown`                              |
| About page                             | `about.markdown`                              |
| Page structure (header, window, footer)| `_layouts/default.html`, `_includes/`         |

Keep GIFs small (under ~2 MB each) so phones don't choke. To shrink one:

```sh
ffmpeg -i big.gif -vf "fps=12,scale=200:-1:flags=lanczos,split[a][b];[a]palettegen=reserve_transparent=1[p];[b][p]paletteuse" small.gif
```

---

## Previewing locally

One-time setup (Arch):

```sh
sudo pacman -S ruby base-devel
gem install bundler
bundle config set --local path vendor/bundle
bundle install
```

Then, whenever you're writing:

```sh
bundle exec jekyll serve --livereload
```

Open <http://localhost:4000>. The page reloads every time you save. (Restart
it after editing `_config.yml`.) The `Gemfile` uses the `github-pages` gem, so
this is the same Jekyll version GitHub runs; if it builds here, it builds there.

## Publishing

```sh
git add -A
git commit -m "Chapter 4"
git push
```

Check progress or errors under the repo's **Actions** tab on GitHub.
