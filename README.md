# blog.confined-space.com

Blog for [confined-space.com](https://confined-space.com), deployed as a static site to GitHub Pages.

## Publishing a post

Drop a file into the `uploads/` folder on the `main` branch (via GitHub's web UI, or a local commit + push) and a GitHub Action converts it into a published post automatically. Supported formats:

- **`.docx`** — a Word document. The first heading/line becomes the title; the rest becomes the post body.
- **`.txt`** — a plain-text file, one paragraph per line.
- **`.zip`** — a zip containing one `.docx`/`.txt` plus one image (`.jpg`, `.png`, `.webp`, `.gif`); the image becomes the post's featured image.

Within a minute or two of the push:

1. `.github/workflows/convert-docx.yml` runs `scripts/convert.js`, which generates `posts/<slug>.html`, adds an entry to `posts.json`, and moves the source file to `uploads/processed/`. It commits that back to `main`.
2. That commit triggers `.github/workflows/deploy.yml`, which publishes the site to GitHub Pages.

No manual build step is needed — uploading the file is the entire workflow.

## Local development

```bash
npm install
npm run convert   # process anything sitting in uploads/
```

Then open `index.html` directly, or serve the folder with any static file server.

## GitHub Pages setup (one-time)

In the repo's **Settings → Pages**, set the source to **GitHub Actions** (the `deploy.yml` workflow handles the rest). A `CNAME` file pointing to `blog.confined-space.com` is already committed — once the custom domain's DNS is added (a `CNAME` record pointing at `<org>.github.io`), enable it under **Settings → Pages → Custom domain** and check **Enforce HTTPS**.
