# Hoon

This section contains information about Hoon/Arvo development.

## Contents

[Auras: how does Urbit time work?](#auras-how-does-urbit-time-work) \
[Best Practice: iterating over `cord`](#best-practice-iterating-over-cord) \
[Best Practice: disable `%spot` hints](#best-practice-disable-spot-hints) \
[Best Practice: helper cores in Gall agents](#best-practice-helper-cores-in-gall-agents) \
[Best Practice: when to use lead cores?](#best-practice-when-to-use-lead-cores) \
[Build: do I need to compile a new pill for changes to Urbit source code?](#build-do-i-need-to-compile-a-new-pill-for-changes-to-urbit-source-code) \
[Build: how do I test that `lib`/`sur` files are correct?](#build-how-do-i-test-that-libsur-files-are-correct) \
[Casting: auras from text](#casting-auras-from-text) \
[Casting: pull type out of gate output](#casting-pull-type-out-of-gate-output) \
[Casting: what is "normalization"?](#casting-what-is-normalization) \
[Error: `bail:meme` vs. `recover: dig: meme`](#error-bailmeme-vs-recover-dig-meme) \
[Error: `fire-core -fork-type.#! expected-fork-to-be-core fire-type`](#error-fire-core--fork-type-expected-fork-to-be-core-fire-type) \
[Error: `fuse-loop`](#error-fuse-loop) \
[Error: `mint-vain` or `-need.[i=@ t=it(@)] -have.%~` when using `list`](#error-mint-vain-or--needi-tit--have-when-using-list) \
[Functions: how does `+cap` work?](#functions-how-does-cap-work) \
[Functions: how does `+mas` work?](#functions-how-does-mas-work) \
[Functions: when should `homo` be used?](#functions-when-should-homo-be-used) \
[Gates: core vs. door, gate vs. trap](#gates-core-vs-door-gate-vs-trap) \
[Gates: gates with default values](#gates-gates-with-default-values) \
[Gates: is it bad to recurse on a wet gate?](#gates-is-it-bad-to-recurse-on-a-wet-gate) \
[Gates: naming a `|-` gate used as a loop](#gates-naming-a---gate-used-as-a-loop) \
[Jets: what are jet labels?](#jets-what-are-jet-labels) \
[Molds: bunt value of a mold](#molds-bunt-value-of-a-mold) \
[Operators: what does the `,` operator do?](#operators-what-does-the--operator-do) \
[Parsing: `cord` vs. `tape`](#parsing-cord-vs-tape) \
[Rendering: difference between `%palm` and `%rose` in `$tank`](#rendering-difference-between-palm-and-rose-in-tank) \
[Runes: centis (`%=`) vs. cencab (`%_`)](#runes-centis--vs-cencab-_) \
[Runes: how does bucket (`$^`) work?](#runes-how-does-bucket--work) \
[Runes: how does siglus (`~+`) work?](#runes-how-does-siglus--work) \
[Runes: how does sigtis (`~=`) work?](#runes-how-does-sigtis--work) \
[Runes: how does tissig (`=~`) work?](#runes-how-does-tissig--work) \
[Scry: can I virutalize scries?](#scry-can-i-virtualize-scries) \
[Scry: pass-through scry handler](#scry-pass-through-scry-handler) \
[Scry: what if the agent I'm scrying isn't running?](#scry-what-if-the-agent-im-scrying-isnt-running) \
[Scry: why does `.^` return `(unit (unit))`?](#scry-why-does--return-unit-unit) \
[Testing: disable authentication for HTTP](#testing-disable-authentication-for-http) \
[Types: inference via pattern matching](#types-inference-via-pattern-matching) \
[Types: recursive types](#types-recursive-types) \
[Vanes: Behn inserts src.bowl into timer path](#vanes-behn-inserts-srcbowl-into-timer-path)

### Auras: how does Urbit time work?

Absolute Urbit time (`@da`) is a compound value:
- The low 64 bits are `1/2^64` sub-second precision
- The high 64 bits TAI64 with a different epoch (~292.5 billion years ago)

***source:*** *`~master-morzod`*\
***context:*** https://github.com/urbit/urbit/blob/b8da026c5e0d7667c30f55298232da0f55004650/pkg/npm/api/lib/lib.ts \
***location:*** *TODO*

### Best Practice: iterating over `cord`

Using `rsh` to iterate over the byte data of a `cord` reallocates the entire remainder of the string. Instead, it's much
more efficient to keep a "cursor" into the `cord` and slice bytes out of it using `cut`. Iterating over a `cord` using
`cut` takes roughly 15% the amount of time as iterating over the same `cord` using `rsh`.

***source:*** *`~master-morzod`*\
***context:*** https://github.com/urbit/urbit/pull/5456#issuecomment-980657399 \
***location:*** *TODO*

### Best Practice: disable `%spot` hints

`%spot` hints capture source locations, which get pushed onto the stack in case a trace needs to be generated. You can
squeeze more performance out of code after it's confirmed to compile by disabling `%spot` hints by adding the `!.` rune
to the beginning.

***source:*** *`~master-morzod`*\
***context:*** *NONE* \
***location:*** *TODO*

### Best Practice: helper cores in Gall agents

If you have a helper core with a sample (a door) in your Gall agent, wrap it in another core and put the door under a
named arm. Then, make an alias to point at that arm. This will prevent weird bugs whose root cause is accidentally
invoking arms from the helper core without using the alias, and hence forgetting to set the sample of the core
correctly.

Ex:
```
++  helper-core  ~(. ^helper-core bowl)
...
|%
++  helper-core
  |_  =bowl:gall
  ...
==
```

***source:*** *`~midlev-mindyr`*\
***context:*** *NONE* \
***location:*** *TODO*

### Best Practice: when to use lead cores?

Lead cores are opaque, so you can't read their payload. This is very nice if you want to use the `=,` rune on a core
which has a massive payload. For example, the `html` and `mimes:html` arms in `zuse` are lead cores so that users don't
import multiple copies of the standard library into their subject.

***source:*** *`~master-morzod`*\
***context:*** *NONE* \
***location:*** *TODO*

### Build: do I need to compile a new pill for changes to Urbit source code?

No, but depending on the size of the changes that you're making, to how many ships you need to apply the changes, and
with how many people you need to share the changes, it may make development much faster.

***source:*** *`~master-morzod`*\
***context:*** *TODO*\
***location:*** *TODO*

### Build: how do I test that `lib`/`sur` files are correct?

An easy way to check from inside an Urbit ship is to run the following commands:
```
> =l -build-file %/lib/my-file/hoon
> (my-gate:l 'some' 'args')

> =j -build-file /=/some-desk/=/lib/my-other-file/hoon
> (my-other-gate:j 'some' 'args')
```

***source:*** *`~timluc-miptev`*\
***context:*** *NONE*\
***location:*** https://operators.urbit.org/manual/os/dojo-tools#-build-file

### Casting: pull type out of gate output

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
***location:*** *TODO*

### Casting: auras from text

When attempting to cast an aura from text stored as a `cord` (e.g. casting JSON data), it's necessary to use a
formatting function such as `+slav` or `+slaw`. Just casting the input with kethep (`^-`) or `+soft` won't work, because
auras aren't actual types, they're just hints about how atoms should be interpreted (i.e. the actual type is `atom`).
Therefore, any aura can be trivially converted to any other aura, but the data might not retain the correct meaning
(e.g. `^-  @ud  '42'` is `12.852`, not `42`).

***source:*** *`~dozreg-toplud`*\
***context:*** https://docs.urbit.org/language/hoon/reference/stdlib/4m#slaw \
***location:*** *TODO*

### Casting: what is "normalization"?

Normalization is a two-step process performed by the compiler when a mold receives a value:

1. Make sure that the types match so that the mold can be applied to the value
2. Apply the faces of the mold to the value

It's similar to the casting process in C: you can access the fields of an object through a pointer either directly (i.e.
byte data) or by casting the pointer and using the member names.

***source:*** *`~timluc-miptev`*\
***context:***  *NONE*\
***location:*** *TODO*

### Error: `bail:meme` vs. `recover: dig: meme`

Both of these errors are the same: OOM. However, `bail:meme` comes from Arvo itself whereas `recover: dig: meme` comes
from a signal handler. In other words, the latter is detected by the runtime, which stops the computation and notifies
the kernel which produces the former.

***source:*** *`~master-morzod`*\
***context:*** *TODO*\
***location:*** *TODO*

### Error: `fire-core -fork-type.#! expected-fork-to-be-core fire-type`

This error has been known to appear in the following situations:
- Attempting to use the head of a `list` as the key in a `map` without checking whether the `list` is `null`

***source:*** *`~finmep-lanteb`*\
***context:*** *TODO*\
***location:*** *TODO*

### Error: `fuse-loop`

Any `_-loop` error in the compiler comes from expanding a `%hold` type, and then subsequently encountering that same
`%hold` type again (i.e. a recursive definition error in the encountered type).

`fuse` refers to `+fuse:ut`, which is used for type intersection during branch specialization in `?:`, `?=`, and any
other rune that reduces to those two. If a pattern matches a wing, the type of the pattern is "added" to the type of the
wing, otherwise it's "subtracted". Therefore, `fuse-loop` means the detection of an infinite loop in the type definition
during branch specialization.

The above is the exact, technical answer. At a practical level, it's a rather rare compiler error as, generally, type
recursion issues tend to be caught by `fish-loop` (e.g. attempting to use `?=` with a recursive mold like `list`). A
`fuse-loop` error implies that there is a recursive type error in the type of the wing of a `?=` / `?:`, as opposed to
the type of the pattern (i.e. in the `~[1 2 3 4]` of `?=((list @) ~[1 2 3 4])`, as opposed to the `(list @)`).

***source:*** *`~master-morzod`*\
***context:*** https://developers.urbit.org/guides/core/hoon-school/I-testing#fish-loop \
***location:*** *TODO*

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

***source:*** *`~norsyr-torryn`, `~palfun-foslup`, `~sorreg-namtyv`*\
***context:***
- https://github.com/urbit/arvo/issues/1024
- https://web.archive.org/web/20140424223310/http://urbit.org/doc/hoon/tut/7/

***location:*** *TODO*

### Functions: how does `+cap` work?

First, see the section below on how `+mas` works. Then, it's a similar idea:
`+cap` determines whether the index is within the head or the tail of a tree,
therefore (using the bit labeling from the `+mas` example), all we care about is
bit `a`, representing the first "turn". If `a` is `0`, then the index is in the
head; otherwise, it's in the tail.

***source:*** *`~finmep-lanteb`* \
***context:*** NONE \
***location:*** https://docs.urbit.org/language/hoon/reference/stdlib/1b#cap

### Functions: how does `+mas` work?

If the bits of the atom passed in to `+mas` as input look like the following:
```
1abc...z
```
(ex: `13 = 0b1101`, therefore `a = 1`, `b = 0`, and `c = 1`), then what `+mas`
is really doing is returning the atom made from bits:
```
1bc...z
```
(ex: `(mas 13) = 5 = 0b101`)

This is because the bits of an atom representing an index encode how to walk
through the tree in the following way:
```
        1101
        ^^^^
        ||||
RIGHT --+┘||
LEFT  --+-┘|
RIGHT --+--┘
DONE  --┘
```

Since `+mas` is computing the relative index within either the head or tail, we
don't care about the first "turn" being right or left, so we drop the bit
entirely.

***source:*** *`~finmep-lanteb`* \
***context:*** NONE \
***location:*** https://docs.urbit.org/language/hoon/reference/stdlib/1b#mas

### Functions: when should `homo` be used?

`+homo` is occasionally necessary to unify the types of disparate inputs when working simultaneously with `$list`s and
wet gates, typically when dealing with "casually constructed" `$list`s (e.g. `(limo %a %b %c)`):
```
> =l (limo [%abc %def 123 ~])
> ? l
  ^#3.?([i=%abc t=#3] ^#2.?([i=%def t=#2] it(@ud)))
[i=%abc t=[i=%def t=~[123]]]
> ? (homo l)
  it(?(%abc %def @ud))
~[%abc %def 123]
```

***source:*** *`~master-morzod`* \
***context:*** https://docs.urbit.org/courses/hoon-school/R-metals#wet-gates \
***location:*** https://docs.urbit.org/language/hoon/reference/stdlib/2b#homo

### Gates: core vs. door, gate vs. trap

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
***context:***
- https://developers.urbit.org/reference/glossary/core
- https://developers.urbit.org/reference/glossary/door
- https://developers.urbit.org/reference/glossary/gate
- https://developers.urbit.org/reference/glossary/trap

***location:*** https://developers.urbit.org/guides/core/hoon-school/F-cores#repeating-yourself-using-a-trap

### Gates: gates with default values

The `|:` rune produces gates which have default values for the sample. If the sample has multiple values, but you only
want to override one (or just not all) of them, you need to call the gate using the `%*` rune:
```
> =f |:  [a=1 b=2]  (add a b)
> %*  $  f  b  4  ==
5
```

***source:*** *`~tinnus-napbus`* \
***context:*** https://developers.urbit.org/reference/hoon/rune/cen#-centar \
***location:*** *TODO*

### Gates: is it bad to recurse on a wet gate?

Wet gate recursion is almost never desirable: the `+mull` check is re-performed at each call site, meaning that each
iteration could potentially have a different type (producing heterogeneous `list`s, for example).

***source:*** *`~master-morzod`* \
***context:*** *NONE*\
***location:*** https://docs.urbit.org/courses/hoon-school/R-metals

### Gates: naming a `|-` gate used as a loop

If you want to assign a name to a gate created by `|-` that's used as a loop (for example, if you have nested loops),
the best way to do so is to use create an alias with the `=*` rune:
```
...
|-
=*  outer-loop  $
...
|-
...
```

***source:*** *`~palfun-foslup`, `~rovnys-ricfer`*\
***context:*** *NONE*\
***location:*** *TODO*

### Jets: what are jet labels?

Jet labels are the list passed as argument `r` to sigcen (`~%`) (typically `~`). They are hooks to Hoon code that the
jet may use as a part of its implementation. An example for why one may wish to use these hooks is that the jet may have
only a partial implementation, so it wants to call the raw Hoon code for a subset of its domain. Another example is that
the jet may only perform pre / post processing on input that is computed by raw Hoon.

***source:*** *`finmep-lanteb`* \
***context:*** https://docs.urbit.org/language/hoon/reference/rune/sig#-sigcen \
***location:*** *TODO*

### Molds: bunt value of a mold

By default, molds bunt to their final type case, e.g.:

```
> =mold $?  @ux  [%b @ub]  [%d @ud]  ==
> *mold
[%d 0]
```

***source:*** *`master-morzod`* \
***context:*** https://docs.urbit.org/courses/hoon-school/E-types \
***location:*** [Mold](https://docs.urbit.org/glossary/mold), [Bunt](https://docs.urbit.org/glossary/bunt)

### Operators: what does the `,` operator do?

When used as a wing, `,` strips faces from legs:
```
> =x a=[b=1 c=2]
> ,.x
[b=1 c=2]
```

However, on its own `,` is irregular syntax for `^:`. This will switch the compiler into "structure" mode, thereby
producing a mold:
```
> ,@
<1.mek [* [our=@p now=@da eny=@uvJ] <17.fkq 33.ehb 14.dyd 53.vlb 77.lrt 232.oiq 51.qbt 123.zao 46.hgz 1.pnw %140>]>
> (,@)
0
```

***source:*** *`~finmep-lanteb`, `~sidnym-ladrut`, `~tinnus-napbus`*\
***context:*** https://developers.urbit.org/guides/core/hoon-school/L-struct#structure-mode \
***location:*** https://developers.urbit.org/reference/hoon/limbs/wing

### Parsing: `cord` vs. `tape`

Parsing a `cord` instead of a `tape` trades memory pressure for call overhead. Traversing a contiguously allocated cons
list (the ideal case for a `tape`) is pretty quick. However, operations on a `cord` can be jetted much more effectively.

***source:*** *`~master-morzod`*\
***context:*** https://github.com/urbit/urbit/pull/5456 \
***location:*** *TODO*

### Rendering: difference between `%palm` and `%rose` in `$tank`

In short, `$tank` is massively over-engineered and heavily biased to rendering Hoon. `%palm`s are rendered in
"queenside" format (with backstpes after a new line), whereas `%rose`s are rendered in "kingside" format. This is best
illustrated with these examples:
```
((slog (crip (of-wall:format (~(win re `tank`[%palm ["," "[" "!" "]"] ~['abc' 'def' 'ghi']]) 0 10))) ~) ~)
[   abc
  def
ghi

> ((slog (crip (of-wall:format (~(win re `tank`[%rose ["," "[" "]"] ~['abc' 'def' 'ghi']]) 0 10))) ~) ~)
[ abc
  def
  ghi
]
```

***source:*** *`~master-morzod`, `~sidnym-ladrut`*\
***context:*** *TODO*\
***location:*** *TODO*

### Runes: centis (`%=`) vs. cencab (`%_`)

When modifying the value of a wing, you often modify the type in subtle ways. For example, consider this example from
the STL, which recurses over a `list` using barhep (`|-`):
```
1 ++  lent              ::  length
2   ~/  %lent
3   |=  a=(list)
4   ^-  @
5   =+  b=0
6   |-
7   ?~  a  b
8   $(a t.a, b +(b))
```

Line `8` recurses by calling the irregular form of `%=`. Why not `%_`? After line `7`, we know that `a` is non-null.
Therefore, the type of `a` is no longer `list`, but actually `lest`. However, for the recursion to function correctly,
we need the type of `a` to be `list` when we loop back to line `6`. Therefore, we need to slightly alter the type at the
same time as altering the value.

***source:*** *`~master-morzod`*\
***context:*** *TODO* \
***location:*** *TODO*

### Runes: how does bucket (`$^`) work?

`$^` works exactly like `$@`, but with input of the form `[^ [^ ^]]` instead of form `[@ ^]`.

***source:*** *`~finmep-lanteb`*\
***context:*** *TODO* \
***location:*** *TODO*

### Runes: how does siglus (`~+`) work?

`~+` caches the product of a `[formula subject]` pair until the end of the current "road" (see context).

Each Arvo event runs on the kernel's "outer" road. However, the kernel may launch "inner" roads for virtualizing
processes such as userspace agents. Each road has its own cache, which is cleared when the road is removed at the
conclusion of its task.

***source:*** *`~master-morzod`, `~ritpub-sipsyl`, `~tinnus-napbus`*\
***context:*** https://github.com/urbit/urbit/blob/master/doc/spec/u3.md#u3-the-road-model \
***location:*** https://developers.urbit.org/reference/hoon/rune/sig#-siglus

### Runes: how does sigtis (`~=`) work?

If the values of the two arguments `p` and `q` are identical, `q` is discarded and replaced with a reference to `p`.
This allows Hoon to save space when pinning a new result to the subject by referencing existing data.

***source:*** *`~timluc-miptev`*\
***context:*** *TODO*\
***location:*** https://developers.urbit.org/reference/hoon/rune/sig#-sigtis

### Runes: how does tissig (`=~`) work?

`=~` is a convenient way to compose multiple cores together, though it's rare to see outside of core code (particularly
kernel vanes). Typically, Clay will automatically compose cores for you when building a `.hoon` file in userspace.
However, this needs to be done explicitly in kernel space, so Eyre and Iris use `=~` for this purpose.

A non-idiomatic use case is to use `=~` to pass a single object through a chain of expressions; an example of this can
be found in Ames.

***source:*** *`~watter-parter`*\
***context:*** *TODO*\
***location:*** https://developers.urbit.org/reference/hoon/rune/tis#-tissig

### Scry: can I virtualize scries?

There is no way to virtualize scries directly. Doing so requires the help of a vane, as giving userspace the option to
virtualize scries directly would expose kernel state and violate referential transparency.

***source:*** *`~master-morzod`*\
***context:*** *TODO*\
***location:*** *TODO*

### Scry: pass-through scry handler

Each level of virtualization in Hoon requires an explicit "scry handler": a gate that exists outside the scope of the
virtualization that can handle the virtualized scry operations. However, typically the virtualization call does not
require unique scry handling logic and would prefer to use the parent scry handler. Thus was born the "pass-through scry
handler": a scry handler that just forwards scries to the scry handler being used at *its* level of virtualization:
```
|=(a=^ .*(a [%12 [%0 2] %0 3]))
```

In some cases, this scry handler needs to be passed as compiled Nock. The compiled Nock and the formula to compile it
are below:
```
> .*  0  !=  =>  ~  |=(a=^ ``.*(a [%12 [%0 2] %0 3]))
[[[1 0] [1 0] 2 [0 6] 1 12 [0 2] 0 3] [0 0] 0]
```

***source:*** *`~master-morzod`*\
***context:*** *TODO*\
***location:*** *TODO*

### Scry: what if the agent I'm scrying isn't running?

A scry for an agent that doesn't exist will always crash. However, it's possible to check if an agent is installed and
running using the `u` `care` of a Gall scry:
```
.^(? %gu /our/agent/now)
```

***source:*** *`~tinnus-napbus`*\
***context:*** *TODO*\
***location:*** *TODO*

### Scry: why does `.^` return `(unit (unit))`?

- `~`: "This scry path might have existed in the past, or it may exist in the future, but it doesn't exist for the given
  `cas`."
- `[~ ~]`: "This scry path doesn't exist, has never existed, and will never exist."
- `[~ ~ *]`: "This scry path exists and returned the given data."

***source:*** *`~tinnus-napbus`*\
***context:*** *TODO*\
***location:*** *TODO*

### Testing: disable authentication for HTTP

To disable `+code` authentication when accessing fake ships through the browser, set `authenticated` to always be true
in
[Eyre](https://github.com/urbit/urbit/blob/1b0a7770685afea8abd2f93fea4982e4f8071f0a/pkg/arvo/sys/vane/eyre.hoon#L826).

***source:*** *`~dinleb-rambep`*\
***context:*** *TODO*\
***location:*** *TODO*

### Types: inference via pattern matching

The Hoon compiler is only able to make type inferences through positive pattern matches. Consider the following code
example:

```
> =x `(unit @)`[~ 5]
> ?:  ?=(^ x)  u.x  0
5
> ?:  !?=(@ x)  u.x  0
-find.u.x
find-fork
```

***source:*** *`~dozreg-toplud`* \
***context:*** *TODO* \
***location:*** *TODO*

### Types: recursive types

Recursive types almost always need to have a bunt specified using the `$~` (bucsig) rune to be compiled correctly:
```
|%
+$  recurse
  $~  [*@t ~]
  $:  name=@t
      subs=(map @t recurse)
  ==
--
```

***source:*** *`~rovnys-ricfer`* \
***context:*** *TODO* \
***location:*** *TODO*

### Vanes: Behn inserts `src.bowl` into timer path

When you send a `%wait` task to Behn, it will automatically prepend `src.bowl` to the timer's path. This can be an issue
if you want to cancel timers, as you'll need to track which ship set the timer.

***source:*** *`~hodzod-walrus`*\
***context:*** *TODO*\
***location:*** https://developers.urbit.org/reference/arvo/behn/tasks
