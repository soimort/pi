# pi

A simple pandoc wrapper that does the trick.


## Motivation

**pi** is the successor of [pants](https://github.com/soimort/pi/blob/pants/pants) and [pancake](https://github.com/soimort/pi/blob/pancake/pancake) (and kind of a rewrite of the latter). The aim is to enable handy academic writing and blogging through the power of pandoc, without having to deal with the bloated TeX system or (yet-another) site generator.


## Basic usage

Preview any Markdown file:

    $ pi -p README.md

Arguments following the double-dash `--` are passed directly to `pandoc`:

    $ pi -p README.md -- -t latex -o output.pdf

Preview everything in the current path (recursively):

    $ pi -pr .

Configuration of page builds is done via metadata fields in specific YAML files, such as `_config.yaml`: (compatible with [pancake](https://github.com/soimort/pi/tree/pancake))

```yaml
---
template:            template/default.html
target:              index.html
---
```

Build everything in the current path (recursively) first, then start a server and watch for changes:

    $ pi -r .

Simply start a server and watch for changes (in the current path, by default):

    $ pi


## Dependencies

* Ruby (>= 2.2)
    * Gem: [Listen](https://github.com/guard/listen) (>= 3.0)
* [Pandoc](http://pandoc.org/) (>= 1.15)


## Command-line arguments

```
Usage: pi [options] [sources]

    -C, --converter [PROGRAM]        Set document converter (default: pandoc)
    -P, --port [PORT]                Set server port (default: 8000)
    -W, --watch [PATH]               Set path to watch for changes (default: ./)
        --latency [SECONDS]          Set delay between checking for changes (default: 0.25)
        --wait_for_delay [SECONDS]   Set delay between calls to the callback (default: 0.1)
    -r, --recursive                  Generate recursively
    -p, --preview                    Preview by path
        --dry                        Dry run
    -f, --feed                       Build feed
        --no-server                  Do not start server
        --no-watch                   Do not watch for changes
    -v, --[no-]verbose               Run verbosely
    -d, --debug                      Enable debug messages
    -V, --version                    Show program name and version
```
