# gh-simple-api

Implement [PEP 503][pep-0503] simple api with GitHub Page.

## Setup

1. You should **create your own repository** and choose this repo as the template. Select **include all branches**, so that you'll get `master`, `idx-pages` and `gh-pages` branch.
2. In repo's GitHub Page settings, switch GitHub Page source to `GitHub Action`.
3. You can customize your site by editing `gh-pages` branch just like other `JekyII` GitHub Pages.
4. Once you edit `idx-pages`(by dispatching the following workflows) or edit `gh-pages`, our build workflow will fetch built index htmls to `/simple` dir, then build your site from `gh-pages` source.

## Usage

### Action `hash-release`

`hash-release` will upload given files to GitHub Release. It outputs urls with `sha256` fragments.

<details>

<summary>Inputs, Outputs and Example</summary><br>

- Inputs:
  - tag: The release tag.
  - files: The files to upload.
  - repo: Upload to which repository, default as the caller repo.
- Outputs:
  - urls: urls point to uploaded files with hash attached to url fragments. Order is not guaranteed.

Upload files under `dist/` to Release of current repository:

```yaml
- uses: aioqzone/gh-simple-api/.github/actions/hash-release@master
  with:
    tag: 0.1.0
    files: dist/* # `dist` is not guaranteed to work!
```

</details>

---

### Workflow `add.yml`

`add.yml` will add urls and the corresponding files to the given project and deploy the index to GitHub Page.

> **Note** This workflow should be triggered by `workflow_dispatch`.

<details>

<summary>Inputs, Outputs and Example</summary><br>

- Inputs:
  - project: Which project to add or update.
  - urls: Assets urls. Should include a hash in url fragment. See [PEP 503][pep-0503].
  - repo: Index hosting repository, default as caller repository.
  - index-branch: Your GitHub Page branch, default as `idx-pages`.
  - republish: Whether to republish the pages after success.

Add two URLs to `project1` index hosted in `aioqzone/aioqzone-index@idx-pages`

``` yaml
project: project1
urls: https://example.com/package1-0.1.0-cp3-none.whl#sha256=111111 https://example.com/package1-0.1.0.tar.gz#sha256=2222222
```

</details>

---

### Workflow `remove.yml`

`remove.yml` will remove the files in the given project and deploy the index to GitHub Page.

> **Note** This workflow should be triggered by `workflow_dispatch`.

<details>

<summary>Inputs, Outputs and Example</summary><br>

- Inputs:
  - project: Which project to remove files from.
  - files: The files to remove.
  - index-branch: Your GitHub Page branch, default as `idx-pages`.
  - republish: Whether to republish the pages after success.

Remove two files from `project1` hosted in `idx-branch` of this repository.

``` yaml
project: project1
files: package1-0.1.0-cp3-none.whl package1-0.1.0.tar.gz
```

</details>


[pep-0503]: https://peps.python.org/pep-0503/
