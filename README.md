doublelist
==========

> A doubly linked list suitable for stack allocation.

## Installation

Add this to `zz.toml`:

```toml
[dependencies]
doublelist = "*"

[repos]
doublelist = "git://github.com/jwerle/doublelist"
```

## Usage

```c++
using doublelist
using log

fn main () -> int {
  // allocate a new mutable list with tail size of 2 (the number of possible elements)
  new+2 mut items = doublelist::make();

  items.push("hello");
  items.push("world");

  // get an iterator to the list
  let mut it = items.iterator();
  while !it.ended {
    let node = it.next();
    log::info("%s", node);
  }

  return 0;
}
```

## API

### List

A doubly linked list suitable for stack allocation.

#### `new+tail l = doublelist::make()`

`List` constructor. Initializes a `List` pointer.

```c++
new+4 items = doublelist::make();
```

#### `node = l.rpush(value)`

Pushes a value to the right of the list updating the tail returning the
newly created list node.

```c++
let hello = l.rpush("hello");
let world = l.rpush("world");
```

_See the [Node API](#node) for more information about list nodes._

#### `node = l.lpush(value)`

Pushes a value to the left of the list updating the head returning the
newly created list node.

```c++
let world = l.lpush("world");
let hello = l.lpush("hello");
```

_See the [Node API](#node) for more information about list nodes._

#### `value = l.rpop()`

Pops a value from the tail of the list.

```c++
let value = l.rpop();
```

#### `value = l.lpop()`

Pops a value from the head of the list.

```c++
let value = l.lpop();
```

#### `l.push(value)`

An alias for `l.rpush(value)`.

```c++
l.push(value);
```

#### `l.unshift(value)`

An alias for `l.lpush(value)`.

```c++
l.unshift(value);
```

#### `l.pop()`

An alias for `l.rpop()`.

```c++
let value = l.pop();
```

#### `l.shift()`

An alias for `l.lpop()`.

```c++
let value = l.shift();
```

#### `it = l.iterator()`

Creates and returns a stack allocated [iterator](#iterator) for a list instance
in the default iterator direction.

```c++
let it = l.iterator();
```

_See the [Iterator API](#iterator) for more information about list iterators._

#### `it = l.iterator_head()`

Creates and returns a stack allocated [iterator](#iterator]) for a list instance
in the "head" direction.

```c++
let it = list.iterator_head();
```

_See the [Iterator API](#iterator) for more information about list iterators._

#### `it = l.iterator_tail()`

Creates and returns a stack allocated iterator for a list instance
in the "tail" direction.

```c++
let it = list.iterator_tail();
```

_See the [Iterator API](#iterator) for more information about list iterators._

#### `it = l.iterator_tail(direction)`

Creates and returns a stack allocated iterator for a list instance
in a given direction.

```c++
let it = list.iterator_with_direction(doublelist::HEAD); // default
// or
let it = list.iterator_with_direction(doublelist::TAIL);
```

_See the [Iterator API](#iterator) for more information about list iterators._

#### `node = l.find(value)`

Finds and returns a list node that points to a value that matches the given
value pointer. A user supplied comparator function can be used by setting
`l.compare` to a `CompareNodeValueFunction(void a*, void *b) -> bool` type,
otherwise simple pointer comparison is used by default.

```c++
let node = l.find("hello");
```

#### `node = l.at(index)`

Finds and returns the list node at a given index.

```c++
l.rpush("hello");
l.rpush("world");
let node = l.at(1);

static_attest(safe(node));
log::info("%s", node->value);
```

#### `l.remove(value)`

Finds a list node that points to a given value and removes it from the list.

```c++
list.rpush("hello");
list.rpush("world");
list.remove("world");
```

#### `l.contains(value)`

Returns a boolean indicating if a list contains a value.

```c++
l.push("hello");

if l.contains("hello") {
  l.push("world");
}
```

#### `size = l.slice(values, offset, count)`

Take a slice from the list at a depth offset.

```c++
void * mut values[2];

l.push("hello");
l.push("world");
l.slice(values, 0, 2);

log::info("%s %s", values[0], values[1]);
```

#### `i = l.index(value)`

Compute the index of a node's value and return it. If not found, then -1 is returned.
-1 is also returned if a compare function is not found.

```c++
new+2 l = doublelist::make();

l.push("hello");
l.push("world");

assert(0 == l.index("hello"));
assert(1 == l.index("world"));
```

### Iterator

An iterator context for a list.

#### `new it = doublelist::iterator::make(list, direction)`

`Iterator` stack constructor with direction from a list. Initializes a
`Iterator` pointer with a `List` pointer and `direction`. Typically, you
will never need to call this function.

#### `it.ended`

A boolean that is set to `true` when the iterator has reach the end of
the list after subsequent calls to `it.next()`.

#### `it.direction`

The direction in which the iterator will traverse the list. See
[Iterator Directions](#iterator-directions) for more information on this
value.

#### `node = it.next()`

Returns a pointer to the next node in the list. This function will set
`it.ended = true` when it has reach the end of the list.

```c+++
let it = l.iterator();
while !it.ended {
  let node = it.next();
}
```

#### `it.seek(node)`

Seeks the iterator to the node.

```c++
it.seek(l.at(l.index("foo")));
```

#### `it.end()`

Marks the iterator as "ended". This will set `it.ended = true` and
remove a reference to any nodes in the list.

#### Iterator Directions

Iterators can traverse a list in the `doublelist::HEAD`
(`doublelist::iterator::Direction::Head`) or `doublelist::TAIL`
(`doublelist::iterator::Direction::Tail`) directions.

### Node

A node structure to contain pointers to a list node's value,
containing List structure, and next/prev nodes.

#### `node.value`

A pointer to the value this node points to.

#### `node.next`

A pointer to the next node in the list.

#### `node.prev`

A pointer to the previous node in the list.

## License

MIT
