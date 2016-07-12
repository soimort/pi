# Pancake

**[Pancake](https://github.com/soimort/soimort/tree/pancake)** is a static site generator powered by [pandoc](http://pandoc.org/), as well as a server for live previewing of Markdown documents.

## Why?

I write a lot of Markdown, structured in files and directories. I need to:

1. Preview Markdown documents *instantly*.
2. Generate static websites as well as *high-quality PDFs* from them, with least effort.
3. Generate *on demand*, i.e., be able to generate a single post / file without rebuilding the whole site.
4. Use *convention* over configuration; use easy-to-read *YAML* for configuration, not yet-another-DSL recipes; use simple *string interpolations*, instead of some non-trivial (or even Turing-complete) templating language.
5. Be able to write my *own filters* to post-process generated documents and to tweak them.

This tool does all of the above.

(Actually, most of the static site generators I know, can do only one or none of them. That's why I'm creating this tool.)

## Prerequisites

* GHC
    * pandoc >= 1.15
* Ruby >= 2.0
    * Gems: [listen](https://rubygems.org/gems/listen/) >= 3.0

## Examples

A subdirectory or file under the current path is called a "***real target***" and can be built alone by `pancake`:

    $ pancake posts/salve-munde

By default, `pancake` will look into the current path "`.`", "`posts/`" and "`posts/salve-munde/`" and find all relevant "configuration paths" (all files whose name starts with an underscore "`_`", and all files under directories whose name starts with an underscore). Among all these "configuration paths", `pancake` is interested in two types of files: metadata files (`.yaml` or `.yml`, e.g., `_config.yaml` and `posts/_config/meta.yaml`) and source files (`.markdown` or `.md`, e.g., `posts/_config/1_footer.md` and `posts/_config/2_references.md`).

In case you don't want `pancake` to search for configuration paths, use the ***control option*** `/no-preload`, e.g., `pancake /no-preload posts/salve-munde`. To specify a configuration file explicitly, use something like `/meta=filename.yaml`.

All metadata files are passed straight to `pandoc`. Moreover, there are some metadata fields used by `pancake` specifically: (**`target`**, **`source`**, **`source-metadata`**, **`source-bibliography`**)

```yaml
target: index.html
source: source.md
source-metadata: source.yaml
source-bibliography: source.bib
```

With these fields above, `pancake` will look for `source.md`, `source.yaml` under the target path `posts/salve-munde` and pass them to `pandoc`, pass `posts/salve-munde/source.bib` as a `--bibliography` option to `pandoc`, and pass `posts/salve-munde/index.html` as an `--output` option to `pandoc`.

*Note*: A ***real target*** may also be a file other than directory; in that case, the **`source`** field is not used since the target filename speaks for itself.

Furthermore, the target path as a string `posts/salve-munde` is passed to `pandoc` as an extra metadata field `id`. So the resulting `pandoc` command will be like:

```shell
$ pandoc --bibliography posts/salve-munde/source.bib \
    --output posts/salve-munde/index.html            \
    -M id=posts/salve-munde                          \
    posts/salve-munde/source.md                      \
    posts/_config/1_footer.md                        \
    posts/_config/2_references.md                    \
    _config.yaml                                     \
    posts/_config/meta.yaml                          \
    posts/salve-munde/source.yaml
```

Some more metadata fields recognized by `pancake`:

```yaml
input-format: markdown       # passed to pandoc as option -f
output-format: html5         # passed to pandoc as option -t
template: templates/default.html5 # passed to pandoc as option --template
mathjax: https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML
# passed to pandoc as option --mathjax
filters: pandoc-citeproc     # passed to pandoc as --filter. can be an array
raw-options: --section-divs  # passed to pandoc directly. can be an array
```

To pass other options to `pandoc`, an alternative way other than specifying the **`raw-options`** field in a metadata file is to use ***extra option*** targets in command line. Anything starts with a hyphen "`-`" is regarded as an extra option to be passed to `pandoc`. For example:

    $ pancake --toc --section-divs posts/salve-munde

*Note*: If an option has a value, the option and the value must appear as a single parameter. For example, `pancake --output README.html README.md` will not work properly, since it treats `README.html` as a ***real target*** other than the value of extra option `--output`. The correct way is to use:

    $ pancake --output=README.html README.md

Besides ***real targets***, ***control options*** and ***extra options***, there are also a few predefined ***phony targets***: (like what you may have in a Makefile)

    $ pancake all

The phony target **`all`** looks for all source files recursively under the current directory, but not under a directory whose name starts with an underscore (which is considered to be a "configuration path"). `pancake` then calls `pandoc` on these source files in alphabetical order, with their relevant metadata respectively.

    $ pancake server

The phony target **`server`** starts a local server to serve the current directory and watch for changes. You may also specify a port to use:

    $ pancake server:port=4000

It is also possible to specify a directory to watch; `pancake` will automatically regenerate all targets every time a file is modified in that directory:

    $ pancake posts/salve-munde server:port=4000,watch=posts/salve-munde

*Note*: Changes of `.html` and `.htm` files will NOT trigger the regenerating, otherwise `pandoc` will be called infinitely (since every time it changes the `.html` file it generates).

*Tips*: `pancake` can serve as a markdown previewer:

    $ pancake -s --output=README.html README.md server

Moreover, you may easily specify a customized template to use via `--template`, or include your own stylesheets as a `css` field in `--metadata`:

    $ pancake --metadata=css:/css/minimal.css -s --output=README.html README.md server

Even better, if you have an HTML template that reads [server-sent events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events), you can tell `pancake` to stream server-sent events via the phony target **`preview`**, so the updates can take effect instantly without having to refresh the browser:

    $ pancake --template=template.html --output=README.html README.md preview
