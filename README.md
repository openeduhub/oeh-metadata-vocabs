# OpenEduHub Vocabulary

![Build gh-pages](https://github.com/openeduhub/oeh-metadata-vocabs/workflows/Build%20/public%20and%20delpoy%20to%20gh-pages%20with%20docker%20container/badge.svg)

This is a repository for vocabulary used in the OpenEduHub-Project.

Every time a push is made to the repository a GitHub-workflow-action is triggered to publish the most recent vocabulary to the `gh-pages`-branch, which is used by GitHub pages. It spins up a Docker-Container made out the [SkoHub-Vocabs](https://github.com/hbz/skohub-vocabs)-tool. You can have a look at the Dockerfile [at this fork of skohub-vocabs](https://github.com/sroertgen/skohub-vocabs/tree/docker).
