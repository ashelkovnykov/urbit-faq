# Urbit

This section contains miscellaneous tidbits about Urbit.

## Contents

[Fake-ship networking](#fake-ship-networking) \
[What are brass, solid, and ivory pills?](#what-are-brass-solid-and-ivory-pills) \
[What are the Hoon naming conventions?](#what-are-the-hoon-naming-conventions) \
[What is `lens`?](#what-is-lens) \
[What is `herb`?](#what-is-herb)

### Fake-ship networking

Fake ships run on their own network using fake keys and do not communicate with live-net ships in any way. Multiple
fake ships running on the same machine can network with each other. However, these fake ships still have 'realistic'
packet routing: fake galaxies can talk to each other, but fake stars/planets cannot - unless they have the appropriate
fake sponsors running, too.

```
~tex & ~mex:            GOOD
~tex & ~bintex:         GOOD
~mex & ~bintex:         BAD
~tex, ~mex, & ~bintex:  GOOD
```

***source:*** *`~finmep-lanteb`, `~monted-tallex`*\
***context:*** *NONE*\
***location:*** https://developers.urbit.org/guides/core/environment#fake-ship-networking

### What are brass, solid, and ivory pills?

A pill is a bootstrap sequence used to launch an Urbit ship for the first time. It consists of 3 ordered lists:
1. A list of events to create the Arvo kernel
2. A list of Arvo events
3. A list of userspace events to set up the Clay file system

The brass pill is a full bootstrap pill for production.

The solid pill is a fast-boot for development.

The ivory pill is for runtime support and is not bootable.

***source:*** *`~tinnus-napbus`*\
***context:*** \
- https://operators.urbit.org/manual/os/dojo-tools#brass \
- https://operators.urbit.org/manual/os/dojo-tools#solid \
***location:*** https://urbit.org/docs/glossary/pill

### What are the Hoon naming conventions?

In early Hoon, there were three naming conventions:
- hyperlapidary
- lapidary
- freehand

#### Hyperlapidary

Only to be used for extremely regular and straightforward namespaces. Specifically designed to be as difficult as
possible to refactor. Most often found in the most core sections of Arvo.
- Arms must be gates or doors
  - Gate names should be 3 letters long and should aim to have mnemonic significance (e.g. `dec`)
  - Door names should be 2 letters long and should aim to resemble pronouns (e.g. `my`)
- Conventional recursive structures have standard names
  - The head of a list is `i`, and the tail is `t`
  - The node in a tree is `n`, and the children are `l` and `r` respectively
- Other structures should be short tuples, no wider than 5 elements
  - The legs are named `p`, `q`, `r`, `s`, and `t` in order of appearance
- Variables and arguments should be named alphabetically with a single letter in order of appearance (e.g. `a`, `b`,
  `c`, etc.)

#### Lapidary

The ordinary style of Hoon. Most of Arvo is written in lapidary style.
- Arms must be four letters long
  - They may or may not be English words that may or may not be cleverly relevant
- All variables, arguments, attributes, etc. must be three letters long
  - Preferably consonant-vowel-consonant
  - If the same string is used more than once in a file, it should represent exactly the same concept
    - As if it were copy+pasted there

#### Freehand

God mode. Reserved for top-layer software, prototyping, and casual coding.
- No rules.

***source:*** *`~sorreg-namtyv`*\
***context:*** https://web.archive.org/web/20140424223310/http://urbit.org/doc/hoon/tut/7/ \
***location:*** *TODO*

### What is `lens`?

`lens` implements a subset of the dojo for use over HTTP, to be used by `herb`.

***source:*** *`~hastuc-dibtux`*\
***context:*** *NONE*\
***location:*** *TODO*

### What is `herb`?

`herb` is a Python CLI tool for controlling a local ship over HTTP. It supports a subset of the dojo syntax, and is
slated to be replaced by a new vane.

***source:*** *`~master-morzod`*\
***context:*** *NONE* \
***location:*** *TODO*
