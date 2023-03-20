# Blog
This repo contains the source code and the markdown-based content files of my blog website.


## Where is the blog hosted?

The blog can be found at [blog.fgebhart.dev](https://blog.fgebhart.dev/)


## Usage

The blog is based on [mkdocs-material](https://squidfunk.github.io/mkdocs-material/). To render the blog locally
run the following commands in the root of this repo:

```
pip install poetry

poetry install

poetry shell

mkdocs serve
```

Go to http://localhost:8000/

Note, images from imgur will not be loaded from the local server when
using `127.0.0.1:8000/` ensure to use `http://localhost:8000/` instead.

Note, if you want the social cards to be generated locally, you need to set the following environment variable:
```
export CARDS=true
```
