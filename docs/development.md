# Development
> Notes for developers working on this project


First follow the install and usage docs to get this project setup locally.


## Editing

Recommended tool for markdown editing:

- [stackedit.io](https://stackedit.io/)

Warning: Adjust the autosave to be say every 5 or 10 minutes so it doesn't trigger a site rebuild every 2 minutes.


## Structure

The menus are auto-generated at each level to make it easy to nest content several layers down and not change a menu manually each time. The approach is in this project comes from this prototype project:

- [github.com/MichaelCurrin/nested-jekyll-menus](https://github.com/MichaelCurrin/nested-jekyll-menus/)

Put content in the [cheatsheets](/cheatsheets/) directory.

This helps when iterating over `site.pages` to separate unrelated page which do not have a prefix - see for example:

- `cheatsheets/base64.md`
- `sitemap.xml`
- `feed.xml`
- `assets/main.scss`
- `cheatsheets/shell/zsh.md`


## Find files with bad layout

Each `index.md` file needs to have `layout: listing` set manually but this can be forgotten. And setting is programmatically has a serious negative impact on memory and therefore build time.

Find all `index.md` files which do not match the layout.

```sh
$ grep -L listing $(find . -type f -name index.md )
```

Sample output before these were fixed:

```
cheatsheets/python/configs/index.md
cheatsheets/javascript/linting/index.md
cheatsheets/javascript/general/index.md
```


## Breadcrumbs

Note on [breadcrumbs.html](/_includes/breadcrumbs.html) and the [path-to-link](/_includes/path-to-link.html).

Split the current path then build up breadcrumbs using home up to
a point and turn that into a link.

Note first two pieces of crumbs are ["", "cheatsheets"].

The outer if statement is to hide entire breadcrumbs section on Home and Cheatsheets pages.

Use the range of `2` to the number of crumbs with `crumbs.size` so that the Home page gets excluded. The value of `2` is used because we slice from `0` (Home) to `1` (Cheatsheets) and so need `2` as one value higher to be inclusive of the `1`.

A naïve approach for breadcrumbs just splits the URL and uses capitalize on the pieces, but that does not work for abbreviations ("NPM Command" -> "Npm Command").

So instead we look up the relevant index.md page and use its title.
But we can't just use the first page we find as there could be duplicate folder names.

Therefore rather than using split pieces, we use the full path (a, then a/b then a/b/c) to find the index files.

Each piece of the breadcrumb pieces needs a bigger slice to build a URL and then find the path. So we use `slice: 0, forloop.index` to get the URL for the first breadcrumb (slice 0 and 1), then the second (slice 0 and 2).
