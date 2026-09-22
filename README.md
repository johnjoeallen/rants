# rants

Various rants on software, architecture, and engineering, published as a
GitHub Pages site (MkDocs + Material).

Site: https://johnjoeallen.github.io/rants/

Posts live in `docs/posts/`. To add a new rant, drop a Markdown file there
and link it from `docs/index.md` and from the `nav:` section of
`mkdocs.yml`.

A GitHub Actions workflow (`.github/workflows/deploy.yml`) builds and
publishes the site to the `gh-pages` branch on every push to `main`.

## Local preview

```sh
pip install -r requirements.txt
mkdocs serve
```
