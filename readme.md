# Use of uv

## install new python versions using uv

```bash
uv python install 3.11
```

## Initialise UV project
```bash
uv init
```

## Create virtual env
```bash
uv venv
```

## install some specific version of environment
```bash
uv venv --python 3.11 .venv
source .venv/bin/activate
```

source .venv/bin/activate

## Install all the libraries from requirememnts.txt

```bash
uv add -r requirements.txt
```
## Install ipykernel for work with jupyter notebook

```bash
uv add ipykernel
```
