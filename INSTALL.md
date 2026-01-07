# Installing and Deploying

## Deployment to GitHub Pages

This site is deployed via GitHub Pages using the `deploy.yml` workflow.

### Setup Steps

1. In the repository, go to **Settings → Actions → General → Workflow permissions** and give `Read and write permissions` to GitHub Actions.

2. The `_config.yml` should have `url` set to `https://mingyima.github.io` and `baseurl` left empty.

3. Push changes to the `main` branch. This will automatically trigger the **Deploy site** action.

4. Wait for the GitHub Action to complete (~4 min). Check the **Actions** tab for progress. Once successful, a `gh-pages` branch will be created.

5. Go to **Settings → Pages → Build and deployment**:
   - Set **Source** to `Deploy from a branch`
   - Set **Branch** to `gh-pages` (NOT `main`)

6. Wait for the `pages-build-deployment` action to finish (~45s), then visit https://mingyima.github.io

### Manual Deployment

To manually re-deploy, go to **Actions**, click "Deploy" in the left sidebar, then "Run workflow."

## Local Development (Optional)

If you want to run the site locally, you'll need Ruby and Bundler installed:

```bash
# Install dependencies
bundle install

# Install Jupyter for notebook support (optional)
pip install jupyter

# Run local server
bundle exec jekyll serve
```

Then open http://localhost:4000 in your browser.

**Tip:** For managing Ruby versions, consider using [rbenv](https://github.com/rbenv/rbenv).
