# podcast-generator

A GitHub Action that automatically generates an RSS/XML podcast feed from a YAML file and hosts it via GitHub Pages.

> Built as part of the [Practical GitHub Actions](https://github.com/marketplace/actions/podcast-generator-workflow-action) course, as part of the [Career Essentials in GitHub Professional Certificate](https://www.linkedin.com/learning/paths/career-essentials-in-github-professional-certificate).

## How it works

1. You maintain a `feed.yaml` file in your repo describing your podcast and its episodes
2. On push, the action spins up a Docker container (Python 3.12), runs `feed.py` to parse the YAML and generate a `podcast.xml` RSS feed, then commits and pushes the result back to your repo
3. GitHub Pages serves the XML file, giving you a publicly accessible podcast feed URL

## Usage

Add the following to your workflow file (e.g. `.github/workflows/generate-feed.yml`):

```yaml
name: Generate Podcast Feed

on:
  push:
    paths:
      - 'feed.yaml'

jobs:
  generate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: JakeFordy/podcast-generator@v1.0
        with:
          email: ${{ github.actor }}@localhost
          name: ${{ github.actor }}
```

## Inputs

| Input   | Description                  | Required | Default                          |
|---------|------------------------------|----------|----------------------------------|
| `email` | Email address for git commit | Yes      | `${{ github.actor }}@localhost`  |
| `name`  | Name for git commit          | Yes      | `${{ github.actor }}`            |

## feed.yaml structure

Your `feed.yaml` should follow this structure:

```yaml
title: My Podcast
subtitle: A short subtitle
author: Your Name
description: Full description of the podcast.
image: /images/artwork.jpg
language: en-us
category: Technology
format: audio/mpeg
link: https://yourusername.github.io/your-repo

item:
  - title: EP01-Episode Title
    description: Episode description here.
    published: Thu, 12 Jan 2023 18:00:00 GMT
    file: /audio/ep01.mp3
    duration: 00:30:00
    length: 576,324
  - title: EP02-Another Episode
    description: Another episode description here.
    published: Thu, 19 Jan 2023 18:00:00 GMT
    file: /audio/ep02.mp3
    duration: 00:22:00
    length: 351,963
```

The `link` field is the base URL for your GitHub Pages site. File and image paths should be root-relative (starting with `/`).

## Output

The action generates a `podcast.xml` file in the root of your repository, compliant with the iTunes podcast RSS spec (`xmlns:itunes`). This can be submitted directly to podcast directories that accept RSS feed URLs.

## Stack

- Python 3.12
- PyYAML
- Docker
- GitHub Actions
- GitHub Pages

## License

MIT
