---
title: Rename
description: How to batch rename files using shell commands
---


## Notes

Be careful not use reserved variables like `PATH` otherwise things can break. Use `file`, `FILE`, `p` or `PATH`.

Be careful about globbing on hidden directories - as you can break `.git`.


## Change extension

The `find` command is probably more efficient and could let you run multiple commands, but using the `for` loop has more familiar syntax. There may be trade-offs for each when it comes to renaming (or moving) a directory, since it will affect future traversing steps in the same sequence. Using `find` might give more precision on the filename and type compared to using a `*` glob with `for` and having to use `if` statements in the loop to do checks. 

### Find command

Note that the `find` command will work recursively by default.

Optionally use the `-depth` command and in some examples below. Note from the docs:

```
-depth Process each directory's contents before the directory itself.  The -delete action also implies -depth.
```

Here is the general form to search and perform an action. The part at the end is hard to remember but is needed. Additionally add `-type f` for just files.

```sh
find PATH -name SEARCH -exec bash -c 'COMMAND' - '{}' \;
```

[source](https://stackoverflow.com/questions/21985492/recursively-change-file-extensions-in-bash)

Rename `.foo` to `.bar`. Note that the `*` glob is necessary otherwise you'll get no results.

```sh
find . -name "*.foo" -exec bash -c 'mv "$1" "${1%.foo}".bar' - '{}' \;
```

Using `git mv`:

```sh
find . -name "*.foo" -exec bash -c 'git mv "$1" "${1%.foo}".bar' - '{}' \;
```

Using `find` and the [rename](#rename-tool):

```sh
find . -name "*.foo" -exec rename 's/\.foo$/.bar/' '{}' \;
```

[source](https://askubuntu.com/questions/35922/how-do-i-change-extension-of-multiple-files-recursively-from-the-command-line)

Use `find` to change extension from `.foo` to `.bar`:

```sh
find . -depth -name "*.foo" -exec sh -c 'mv "$1" "${1%.foo}.bar"' - {} \;
```

Remove `-` and test with `echo`.

```sh
$ find . -name README.md -exec bash -c 'echo $0' '{}' \;
ci-cd/circle-ci/README.md
ci-cd/README.md
ci-cd/netlify/README.md
...
```

### For loop

Use [globstar][] to get recursive globs (`**`) - only Bash 4+ or ZSH.

```sh
for file in PATH/**/*.foo; do
  mv "$file" "${file%.foo}.bar"
done
```

[source](https://www.howtogeek.com/423214/how-to-use-the-rename-command-on-linux/)

Here we replace extension `.foo` with `.bar` using a `for` loop:

```sh
for file in *.foo; do mv -- "$file" "${file%.foo}.bar"
```

Replace space with underscore in certain file types, using parameter expansion.

```sh
for P in *.doc *.mp3 *.wav *.txt; do
  mv -- "$P" "${P// /_}"
done
```

Note the replacement syntax is different to that of `sed`. (Also, the double dash will skip files that are unchanged I think. Or use `mv -i`?)

Here we rename `README.md` to `index.md` (rather than changing extension):

```sh
for P in **/README.md; do git mv -v "$P" "${P//README.md/index.md}"; done
```

Use `-n|--dry-run` flag to preview first (Linux only). Using he `-v|--verbose` flag is implied with that.

Replace an _underscore_ with a _dash_ in file and directory names - using [globstar][].

```sh
for P in **/*_*; do
  git mv --verbose "$P" "${P//_/-}"
done
```

Notes:

- This ignores hidden files. Important - ignore `.git`. Also note that files in `.github` like templates might only work using dashes.
- That will **ignore** the top-level, so be sure to run also for just `*_*`.
- That will give an **error** on directory paths which have an underscore in multiple levels, as it will try to name the same item multiple times in one run. So you have to run the command a second time to catch those. If you have two directory levels and a file inside all with underscores, you'll have to run the command 3 times, which is okay.
- The above can be followed by a search in the IDE with regex pattern. Note that cases should be checked before replacing. Pattern: `\w+_\w+\.md`


### Rename tool

Install the `rename` utility.

```sh
sudo apt install rename
```

Rename file extension of a batch of files.

```sh
rename 's/.foo/.bar/' *.foo
```

[globstar]: {{ site.baseurl }}{% link cheatsheets/shell/files/globstar.md %}
