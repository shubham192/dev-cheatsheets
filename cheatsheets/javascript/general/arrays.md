---
title: Arrays
---

See [Array cheatsheet](https://www.shortcutfoo.com/app/dojos/javascript-arrays/cheatsheet) on ShortCutFoo website.


## Read

```javascript
myArray = ['a', 'b', 'c', 'd']
```

Get length.

```javascript
> myArray.length
4
```

Get element - returns one element.

```javascript
> myArray[index]

> myArray[1]
'b'
```

Slice - returns an array of elements.

```javascript
> myArray.slice(start, end)
[ ... ]
> myArray.slice(1, 3)
[ 'b', 'c' ]
```

Leave out `end`.

```javascript
> myArray.slice(start)

> myArray.slice(1)
[ 'b', 'c', 'd' ]
```

### Get last element

#### Use hard bracket slicing

```javascript
> myArray = ['a', 'b', 'c', 'd']
> myArray[myArray.length - 1]
'd'

> myArray = ['a']
> myArray[myArray.length - 1]
'a'
```

In Python you simply use a negative index `my_list[-1]`, but in JS that gives a `undefined`.

#### Use slice

You can use a negative index here.

```javascript
> myArray = ['a', 'b', 'c', 'd']
> myArray.slice(-1)
[ 'd' ]
```

Warning - this returns an array, not a string, so do this.

```javascript
> myArray.slice(-1)[0]
'd'
```


## Update

Mutate element.

```javascript
> myArray[index] = value
```

See also [Pop, Push, Shift and Unshift Array Methods in JavaScript](https://alligator.io/js/push-pop-shift-unshift-array-methods/)

### Modify end

**Remove** last element.

```javascript
> myArray.pop();
```

**Insert** element at the end. i.e. append.

```javascript
> myArray.push(obj)
```

### Modify at the start

**Remove** from the beginning. Like Python's `list.pop(0)`. Returns new length.

```javascript
> myArray.shift()
```

**Insert** at the start. Like Python's `list.insert(0, obj)`. Returns new length.

```javascript
> myArray.shift(obj)
```

### Change order

Reverse.

```javascript
> myArray.reverse()
```

Sort. See [Sort](sort.md) for more info, as this will not sort numbers properly.

```javascript
> myArray.sort()
```

### Join arrays

Concatenate arrays.

```javascript
> myArrayA.concat(myArrayB)
```

Join elements using a separator e.g. `', '`. This will return a single string.

```javascript
> myArray.join(sep)
```


## Map, reduce, filter

Return a new object after applying transformation. The old object is no affected, unless you reassign over the same name.

Examples:

```javascript
myArray.map((x) => x*2)
```

```javascript
myArray.reduce((x, y) => x + y)
```

```javascript
myArray.filter((x) => x > 2)
```
