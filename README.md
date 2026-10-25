"# gh-deployment-workflow" 
# GitHub Pages Deployment Workflow

![Deploy](https://github.com/agyesam12/gh-deployment-workflow/actions/workflows/deploy.yml/badge.svg)

A simple CI/CD project: a GitHub Actions workflow that automatically deploys a static website to GitHub Pages whenever `index.html` changes.

- **Live site:** https://agyesam12.github.io/gh-deployment-workflow/
- **Project page:** https://roadmap.sh/projects/github-actions-deployment-workflow

## How it works

```
push to main (index.html changed) → GitHub Actions → build job → deploy job → live on GitHub Pages
```

The workflow lives in [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml).

| Part | What it does |
|---|---|
| `on: push: paths: index.html` | Runs **only** when `index.html` changes on `main`. Editing the README doesn't trigger a deploy. |
| `workflow_dispatch` | Adds a **Run workflow** button in the Actions tab for manual deploys |
| `permissions` | Least privilege: read the code, write to Pages, and get an OIDC token, so no secrets need to be stored |
| `concurrency` | Stops two deploys from running at once if you push twice quickly |
| **build** job | Copies `index.html` into a `_site` folder and uploads it as a Pages artifact |
| **deploy** job | Runs after `build` (`needs: build`) and publishes the artifact to GitHub Pages |

## Project structure

```
.
├── index.html                    # the website
├── README.md
└── .github/
    └── workflows/
        └── deploy.yml            # the CI/CD workflow
```

## Set it up yourself

1. Fork or clone this repository.
2. Go to **Settings → Pages** and under **Build and deployment → Source**, choose **GitHub Actions**.
3. Edit `index.html`, then commit and push to `main`:
   ```bash
   git add index.html
   git commit -m "Update website"
   git push
   ```
4. Open the **Actions** tab to watch the workflow run. When it finishes, your site is live at `https://<your-username>.github.io/<repo-name>/`.

## What I learned

- Writing GitHub Actions workflows: triggers, path filters, jobs, steps and `needs`
- Continuous deployment: every change to the site goes live automatically
- Least-privilege permissions and OIDC-based deployment with no stored secrets
- Using `concurrency` to keep deployments safe