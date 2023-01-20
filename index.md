# Welcome to gh-simple-api !

This is the welcome page of [gh-simple-api][repo]. <!-- change this link -->

To access simple api, goto [simple](./simple).
For further information, goto our [repository][repo]. <!-- do not change this link -->

## Using simple index

### Poetry

Add this section into `pyproject.toml`:

```toml
[[tool.poetry.source]]
# a name for this source
name = "aioqzone-index"
# your simple api url
url = "https://aioqzone.github.io/aioqzone-simple-index/simple/"
default = false
secondary = true
```

Reference: https://python-poetry.org/docs/repositories/#simple-api-repository


[repo]: https://github.com/aioqzone/gh-simple-api
