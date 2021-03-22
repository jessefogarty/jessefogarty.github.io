+++
author = "Jesse"
title = "Pip Modules Names Only Using sed"
date = "2021-03-22"
description = "Detailing a sed command I utilize in order to get pip package names only from pip freeze"
tags = [
    "Linux",
    "Python",
    "sed",
]
categories = [
    "Python",
]
+++
# Getting Pip Package Names Using sed & pip freeze

Upgrading pip packages can be a pain. More so when you're not using virtual environments but, even with them it's still annoying. There are some packages out there to help such as `pip-review`. I've had issues with it and accordingly have been updating manually.

### pip freeze + sed
```bash
pip freeze | sed "s/==.*$//g" > pip_pkgs.txt
```
To explain what's going on, I'll break down the above into a couple parts.
- `pip freeze` - will generate a list of installed pip packages in the format `<package>==<version>`
- we when pipe the pip freeze output to `sed`. `"s/==.*$//g"` is used to strip the == and version numbers. We then save the output to a file `pip_pkgs.txt`

I plan on the near future working this into a python script. Here's some of the functionality I've been contemplating.
- [ ] interactive Y/N upgrade loop
- [ ] track version before and after upgrade for easy rollbacks
- [ ] *maybe* an update checker. The annoyance is pypi blocking search queries
