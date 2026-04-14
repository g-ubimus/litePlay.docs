# litePlay.js docs
litePlay.js docs are build using [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).

Steps to build it:

1. Clone the repo:

```
git clone git@github.com:g-ubimus/litePlay.docs.git
```

2. Create a python virtual environment:

```
python -m venv venv
```

3. Activate the virtualenv:
 
```
source venv/bin/activate
```

4. Install material:

```
pip install -r requirements.txt
```

5. Build the docs:

```
mkdocs build
```

6. Access them with:

```
mkdocs serve 
```

7. Be sure to deactivate the environment when finished:
 
```
deactivate
```
