# Winged Cat

Two levels of language:

- Bottom level. Two principles: "Every _thing_ is an object" and "Every
  _action_ is a message passed to/between objects." Very simple syntax to
  achieve those. Blocks (functions) with explicit capturing.

- Syntax level. `syntax` blocks allow custom syntax to be created.

Compilation/execution:

1. Read source
2. Apply syntax blocks to source
3. Goto 1. until no syntax blocks remain
4. Compile / interpret the result

Idea:

- Clear semantics of the language building blocks
- Easy to understand at a basic level
- Has abstraction mechanisms to build more
- Simple to learn

"Everything is an object" …including the global environment / top level.

## Basic/core syntax

```
## Comments
# Must start with # as first character on a line


## Idents
# Can be any Unicode characters...

a
apples
☺
!
,

# ...except whitespace, (, \, or ).
# but (, \, and ) can be escaped with \.

\(
\)
\\

# '#' is also allowed so long as it's not on the first column.
 #

# Naked idents in the global space don't make much sense


## Tuples
# Syntax: '(' [ <ident>... separated by whitespace ] ')'

()
(#)
(a b c)
(apple pies)
(1 2 3 4 5 6)
(, ! a 2 ☺ @)
(\\ \( \))

# ANY Unicode whitespace is fine as separators:
(a b c	d)


## Message passing
# Syntax: <ident>[<tuple>]
# First part is called the "tag"
# Second part is the "arguments", and assumed to be () if not provided
# No whitespace between the two

lemons
apples()
fruit(apples oranges)
@(☺ ☹ )

# Semantics:
# 1. Look up <ident> in the scope
# 2. If it doesn't exist, select catchall
# 3. Send it the arguments


## Scope
# Stack of scopes. A "scope" is lexical, it's the tuple we're in.
# The catchall is defined in all scopes, and most often passes on lookups
# to scope up the stack (except for the last catchall, which fails).

# Send message tag "bottom" to bottom-most scope
bottom

# Equivalent to:
bottom()

# Three layers of scope
bottom(mid (top(foo bar baz)))

# Equivalent to:
bottom(
  mid()
  (
    top(
      foo()
      bar()
      baz()
    )
  )
)


## Built-ins
# Behaviour that is built-in to be able to bootstrap programming

# Define
# Adds ident to the scope with the second item as value
define(ident value)
```
