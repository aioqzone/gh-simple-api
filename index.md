# Welcome!

To access simple api, goto [simple](./simple).
For further information about `gh-simple-api`, goto our [repository][repo].

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

For more information, goto [poetry docs](https://python-poetry.org/docs/repositories/#simple-api-repository).

### Pip

...

[repo]: https://github.com/aioqzone/gh-simple-api
