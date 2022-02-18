# Hoon

This section contains information about Hoon development.

## Contents

[Casting: Pull type out of gate output](#casting-pull-type-out-of-gate-output) \
[`cord`: iterating over byte data](#cord-iterating-over-byte-data) \
[Error: `fire-core -fork-type.#! expected-fork-to-be-core fire-type`](#error-fire-core--fork-type-expected-fork-to-be-core-fire-type) \
[Error: `mint-vain` or `-need.[i=@ t=it(@)] -have.%~` when using `list`](#error-mint-vain-or--needi-tit--have-when-using-list) \
[Functions: core vs. door, gate vs. trap](#functions-core-vs-door-gate-vs-trap) \
[Functions: why are some arms called by `arm:core` and some by `~(arm core input)`?](#functions-why-are-some-arms-called-by-armcore-and-some-by-arm-core-input) \
[Operators: what does the `,` operator do?](#operators-what-does-the--operator-do) \
[Parsing: `cord` vs. `tape`](#parsing-cord-vs-tape) \
[Runes: how does bucket (`$^`) work?](#runes-how-does-bucket--work) \
[Runes: how does siglus (`~+`) work?](#runes-how-does-siglus--work) \
[Runes: how does sigtis (`~=`) work?](#runes-how-does-sigtis--work) \
[What is `lull`?](#what-is-lull) \
[What is "normalization"?](#what-is-normalization) \
[What is the directory structure of a desk (e.g. `urbit/pkg/arvo`)?](#what-is-the-directory-structure-of-a-desk-eg-urbitpkgarvo) \
[What is `zuse`?](#what-is-zuse) \
[Who/what uses the `;` XML runes?](#whowhat-uses-the-mic--xml-runes)

### Casting: Pull type out of gate output

Consider the following situation: you have a `wet` `gate` `f` which takes another `gate` `g` as input. You want to
explicitly cast the output of `f` to some type which is wholly or partially derived from the output of `g`. For example,
let's say that `g` returns a `unit` of some type and `f` returns the value within that `unit`. The following code
demonstrates how to do so:
```
|*  g=$-(@ud (unit *))
^-  $_
    =/  a  (_*g)
    ?>  ?=  [~ *]  a
    u.a
=/  b  (g 40)
?~  b
  'a'
u.b
```
Another, simplified method to do so for this particular case is:
```
|*  g=$-(@ud (unit *))
^-  _(need *g)
=/  b  (g 40)
?~  b
  'a'
u.b
```
You can verify that both of these functions behave as intended by saving the above code as generators `gen1` and `gen2`,
then using the "type spear" (`-:!>(l)`) as follows:
```
> =g |=(a=@ud ^-((unit @t) (some `@t`a)))
>
> =t +gen1 some
> t
40
> -:!>(t)
#t/*
> =t +gen1 g
> t
'('
> -:!>(t)
#t/@t
>
> =t +gen2 some
> t
40
> -:!>(t)
#t/*
> =t +gen2 g
> t
'('
> -:!>(t)
#t/@t
```

***source:*** *`~finmep-lanteb`, `~sarpen-laplux`*\
***context:*** *TODO*\
***location:*** *TODO*\
***documented:*** *TODO*

### `cord`: iterating over byte data

Using `rsh` to iterate over the byte data of a `cord` reallocates the entire remainder of the string. Instead, it's much
more efficient to keep a "cursor" into the `cord` and slice bytes out of it using `cut`. Iterating over a `cord` using
`cut` takes roughly 15% the amount of time as iterating over the same `cord` using `rsh`.

***source:*** *`~master-morzod`*\
***context:*** https://github.com/urbit/urbit/pull/5456#issuecomment-980657399 \
***location:*** *TODO*\
***documented:*** *TODO*

### Error: `fire-core -fork-type.#! expected-fork-to-be-core fire-type`

This error has been known to appear in the following situations:
- Attempting to use the head of a `list` as the key in a `map` without checking whether the `list` is `null`

***source:*** *`~finmep-lanteb`*\
***context:*** *TODO*\
***location:*** *TODO*\
***documented:*** *TODO*

### Error: `mint-vain` or `-need.[i=@ t=it(@)] -have.%~` when using `list`

This is known in Urbit as the TMI or "Too Much Information" problem: when the compiler knows too much about the type of
a noun, to the point where it interferes with casting (usually when modifying the subject). Most often, this happens
when working with a `list`, after checking whether it is `null`.

The example below should illustrate how the compiler sees the types after using various Hoon runes, and the link below
should provide additional context.

```
> =l1 `(list @)`[1 2 3 ~]
> =l2 `(list @)`~
> =f1 |=(l=(list @) ?~(l -:!>(l) -:!>(l)))
> =f2 |=(l=(list @) ?:(=(~ l) -:!>(l) -:!>(l)))
> =f3 |=(l=(list @) ?:(?=(~ l) -:!>(l) -:!>(l)))
> =f4 |=(l=(list @) ?:(.=(~ l) -:!>(l) -:!>(l)))
> =f5 |=(l=(list @) ?:(?=(~ l) -:!>(l) =>(.(l `(list @)`l) -:!>(l))))
> (f1 l1)
#t/[i=@ t=it(@)]
> (f2 l1)
#t/it(@)
> (f3 l1)
#t/[i=@ t=it(@)]
> (f4 l1)
#t/it(@)
> (f5 l1)
#t/it(@)
> (f1 l2)
#t/%~
> (f2 l2)
#t/it(@)
> (f3 l2)
#t/%~
> (f4 l2)
#t/it(@)
> (f5 l2)
#t/%~
```

***source:*** *`~norsyr-torryn`*\
***context:*** https://github.com/urbit/arvo/issues/1024 \
***location:*** *TODO*\
***documented:*** *TODO*

### Functions: core vs. door, gate vs. trap

Just as a door is just a core with a sample, a gate is just a trap with a sample. This can be confirmed by the following
code:
```
> =foo |=(a=@ a)
> =bar =|(@ |.(+6))
> (foo 5)
5
> (bar 5)
5
```

***source:*** *`~master-morzod`*\
***context:*** https://urbit.org/docs/hoon/hoon-school/doors \
***location:*** *TODO*\
***documented:*** *TODO*

### Functions: why are some arms called by `arm:core` and some by `~(arm core input)`?

`~(arm core input)` is a call to an arm in a door (i.e. a core with a sample) and `input` is being used to parameterize
the door. `arm:core` is how you call an arm of a core (i.e. no sample, nothing to parameterize). 

***source:*** *`~master-morzod`*\
***context:*** *TODO* \
***location:*** *TODO*\
***documented:*** *TODO*

### Operators: what does the `,` operator do?

The `,` operator strips faces from values.

***source:*** *`~finmep-lanteb`*\
***context:*** *NONE* \
***location:*** *TODO*\
***documented:*** *TODO*

### Parsing: `cord` vs. `tape`

Parsing a `cord` instead of a `tape` trades memory pressure for call overhead. Traversing a contiguously allocated cons
list (the ideal case for a `tape`) is pretty quick. However, operations on a `cord` can be jetted much more effectively.

***source:*** *`~master-morzod`*\
***context:*** https://github.com/urbit/urbit/pull/5456 \
***location:*** *TODO*\
***documented:*** *TODO*

### Runes: how does bucket (`$^`) work?

`$^` works exactly like `$@`, but with input of the form `[^ [^ ^]]` instead of form `[@ ^]`.

***source:*** *`~finmep-lanteb`*\
***context:*** *TODO* \
***location:*** *TODO*\
***documented:*** *TODO*

### Runes: how does siglus (`~+`) work?

`~+` caches the product of a `[formula subject]` pair until the end of the current "road" (see context).

Each Arvo event runs on the kernel's "outer" road. However, the kernel may launch "inner" roads for processes such as
userspace agents. Calling the scry rune (`.^`) will also start an "inner" road using the kernel's scry-handler. Thus,
the following stack of roads is common:
```
kernel road -> agent road -> scry road
```
Each road has its own cache, which is cleared when the road is removed at the conclusion of its task (the above example
would have three caches which would be cleared in reverse order).

***source:*** *`~master-morzod`, `~ritpub-sipsyl`, `~tinnus-naptbus`*\
***context:*** https://github.com/urbit/urbit/blob/master/doc/spec/u3.md#u3-the-road-model \
***location:*** *TODO*\
***documented:*** *TODO*

### Runes: how does sigtis (`~=`) work?

If the values of the two arguments `p` and `q` are identical, `q` is discarded and replaced with a reference to `p`.
This allows Hoon to save space when pinning a new result to the subject by referencing existing data.

***source:*** *`~timluc-miptev`*\
***context:*** *TODO* \
***location:*** *TODO*\
***documented:*** *TODO*

### What is `lull`?

`lull` is a library of userspace `mold`s.

***source:*** *`~timluc-miptev`*\
***context:*** *TODO* \
***location:*** *TODO*\
***documented:*** *TODO*

### What is "normalization"?

Normalization is a two-step process performed by the compiler when a mold receives a value:

1. Make sure that the types match so that the mold can be applied to the value
2. Apply the faces of the mold to the value

It's similar to the casting process in C: you can access the fields of an object through a pointer either directly (i.e.
byte data) or by casting the pointer and using the member names.

***source:*** *`~timluc-miptev`*\
***context:***  *NONE*\
***location:*** *TODO*\
***documented:*** *TODO*

### What is the directory structure of a desk (e.g. `urbit/pkg/arvo`)?

```
app:    gall apps
gen:    generators
lib:    libraries which may be called from any other directory
mar:    marks
sur:    molds
sys:    core OS code (hoon.hoon, arvo.hoon, vanes)
ted:    threads
tests:  unit tests
```

***source:*** *`~timluv-miptev`*\
***context:*** \
https://github.com/urbit/urbit/tree/master/pkg/arvo \
https://urbit.org/docs/userspace/dist/dist#anatomy-of-a-desk \
***location:*** *TODO*\
***documented:*** *TODO*

### What is `zuse`?

`zuse` is like a userspace stdlib.

***source:*** *`~timluc-miptev`*\
***context:*** *TODO* \
***location:*** *TODO*\
***documented:*** *TODO*

### Who/what uses the mic (`;`) XML runes?

The XML runes are used by Sail, a domain-specific language for Hoon used to express XML data structures for rendering in
webpages. Sail is documented poorly and used very little.

***source:*** *`~master-morzod`*\
***context:*** www.sppxiii.com/docs/learn/arvo/sail/index.html \
***location:*** *TODO*\
***documented:*** *TODO*
