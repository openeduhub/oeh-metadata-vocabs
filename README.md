# OpenEduHub Vocabulary

![Build gh-pages](https://github.com/openeduhub/oeh-metadata-vocabs/workflows/Build%20/public%20and%20delpoy%20to%20gh-pages%20with%20docker%20container/badge.svg)

This is a repository for vocabulary used in the OpenEduHub-Project.

Every time a push is made to the repository a GitHub-workflow-action is triggered to publish the most recent vocabulary to the `gh-pages`-branch, which is used by GitHub pages. It spins up a Docker-Container made out the [SkoHub-Vocabs](https://github.com/hbz/skohub-vocabs)-tool. You can have a look at the Dockerfile [at this fork of skohub-vocabs](https://github.com/sroertgen/skohub-vocabs/tree/docker).

## Reuse

If you want to reuse this repo and have your vocabulary automatically pushed und published via GitHub-Pages, follow these steps:

1. Fork this repo
2. go to the `.github/workflows/main.yml`-file, delete everything and paste in the following:

```yaml
name: Build /public and delpoy to gh-pages with docker container

on:
  push:
    branches:
      - master
      - main
      - gh-pages
  workflow_dispatch:
    inputs:
      logLevel:
        description: 'Log level'
        required: true
        default: 'warning'
      tags:
        description: 'Test scenario tags'

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout 🛎️
        uses: actions/checkout@v2 # If you're using actions/checkout@v2 you must set persist-credentials to false in most cases for the deployment to work correctly.
        with:
          persist-credentials: false

      - name: remove public and data-dir if already exists
        run: rm -rf public data

      - run: mkdir public

      - run: mkdir data

      - run: git clone https://github.com/openeduhub/oeh-metadata-vocabs-playground.git data/

      - name: make .env.production file
        run: echo "PATH_PREFIX=oeh-metadata-vocabs-playground/" > .env.production

      - name: build public dir with docker image
        run: docker run -v $(pwd)/public:/app/public -v $(pwd)/data:/app/data -v $(pwd)/.env.production:/app/.env.production laocoon667/openeduhub-skohub-vocabs:docker

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

3. make sure to replace the following line:
  - `- run: git clone https://github.com/openeduhub/oeh-metadata-vocabs-playground.git data/` <- adjust the path to point to **YOUR** repository
  - `run: echo "PATH_PREFIX=oeh-metadata-vocabs-playground/" > .env.production` <- the `PATH_PREFIX` has to be set to **YOUR** repository name
4. in your repository settings go to the "GitHub Pages" setting and select `gh-pages` as the branch your site is being built from. If it is not available yet, you might have to push something to your repo, so the GitHub-Action gets triggered or you can trigger it manually with going to "Actions" in the menubar, then select the workflow "Build /public and deploy..." and click "Run workflow". This way you can trigger the workflow automatically.
5. after that your vocabulary will be automatically published every time a push to this repo is made.
