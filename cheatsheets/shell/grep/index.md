---
title: Grep
layout: listing
---

## Man page

Linux: [ss64.com/bash/grep.html](https://ss64.com/bash/grep.html)

## Tutorials

- [Linux grep command](https://www.computerhope.com/unix/ugrep.htm) 🔗
- [Linux egrep command](https://www.computerhope.com/unix/uegrep.htm) 🔗
- [Grep command in Linux Unix](https://www.journaldev.com/24271/grep-command-in-linux-unix) 🔗


## Get line from file
<!-- TODO move to cookbook -->

Get bundler version from lock file, which has a title on one line and version on the next (and in this case always the last line of the file).

```
# Gemfile.lock

# ...
# ...
BUNDLED WITH:
  1.17.2
```


```sh
$ VSN="$(grep -A 1 "BUNDLED WITH" Gemfile.lock | tail -n 1)"
```

Notes:

- If the file does not exist, `grep` will show a warning on stderr. If you are existing on errors you can continue.
- If file does not contain the line, `grep` will exit on a success code still and the value will be an empty string.
- Use `-A 1` as an offset so we get the line after it. And then tail to keep it one line - don't know what happens if there's more content after it.

Use-case:

```sh
$ BUNDLE_VSN=gem install bundler \
-v "$(grep -A 1 '"BUNDLED WITH'" Gemfile.lock | tail -n 1)"
$ gem install bundler -v "${BUNDLE_VSN:-2.1.4}"
```

Copied from [source](https://bundler.io/blog/2019/05/14/solutions-for-cant-find-gem-bundler-with-executable-bundle.html).

Whitespace does not matter here but in other cases could be trimmed.
