# Support Page Deployment Guide (GitHub Pages)

This directory contains the support page and privacy policy for the iOS application.
To make these pages accessible via a URL (e.g., for App Store submission), you can host them using GitHub Pages.

## Deployment Options

### Option 1: Main Branch Deployment (Simplest)
If your repository's GitHub Pages settings are configured to serve from the root of the `main` branch:

1. Push this `websupport` folder to your repository.
2. Your support URL will be:
   `https://<your-username>.github.io/<repo-name>/rishu_node1_tx/websupport/index.html`
3. Your privacy policy URL will be:
   `https://<your-username>.github.io/<repo-name>/rishu_node1_tx/websupport/privacy.html`

### Option 2: Dedicated Branch (Recommended for Clean URLs)
To have a cleaner URL like `https://<your-username>.github.io/<repo-name>/`, you can deploy just this folder to the `gh-pages` branch.

#### Using GitHub Actions (Automated)
1. Create a file `.github/workflows/deploy-support.yml` in your repository root.
2. Add the following content:

```yaml
name: Deploy Support Page

on:
  push:
    branches:
      - main
    paths:
      - 'rishu_node1_tx/websupport/**'

permissions:
  contents: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./rishu_node1_tx/websupport
```

3. Go to repository **Settings > Pages**.
4. Under **Build and deployment**, select **Source: Deploy from a branch**.
5. Select **Branch: gh-pages** and folder **/(root)**.
6. Your URL will be: `https://<your-username>.github.io/<repo-name>/`

## File Structure
- `index.html`: The main support page with contact info and FAQ.
- `privacy.html`: The privacy policy page.

## Customization
- Edit `index.html` to update FAQs and contact email.
- Edit `privacy.html` to update privacy terms.
- Both files use the Macaron color scheme defined in the project.
