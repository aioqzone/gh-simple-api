# gh-simple-api

Implement [PEP 503][pep-0503] simple api with GitHub release and GitHub Page.

## Setup

1. You should create your own repository and choose this repo as the template.
2. Setup GitHub Page in your repo settings. Feel free to choose a theme as your landing page.

## Usage

### release.yml

`release.yml` will upload the given files to GitHub release of the given repository and return urls
including a `SHA256` hash in their url fragments.

- Inputs:
  - tag: Release tag.
  - files: Assets files. To define a display label for an asset, append text starting with '#' after the file name.
  - repo: Release hosting repository, default as caller repository.

- Outputs:
  - urls: the urls of the uploaded files, with sha256 as url fragment.

``` yaml
- uses: aioqzone/gh-simple-api/.github/workflows/release.yml@master
  id: upload
  with:
    tag: 0.1.0
    files: dist/*
    repo: aioqzone/project1
- run: echo ${{ steps.upload.outputs.urls }}
# https://github.com/aioqzone/project1/releases/download/project1/package1-0.1.0-cp3-none.whl#sha256=111111 https://github.com/aioqzone/project1/releases/download/project1/package1-0.1.0.tar.gz#sha256=2222222
```

### add.yml

`add.yml` will add urls and the corresponding files to the given project and deploy the index to GitHub Page.

- Inputs:
  - project: Which project to add or update.
  - urls: Assets urls. Should include a hash in url fragment. See [PEP 503][pep-0503].
  - repo: Index hosting repository, default as caller repository.
  - index-branch: Your GitHub Page branch, default as `gh-page`.

``` yaml
- uses: aioqzone/gh-simple-api/.github/workflows/add.yml@master
  with:
    project: project1
    urls: https://example.com/package1-0.1.0-cp3-none.whl#sha256=111111 https://example.com/package1-0.1.0.tar.gz#sha256=2222222
    repo: aioqzone/aioqzone-simple-index
```

### remove.yml

`remove.yml` will remove the files in the given project and deploy the index to GitHub Page.

> **Note** This workflow should be triggered manually.

- Inputs:
  - project: Which project to remove files from.
  - files: The files to remove.
  - repo: Index hosting repository, default as caller repository.
  - index-branch: Your GitHub Page branch, default as `gh-page`.

``` yaml
- uses: aioqzone/gh-simple-api/.github/workflows/remove.yml@master
  with:
    project: project1
    files: package1-0.1.0-cp3-none.whl package1-0.1.0.tar.gz
    repo: aioqzone/aioqzone-simple-index
```


[pep-0503]: https://peps.python.org/pep-0503/
