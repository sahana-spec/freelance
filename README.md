
```yaml
name: Deploy StudentSphere

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 22
          cache: npm

      - name: Install dependencies
        run: npm install

      - name: Build website
        run: npm run build -- --base=/${{ github.event.repository.name }}/

      - name: Upload website
        uses: actions/upload-pages-artifact@v3
        with:
          path: dist

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4

Commit the new file to the `main` branch.

## Step 4: Enable GitHub Pages

1. Open your repository's **Settings**.
2. Select **Pages** in the sidebar.
3. Under **Build and deployment**, set **Source** to **GitHub Actions**.

## Step 5: Get your live website link

1. Open the **Actions** tab in your repository.
2. Click the **Deploy StudentSphere** workflow.
3. Wait for both jobs to finish successfully.
4. Open **Settings → Pages** to find your published website.

Your website address will look like:

`https://YOUR-USERNAME.github.io/StudentSphere/`

Replace `YOUR-USERNAME` with your GitHub username.

**Bro, let's do it one step at a time.** Have you already created the `StudentSphere` repository on GitHub, or are you currently on the GitHub homepage?
```
