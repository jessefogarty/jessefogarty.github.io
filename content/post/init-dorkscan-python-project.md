+++
author = "unkwn1"
title = "DorkScan - Automating Dorked Search Queries"
date = "2022-01-01"
description = "DorkScan automates the dorked query and scraping of search engine result pages to find links that include query parameters."
tags = [
    "python",
    "gitlab",
    "google dorking",
    "web scraping",
]
categories = [
    "Projects",
    "InfoSec",
    "Coding",
]
+++

# DorkScan - Automate Scraping Dorked Search Queries

[DorkScan](https://gitlab.com/unkwn1/dorkscan), a project I've been working on for the past couple of weeks after contributing to a friends (original) project. DorkScan automates the dorked query and scraping of search engine result pages to find links that include query parameters. Below I'll go over the module and some interesting things I learned along the way.

## DorkScan()
DorkScan boils down to an importable function. Under the hood there's the main function and supporting modules; engines and network.

The main function DorkScan() takes the required / optional arguments - doing some basic sanity checks. Then it begins iterating through pages of result pages. 

Within page iteration, an instance of ThreadPoolExecutor() is started.

Within ThreadPoolExecutor loop dthe list of dorks is iterated over submitting a fetch() call for each dork.

```python
def DorkScan(
    dorks: list[str],
    search_engine: str="BING",
    pages: int=1,
    console: Console=Console()
    ) -> list[ScanResults]

# Example Usage
from dorkscan import DorkScan
results = DorkScan(
    search_engine: str="BING",
    pages: int=1,
    dorks: list = ["inurl:.php?", "inurl:admin.php"]
    )
print(results)
```

To view a live demonstration of DorkScan run it as a python module.

```bash
$pip install dorkscan
$python3 -m dorkscan
```

## python TIL's

### Rich

While dorkscan doesn't have a CLI. It does however have offer a demonstration / help page via running the module itself `python3 -m dorkscan`. I used Rich to handle the console output and tbh its the part of the project I'm most proud of.
(Insert screen recording)

### Packaging

This is the second python project I've packaged and distributed, both through PyPI. Using `Twine`, I made sure to upload to the test PyPI repository. Uploading packages to multiple repositories was so much easier than I had expected. Using a `.pypirc` makes it as simple as switching your `-r` argument.

```config
[distutils]
index-servers =
    testpypi
    pypi

[pypi]
username = unkwn1

[testpypi]
username = unkwn1
```

```sh
$twine upload -r testpypi dist/*
```

#### Testing / Versioning

Also, I need to find a way to support lower versions of python. Currently dorkscan is confined to python 3.10 as I've utilized `match`/`case`.

I haven't got around to using Tox for testing.

### Naming

Even though DorkScan boils down to a single function for public use I wanted to have a demo that was CLI-like. All of my python projects end up with import statements like `from dorkscan.scan import DorkScan`. It turns out putting things into those pesky `__init__.py` and `__main__.py` files really  help.

DorkScan resides in the init file and the function called when running the demo resides in main. Importing the function is now cleaner imo as `from dorkscan import DorkScan` and the CLI demo is `python3 -m dorkscan`.
