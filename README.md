# gh-simple-api

Implement [PEP 503][pep-0503] simple api with GitHub release and GitHub Page.

## Usage

### release.yml

`release.yml` will upload the given files to GitHub release of the given repository and return urls
including a `SHA256` hash in their url fragments.

- Inputs:
  - project: Which project to add or update.
  - files: Assets files. To define a display label for an asset, append text starting with '#' after the file name.
  - repo: Index hosting repository.

- Outputs:
  - urls: the urls of the uploaded files, with sha256 as url fragment.

``` yaml
- uses: aioqzone/gh-simple-api/.github/workflows/release.yml@master
  id: upload
  with:
    project: project1
    files: dist/*
    repo: aioqzone/aioqzone-simple-index
- run: echo ${{ steps.upload.outputs.urls }}
# https://github.com/aioqzone/aioqzone-simple-index/releases/download/project1/package1-0.1.0-cp3-none.whl#sha256=111111 https://github.com/aioqzone/aioqzone-simple-index/releases/download/project1/package1-0.1.0.tar.gz#sha256=2222222
```

### add.yml

`add.yml` will update the index hosted in the given repository and deploy it to GitHub Page.

- Inputs:
  - project: Which project to add or update.
  - urls: Assets urls. Should include a hash in url fragment. See [PEP 503][pep-0503].
  - repo: Index hosting repository.

``` yaml
- uses: aioqzone/gh-simple-api/.github/workflows/add.yml@master
  with:
    project: project1
    urls: https://example.com/package1-0.1.0-cp3-none.whl#sha256=111111 https://example.com/package1-0.1.0.tar.gz#sha256=2222222
    repo: aioqzone/aioqzone-simple-index
```


[pep-0503]: https://peps.python.org/pep-0503/
