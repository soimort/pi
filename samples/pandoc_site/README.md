---
title: README
author: Mort Yao

---

## How

[String](https://github.com/soimort/string) is embedded in `Makefile` already, together with all the necessary configuration. Build the site with *pandoc*:

    $ make

You'll find everything the web server needs in `_site/`. Also, you can preview the site using a temporary server: (Ruby required)

    $ make watch

## What

Explanation on the directory structure: (names used here are specific to this demo site and its `Makefile` configuration; you _don't have to_ do naming this way)

* `README.md`
    * Markdown source files (`*.md`) can be put anywhere in any directory (except for a directory whose name starts with an underscore "`_`", which is ignored by the generator)
    * A Markdown file always translates to its corresponding path on the server, e.g., `README.md` -> `/README.html`
    * A Markdown file may contain a **YAML front matter block**, e.g., the front matter block of the Markdown file that generates this page:

```yaml
        ---
        title: README
        author: Mort Yao
        ---
```

* `index.md`
    * `index.md` translates to `/index.html`, which is the default page served by many web servers (as a convention)
* `stories/`
    * `black-cat.md`
        * As explained before: `black-cat.md` -> `/stories/black-cat.html`
    * `tell-tale-heart.md`
        * As explained before: `tell-tale-heart.md` -> `/stories/tell-tale-heart.html`
* `_templates/`
    * `main.html`
        * The HTML template for *pandoc* to use
        * Variables in the template (e.g., `$title$` and `$author$`) are replaced by corresponding metadata in the YAML front matter of the Markdown file from which a page is generated
* `css/`
* `images/`
    * Any non-Markdown file in any directory (except for a directory whose name starts with an underscore "`_`", which is ignored by the generator) is copied directly into the output directory
* `_site/`
    * The output directory, which can be served as static site

## Why

Because fancy static site generators never help you write a good blog. Stop toying with them. Focus on writing. Be the master of your own content.
