# Jekyll gems cheatsheet

## List installed gems

```sh
$ bundle list
```
```
Gems included by the bundle:
  * addressable (2.7.0)
  * ...
```

Names of directories:

```sh
ls vendor/bundle/ruby/2.6.0/gems
```
```
addressable-2.7.0           i18n-0.9.5      ...
...
```

## Find gem locally

Fr a gem installed locally such as `minima` theme.

Show path:

```sh
$ bundle show minima
```
```
<PATH_TO_REPO>/vendor/bundle/ruby/2.6.0/gems/minima-2.5.1
```

Open VS Code in that directory. Use `open` to use your file browser.

```sh
code $(bundle show minima)
```