# cs-notes

## About

This is a repo with notes for Computer Science topics

## Tools Used

- **Obsidian**: Notes are taken using Obsidian for easy markdown, images and code snippets insertion
- **mkdocs**: Use to deploy markdown files to github pages.

## Steps

- Clone this repo
- Install Obsidian.
- Turn Off Wiki Links (for MkDocs). Settings → Editor. Disable “Use [[WikiLinks]]”
- Settings → Files & Links. Set “Default location for new attachments” to: ```attachments```
- Install python if not already installed ```brew install python```
- Then using command ```pip3 install mkdocs mkdocs-material```
- cd to this repo and run this command ```mkdocs new .```
- Now update ```mkdocs.yml``` to this (Update nav as you add more files.):

```yml
site_name: cs-notes
site_url: https://hrs070.github.io/cs-notes/

theme:
  name: material
  palette:
    scheme: slate  # Dark mode

docs_dir: docs
use_directory_urls: true

nav:
  - Home: index.md
  - Tricks: tricks.md
  - Principles:
      - Solid: principles/solid.md
```

- Add commits and push to github
- run ```mkdocs gh-deploy```
