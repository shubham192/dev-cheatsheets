---
title: Make command syntax
---

Use of `make` command in Unix environments.

Makefile templates topic on GitHub - [makefile-template](https://github.com/topics/makefile-template)

Targets (commands) will differ per project and environment but these can be applied where relevant.

Remember, the `make` command also supports tab autocomplete for targets.

For language specific examples, see my [Code Cookbook](https://github.com/MichaelCurrin/code-cookbook) project's `make` section.


## Phony

This can be done at the start or just be fore each target.

```makefile
.PHONY docs

docs:
	echo 'Test'
```

## Help

### Basic

```make
# Show summary of make targets.
help:
	@echo 'Print lines that are not indented (targets and comments) or empty.'
	@egrep '^\S|^$$' Makefile
```

### Extended

If you use `@echo` within your targets:

```make
# Show summary of make targets.
help:
	@echo 'Print lines that are not indented (targets and comments) or empty, plus any indented echo lines.'
	@egrep '(^\S)|(^$$)|\s+@echo' Makefile
```

Alt echo: `@echo "Include left-aligned, empty lines and echo lines."`

### Detailed

Copied from Poetry's [Makefile](https://github.com/python-poetry/poetry/blob/master/Makefile).

This is complex - I don't know why. I haven't tested yet but maybe something here is useful.

```makefile
# lists all available targets
list:
	@sh -c "$(MAKE) -p no_targets__ | \
		awk -F':' '/^[a-zA-Z0-9][^\$$#\/\\t=]*:([^=]|$$)/ {\
			split(\$$1,A,/ /);for(i in A)print A[i]\
		}' | grep -v '__\$$' | grep -v 'make\[1\]' | grep -v 'Makefile' | sort"
# required for list
no_targets__:

```


## Export

Use variables in commands.

Given file `.env` with variable set as `FOO=bar` and `script_that_echoes_foo.sh` which does `echo $FOO`.

### Do this

The combination of `export` and `source` works well.

```makefile
export FOO=''

test:
  source .env && echo $$FOO
  source .env && ./script_that_echoes_foo.sh
```


### Don't do this

The following will not work as expected ,due to `make` limitations on environment setting of child processes.

```make
test:
  source .env
  echo $$FOO
```

The following will not work either, with or without `export` set at the top.

```makefile
test:
  export $(<.env) && ./script_that_echoes_foo.sh
```


## Control flow

Using conditionals and iteration, similar to shell.

### For

```make
foo:
	for bar in my-dir/*; do \
		export fizz=$$(echo $$bar) \
		make plan; \
	done
```

TODO: remove `\` and `;` and see if `$(MAKE)` is better than `make`.

### If

Note lack of indentation.

```make
TARGET:
ifneq (CONDIITION, )
	ACTION
endif
```

[Syntax](https://www.gnu.org/software/make/manual/html_node/Conditional-Syntax.html) in Make docs.

Example:

```make
libs_for_gcc = -lgnu
normal_libs =

foo: $(objects)
ifeq ($(CC),gcc)
        $(CC) -o foo $(objects) $(libs_for_gcc)
else
        $(CC) -o foo $(objects) $(normal_libs)
endif
```

Alternate:


```make
TARGET:
	@if [ CONDITION ]; then \
		ACTION ; \
	fi
```


## Internal make calls

### Call another target

This will run `make bar`.

```make
foo: bar
```

This will run `make bar`, `make baz` and then only do the command inside `foo`.

```make
foo: bar baz
	echo 'Foo'
```

### Recursive make

Call `make` within a `Makefile`.

This works:

```make
foo:
	make bar
```

This seems preferred as I've seen this in a few places, although from light testing it looks to do the same.

```make
foo:
	$(MAKE) bar
```
