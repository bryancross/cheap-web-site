# cheap-web-site

Live site: https://bryancross.github.io/cheap-web-site/

## Layout

- `site/` — everything published to GitHub Pages. Only this folder is deployed.
- `.github/workflows/pages.yml` — the deploy workflow.

## Publishing

Pushing to `main` does **not** publish. Commit drafts freely, then deploy when ready:

```sh
git tag v2 && git push origin v2      # deploy by tagging a release
gh workflow run pages.yml             # or deploy the current main manually
```

Deploy history is under the repo's **Actions** tab and **Environments → github-pages**.
