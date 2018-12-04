# pywikiapi: A Tiny Python MediaWiki API Lib

This is a minimalistic library that handles some of the core MediaWiki API complexities like handling continuations, login, errors, and warnings, but does not impose any additional abstraction layers, allowing you to use every single feature of the MW API directly in the most optimal way. 

The library was written by the original author of the MediaWiki API itself, and tries to address some of the mistakes of the original API design... Some things should have been done differently. :)

```python
from pywikiapi import wikipedia
# Connect to English Wikipedia
site = wikipedia('en')

# Iterate over all query results as they are returned
# from the server, handling continuations automatically.
# (pages whose title begins with "New York-New Jersey")
for r in site.query(list='allpages', apprefix='New York-New Jersey'):
  for page in r.allpages:
    print(page.title)

# Iterate over two pages, getting the page info and the list of links for each of the two pages. Each page will be yielded as a separate result.
for page in site.query_pages(titles=['Test', 'API'], prop=['links', 'info'], pllimit=10):
    print(page.title)
    print(', '.join([l.title for l in page.links]))
    print()

site.login(username, password)
site('edit', text=...)
```

## Installation

You can install the  from [PyPI](https://pypi.org/project/pywikiapi/):

    pip install pywikiapi

The library supports Python 2.7+ and Python 3.4+

## How to use

* Create a `Site` object, either directly or with the `wikipedia` helper function.
* Use `site.query_pages(...)` to   
* Use `site.query(...)` or `site.iterate(action, ...)` for all iteration-related API calls. The API will handle all the continuation logic internally.