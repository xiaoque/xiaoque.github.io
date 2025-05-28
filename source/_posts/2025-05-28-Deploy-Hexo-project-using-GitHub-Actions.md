---
title: Deploy Hexo project using GitHub Actions
toc: true
category: Hexo
tags: 
  - Hexo
date: 2025-05-28 11:29:54
---




**Hexo & GitHub Actions - "No artifacts named 'github-pages'" Error**

After migrating to Hexo, I tried setting up GitHub Pages deployment via **GitHub Actions**, following the official guide [GitHub Pages](https://hexo.io/docs/github-pages). Unfortunately, my workflow consistently failed with the error:

 `No artifacts named "github-pages" were found for this workflow run.`  



**Solution:**

My guess is there could be a potential incompatibility or version mismatch between the `actions/upload-pages-artifact` v3 and `actions/deploy-pages` v4.

To address this, just to explicitly ensure the Pages configuration was up-to-date *before* the deployment step. 

```yaml
- name: Configure GitHub Pages
  uses: actions/configure-pages@v4
```



**My Complete GitHub Actions Workflow:**

Here's the GitHub Actions workflow file (`.github/workflows/your-workflow-name.yml`) that now works:

```yaml
name: GH Pages Deploy

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: write
      pages: write
      id-token: write
    concurrency:
      group: ${{ github.workflow }}-${{ github.ref }}
    steps:
      - uses: actions/checkout@v4
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
      - name: Use Node.js 20
        uses: actions/setup-node@v4
        with:
          node-version: "20"
      - name: Cache NPM dependencies
        uses: actions/cache@v4
        with:
          path: ~/.npm
          key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-node-
      - name: Install Dependencies
        run: npm install    
      - name: Build
        run: npm run build
      - name: Upload Pages artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public  
  deploy:
    needs: build
    permissions:
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
    - name: Configure Pages
      uses: actions/configure-pages@v4
    - name: Deploy to GitHub Pages
      id: deployment
      uses: actions/deploy-pages@v4
```



Hopefully, this helps anyone else struggling with the same GitHub Actions deployment error for their static sites!

