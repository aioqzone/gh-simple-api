# gh-simple-api

Implement [PEP 503][pep-0503] simple api with GitHub Page.

## Setup

1. You should **create your own repository** and choose this repo as the template. Select **include all branches**, so that you'll get `master`, `idx-pages` and `gh-pages` branch.
2. In repo's GitHub Page settings, switch GitHub Page source to `GitHub Action`.
3. You can customize your site by editing `gh-pages` branch just like other `JekyII` GitHub Pages.
4. Once you edit `idx-pages`(by calling the following workflows) or edit `gh-pages`, our build workflow will fetch built index htmls to `/simple` dir, then build your site from `gh-pages` source.

## Usage

### release.yml

`release.yml` is a helper workflow which will upload the given files to GitHub release of the given repository and return urls **including a `SHA256` hash in the url fragments**.

<details>

<summary>Inputs, Outputs and Example</summary><br>

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
    files: dist/* # use `dist` here is ok as well
    repo: aioqzone/project1
- run: echo ${{ steps.upload.outputs.urls }}
# https://github.com/aioqzone/project1/releases/download/0.1.0/package1-0.1.0-cp3-none.whl#sha256=111111 https://github.com/aioqzone/project1/releases/download/0.1.0/package1-0.1.0.tar.gz#sha256=2222222
```

</details>

### add.yml

`add.yml` will add urls and the corresponding files to the given project and deploy the index to GitHub Page.

<details>

<summary>Inputs, Outputs and Example</summary><br>

- Inputs:
  - project: Which project to add or update.
  - urls: Assets urls. Should include a hash in url fragment. See [PEP 503][pep-0503].
  - repo: Index hosting repository, default as caller repository.
  - index-branch: Your GitHub Page branch, default as `idx-pages`.

``` yaml
- uses: aioqzone/gh-simple-api/.github/workflows/add.yml@master
  with:
    project: project1
    urls: https://example.com/package1-0.1.0-cp3-none.whl#sha256=111111 https://example.com/package1-0.1.0.tar.gz#sha256=2222222
    repo: aioqzone/aioqzone-index
```

</details>

### remove.yml

`remove.yml` will remove the files in the given project and deploy the index to GitHub Page.

> **Note** This workflow should be triggered manually.

<details>

<summary>Inputs, Outputs and Example</summary><br>

- Inputs:
  - project: Which project to remove files from.
  - files: The files to remove.
  - index-branch: Your GitHub Page branch, default as `idx-pages`.

``` yaml
project: project1
files: package1-0.1.0-cp3-none.whl package1-0.1.0.tar.gz
```

</details>

### add_wohash.yml

`add_wohash.yml` is a wrapper of `add.yml`. It allows you to add asset urls without hash fragment included. This workflow will fetch the assets and calculate their `sha256`.

<details>

<summary>Inputs, Outputs and Example</summary><br>

- Inputs:
  - project: Which project to add or update.
  - urls: Assets urls. Needn't include a hash fragment (but is also allowed).
  - index-branch: Your GitHub Page branch, default as `idx-pages`.

``` yaml
project: project1
urls: https://example.com/package1-0.1.0-cp3-none.whl https://example.com/package1-0.1.0.tar.gz
```

</details>

[pep-0503]: https://peps.python.org/pep-0503/
