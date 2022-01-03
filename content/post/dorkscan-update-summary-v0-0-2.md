+++
author = "unkwn1"
title = "DorkScan - v0.0.2 Update Notes"
date = "2022-01-02"
description = "Switched from bs4 to lxml. Engines class created. Default search engines moved to asset vs hard-coded."
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

# DorkScan v0.0.2

A day later and two merges later and I think DorkScan is shaping up to be a bit of code I'm quite happy with. These changes I'll be going over fix bugs, reduce code and, generally make the code more approachable.

## Switching HTML Parser

When I came into the project the previous author was using bs4. The parser seemed to be a underlying issue as the links being retrieved would sometimes be truncated. This was likely the result of a wrong selector. There's nothing wrong with bs4; however, using the aforementioned selector issue as an example I simply find it bulky and tend to use lxml whenever possible.

The main change here besides the library is that I now pull every link from the SERP source code. I think use an if statement with a few regex searches to filter the results.

**Previous Code**

```python
soup = bsoup(resp.text, "html.parser")
links = soup.findAll(engine["soup_tag"], engine["soup_class"])
# filter
[
link.text
for link in links
	if len(links) > 0
		and not re.search(blacklist, link.text)
			and re.search(r"\?.+\=", link.text)

```

**New Code**

```python
data = html.fromstring(resp).xpath("//a/@href")
links = [
	link
	for link in data
		if re.search(r"^http|s://", link)
			and re.search(r"\?.+\=", link)
				and not re.search(blacklist, link)
]

```

**Ignore the indenting it's to help illustrate the steps in the list comprehension*

## Making Engines More Modular

One thing that bugged me considerably was the hard-coded search engines dictionary. It confined the use to dictionary's already added. The problem with making things easier for people to add their own SearchEngine dictionaries was the changing names for query parameters.

I had to figure out was how to update a dictionary without knowing the keys - sometimes a search engine will use the key `first` to denote a page, others `b`. So, instead of using a predetermined key I utilize the order to make changes.

Here's an example below of how I use a dictionary's keys to update the values.

```python
engine["params"] = {
	# key will always be used as it's the existing key
	k: (dork if k == "q" else v * page) 
	for (k, v) in engine["params"].items()
   }

# Because I know the key 'q' exists I can use it to
# figure out where page should go. However, 
# I could do a type check 
# if it's a str I know it's q if it's an int it's the page value
```

This may lead to export / add dictionary functions etc.
