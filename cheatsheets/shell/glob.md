# Glob sheel cheatsheet
> How to match paths using "glob" patterns.  

Globbing is a programming concept that involves the use of wildcards and special characters to match and filter. Glob patterns are similar to regex patterns, but simpler and limited in scope.

This guide is based on Bash and ZSH.


## Resources

- [GNU/Linux Command-Line Tools Summary: Wildcards](http://tldp.org/LDP/GNU-Linux-Tools-Summary/html/x11655.htm)
- [Bash: globbing](http://tldp.org/LDP/abs/html/globbingref.html)
- [Wikipedia: glob (programming)](https://en.wikipedia.org/wiki/Glob_(programming))
- [Linux Programmer's Manual: GLOB(7)](http://man7.org/linux/man-pages/man7/glob.7.html)

[Source](https://github.com/begin/globbing/blob/master/README.md)


## Basics

[Source](https://github.com/begin/globbing/blob/master/cheatsheet.md)

Wildcard matches are used alongside literals.

Basic wildcards:

- `*` - Match any character zero or more times.
- `**` - Match any character zero or more times, including `/` unlike the others.
- `?` - Match any character one time.
- `[abc]` - Match any of the characters.

Example:
- `*/*` - will match `foo/bar`.
- `*` will match all files and directories in the current directory.
	```sh
	echo *
	
	ls *
	
	for P in *; do echo $P; done
	``` 

## Advanced


Wildcard expansion can be done. This can be previed with `echo` but might be used with `ls`.

```sh
echo foo/{bar,baz}
# => foo/bar foo/baz
```

### POSIX character classes

- `[[:alpha:][:digit:]]` -  match `a1` but not `aa`.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4MDczNTM2MDJdfQ==
-->