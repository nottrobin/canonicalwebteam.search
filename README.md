# canonicalwebteam.search

Flask extension to provide a search view for querying the webteam's Google Custom Search account.

## Installation

`pip3 install canonicalwebteam.search`

Or add `canonicalwebteam.search` to your `requirements.txt`.

## Usage

### Application code

You can add the extension on your project's application as follows:

```
from canonicalwebteam.search import build_search_view

app = Flask("myapp")

app.add_url_rule("/search", "search", build_search_view())

# Or, a bit more complex example

app.add_url_rule(
    "/docs/search",
    "docs-search",
    build_search_view(
        site="maas.io/docs",
        template_path="docs/search.html"
    )
)
```

### The template

You need to create an HTML template at the specificed `template_path`. By default this will be `search.html` inside your templates folder. This template will be passed the following data:

- `{{ query }}` - the contents of the `q=` search query parameter
- `{{ start }}` - the contents of the `start=` query parameter - the offset at which to start returning results (used for pagination - default 0)
- `{{ num }}` - the contents of the `num=` query parameter - the number of search results to return  (default 10)
- `{{ results }}` - the results returned from the Google Custom Search query. The actual search results are in `{{ results.entries }}`.

### The API key

You then need to provide the API key for the Google Custom Search API  as an environment variable called `SEARCH_API_KEY` when the server starts - e.g.:

```
SEARCH_API_KEY=xxxxx FLASK_APP=app.py flask run
```

Once this is done, you should be able to visit `/search?q={some_query}` in your site and see search results built with your `search.html` template.
