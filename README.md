# pants

**pants (pandoc-使い)** is a minimal site generator powered by [pandoc](http://pandoc.org/).

It is written in 22 lines of Bash, because any other fancy site generators more than 22 lines are for suckers. Suppose:

* You have **a folder of markdown files**, whatever the directory structure is.
* You have **an HTML template**.
* You want to turn them into **a simple website**, following the same structure.

Here's how you use pants:

* Put on the `pants` script somewhere.
* Create a `src` folder containing some markdown files.
* Run `./pants`.
* If you want to understand how it works, just take a look at `./pants`.
* If you want to configure, just modify your `./pants`.

Here's why you may want to use pants:

* Because `pandoc` is the best universal document converter and you want to use it to build your blog.
* Because you don't need a ridiculous static site generator which you can't read through the code, for such a simple task of building a simple website.

Here's why you may not want to use pants:

* You think it is too simple sometime naïve. (yes it is, because it has only 22 lines!)
* You don't like the name.
* You hate the "yet another static site generator" thing.

...or whatever.

---

For your information, you might have imagined that pants (パンツ) look like this:

![](https://i.imgur.com/2ORotDP.jpg)

But actually, they are like this:

![](https://i.imgur.com/2ORotDP.jpg)

---

## FAQ

Q: How do I configure this?

A: Modify your `pants`.

Q: Can I have that feature?

A: Extend your `pants`.

Q; Can I ...?

A: Write your own `pants`.

Q: I think `pants` sucks!

A: But "pants" is a plural form. You should really say "pants suck".

Q: Are you serious?

A: Yes.

Q: Seriously, are you serious?

A: No.
