# cheap-web-site

Live site: https://bryancross.github.io/cheap-web-site/

## Layout

- `site/`: everything published to GitHub Pages. Only this folder is deployed.
  - `site/index.html`: the home page, a list of links to every page.
  - `site/<slug>/index.html`: one folder per page, served at `https://bryancross.github.io/cheap-web-site/<slug>/`.
  - `site/<slug>/…`: images and other files used only by that page.
  - `site/assets/`: CSS, fonts and images shared across pages.
- `.github/workflows/pages.yml`: the deploy workflow.

Pushing to `main` does **not** publish. Only a `v*` tag or a manual workflow run deploys.

## Publishing a new page

### 1. Create the page

Pick a slug: lowercase and hyphenated, e.g. `electric-sheep`.

```sh
mkdir site/electric-sheep
# write site/electric-sheep/index.html; put its images alongside it
```

Use **relative links** (no leading `/`), because the site lives under `/cheap-web-site/`:

| From | Link to | Write |
| --- | --- | --- |
| a page | its own image | `photo.jpg` |
| a page | shared CSS | `../assets/style.css` |
| a page | the home page | `../` |
| home page | a page | `electric-sheep/` |

### 2. Link it from the home page

Add a link card for the new page in `site/index.html`, pointing to `electric-sheep/`.

### 3. Preview locally

```sh
python3 -m http.server -d site 8000
```

Open http://localhost:8000/, click through to the new page, and check that images and styles load. Stop the server with Ctrl+C.

### 4. Commit and push (this does not publish)

```sh
git add site/
git commit -m "Add electric-sheep page"
git push
```

### 5. Publish

Find the latest tag and bump it:

```sh
git tag --sort=-v:refname | head -1     # e.g. v3
git tag v4 && git push origin v4
```

Or deploy the current `main` without a tag:

```sh
gh workflow run pages.yml
```

### 6. Confirm it's live

```sh
gh run watch        # pick the "Deploy to GitHub Pages" run
```

Or check the repo's **Actions** tab. When the run is green (about a minute), open
`https://bryancross.github.io/cheap-web-site/electric-sheep/`. If you still see the old version, do a hard refresh (Cmd+Shift+R).

## Troubleshooting

- **404 on the new page**: check that the folder is under `site/`, the file is named `index.html`, and the URL slug matches the folder name exactly (it's case-sensitive). Broken images or CSS usually mean a link starts with `/`; make it relative.
- **Deploy rejected / environment protection error**: the tag must start with `v` (e.g. `v4`). Other tag names aren't allowed to deploy.
- **Page isn't updating**: did you tag *after* pushing? A tag deploys whatever commit it points to. Tag the latest commit with a new version, or run `gh workflow run pages.yml`. Then hard refresh.
- **Deploy history**: see the **Actions** tab, or **Environments → github-pages** on the repo page.
