# thearchive.nz

Hugo site using the [Blowfish](https://blowfish.page/) theme, v3.9.0.

The theme is **not** a submodule or a `themes/` directory. Its `layouts/`,
`assets/`, `i18n/`, `static/`, `data/` and `archetypes/` have been copied into
the project root, so everything Hugo needs is in one tree and `theme = ` is not
set in the config.

## Requirements

Hugo **extended** 0.166.0 or newer (`brew install hugo`). Netlify is pinned to
the same version in `netlify.toml`.

## Local development

```sh
hugo server -D     # -D includes drafts
```

Then open http://localhost:1313.

## Writing a post

```sh
hugo new posts/my-post-title
```

That creates `content/posts/my-post-title/index.md` as a page bundle, using
`archetypes/posts/index.md`. Keep images for the post in the same folder and
reference them relatively:

```markdown
![alt text](screenshot.png)
```

For a hero banner, name an image `feature.jpg` (or `cover.jpg`) in the bundle
and uncomment `showHero` / `heroStyle` in the front matter.

Posts are created with `draft: true`. Flip it to `false` to publish.

A flat file also works if a post has no images:

```sh
hugo new posts/my-post-title.md
```

Set `externalUrl` in the front matter (see `archetypes/external.md`) to make a
post link out to another site instead of rendering its own page.

## Configuration

| File | Purpose |
| --- | --- |
| `config/_default/hugo.toml` | Base URL, taxonomies, pagination, outputs |
| `config/_default/params.toml` | Theme options: layouts, article/list display, analytics |
| `config/_default/languages.en.toml` | Site title, author profile, social links |
| `config/_default/menus.en.toml` | Header and footer navigation |
| `config/_default/markup.toml` | Goldmark, syntax highlighting, table of contents |

Options are documented at https://blowfish.page/docs/configuration/.

## What was removed to keep the repo small

`assets/lib/mermaid/` (5.2 MB) was deleted, so the `{{< mermaid >}}` shortcode
will fail the build. To restore it:

```sh
mkdir -p assets/lib/mermaid
curl -L -o assets/lib/mermaid/mermaid.min.js \
  https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.min.js
```

Also dropped: the theme's `exampleSite/`, screenshots, READMEs, Node tooling,
and all `i18n/` translations except `en.yaml`. KaTeX and Chart.js were kept.

## Upgrading Blowfish

Because the theme lives in the root, upgrading means re-copying the directories
above from a new release and re-applying any local edits. Nothing under
`layouts/` has been modified, so a straight overwrite of `layouts/`, `assets/`,
`i18n/`, `static/` and `data/` is safe; `config/` holds local values and should
be merged by hand.

## Deployment

Netlify builds with `hugo --gc --minify` and publishes `public/`, which is
gitignored.
