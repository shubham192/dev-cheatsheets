---
title: Manage
description: Managing gems for Ruby and Jekyll projects
---

## Check environment

```sh
$ gem env
```

```sh
$ bundle env
```


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
$ ls vendor/bundle/ruby/2.6.0/gems
```
```
addressable-2.7.0           i18n-0.9.5      ...
...
```


## Find gem locally

For a gem installed locally such as `minima` theme.

Show path:

```sh
$ bundle show minima
```
```
<PATH_TO_REPO>/vendor/bundle/ruby/2.6.0/gems/minima-2.5.1
```

Open VS Code in that directory. Use `open` to use your file browser.

```sh
$ code $(bundle show minima)
```


### Gem CLI

Calling `gem` from the shell.

See also [plugins][] page.

[plugins]: {{ site.baseurl }}{% link cheatsheets/jekyll/plugins/index.md %}

### Help

```sh
$ gem COMMAND --help
$ gem help COMMAND
```
### Install

```sh
$ gem install GEMNAME
$ gem install GEMNAME --user-level
```

### Update

Upgrade target gem.

```sh
$ gem update GEMNAME
```

```
Usage: gem update GEMNAME [GEMNAME ...] [options]
```

Upgrade many gems.

```sh
$ gem update
$ gem update --system
```

```
gem help update
Usage: gem update GEMNAME [GEMNAME ...] [options]

  Options:
      --system                     Update the RubyGems system software
```

### Ruby gem command

Calling `gem` from inside Ruby code including a `Gemfile`.

See [Gem versions]({{ site.baseurl }}{% link cheatsheets/ruby/dependencies/versions.md %}) page.

### Bundler command

Gemfile should like this:

```ruby
gem 'foo'
gem 'bar', '~> 2.5'
```

Then Bundler will use that.

```sh
$ bundle install
```


## Install from GitHub

```ruby
gem 'bar', git: 'https://github.com/foo/bar'
```
