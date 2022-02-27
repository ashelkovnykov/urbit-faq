# Urbit

This section contains miscellaneous tidbits about Urbit and being an Urbit pilot.

## Contents

[Fake-ship networking](#fake-ship-networking) \
[How does Urbit time work?](#how-does-urbit-time-work) \
[Is there a place to browse apps?](#is-there-a-place-to-browse-apps) \
[Is there a place to browse in-development apps?](#is-there-a-place-to-browse-in-development-apps) \
[What are brass, solid, and ivory pills?](#what-are-brass-solid-and-ivory-pills) \
[What do the different symbols at the start of dojo commands mean?](#what-do-the-different-symbols-at-the-start-of-dojo-commands-mean) \
[What is `lens`?](#what-is-lens) \
[What is `herb`?](#what-is-herb) \
[Where can I find all the dojo commands?](#where-can-i-find-all-the-dojo-commands)

### Fake-ship networking

Fake ships run on their own network using fake keys and do not communicate with live-net ships in any way. Multiple
fake ships running on the same machine can network with each other.

***source:*** *`~monted-tallex`*\
***context:*** *NONE*\
***location:*** *TODO*\
***documented:*** *TODO*

### How does Urbit time work?

Some facts about Urbit time:
- The base unit of time is `1/2^64` seconds
- `1` second is `2^16`
- Epoch is ~292.5 billion years ago

***source:*** *`~rabsef-bicrym`, `~sarpen-laplux`, `~tinnus-napbus`*\
***context:*** https://github.com/urbit/urbit/blob/b8da026c5e0d7667c30f55298232da0f55004650/pkg/npm/api/lib/lib.ts \
***location:*** *TODO*\
***documented:*** *TODO*

### Is there a place to browse apps?

The best place is the [awesome-urbit](https://github.com/urbit/awesome-urbit) repo.

***source:*** *`~timluc-miptev`*\
***context:*** *NONE* \
***location:*** *TODO*\
***documented:*** *TODO*

### Is there a place to browse in-development apps?

There's no one place to browse all announced in-development apps. It's best to check in the following places for news
regarding in-development apps:
- [Grants/Bounties page](https://urbit.org/grants)
- "Development" chat in the "Urbit Community" group
- Urbit company groups:
    - Bank of Urbit (`~finnem/the-bank-of-urbit`)
    - Dalten Collective
    - dcSpark
    - Tirrel
    - Uqbar

***source:*** *`~timluc-miptev`*\
***context:*** *NONE* \
***location:*** *TODO*\
***documented:*** *TODO*

### What are brass, solid, and ivory pills?

A pill is a bootstrap sequence used to launch an Urbit ship for the first time. It consists of 3 ordered lists:
1. A list of events to create the Arvo kernel
2. A list of Arvo events
3. A list of userspace events to set up the Clay file system

The brass pill is a full bootstrap pill for production.

The solid pill is a fast-boot for development.

The ivory pill is for runtime support and is not bootable.

***source:*** *`~tinnus-napbus`*\
***context:*** *NONE*\
***location:*** https://urbit.org/docs/glossary/pill \
***documented:*** *false*

### What do the different symbols at the start of dojo commands mean?

```
+foo 'some' 'args'        ::  run %/gen/foo.hoon with args
|foo 'some' 'args'        ::  poke %hood with output of %/gen/foo.hoon with args
:foo|bar 'some' 'args'    ::  poke %foo with output of %/gen/bar.hoon with args
:foo ['some' 'args']      ::  poke %foo with args as a %noun mark
:foo &bar ['some' 'args'] ::  poke %foo with args as a %bar mark
:~zod/foo ['some' 'args'] ::  poke %foo on ~zod with args as a %noun mark
-foo 'some' 'args'        ::  run thread %foo with args
-foo!bar 'some' 'args'    ::  run thread %bar in desk %foo with args
```

***source:*** *`~tinnus-napbus`*\
***context:*** *NONE*\
***location:*** *TODO*\
***documented:*** *TODO*

### What is `lens`?

`lens` implements a subset of the dojo for use over HTTP, to be used by `herb`.

***source:*** *`~hastuc-dibtux`*\
***context:*** *NONE*\
***location:*** *TODO*\
***documented:*** *TODO*

### What is `herb`?

`herb` is a Python CLI tool for controlling a local ship over HTTP. It supports a subset of the dojo syntax, and is
slated to be replaced by a new vane.

***source:*** *`~master-morzod`*\
***context:*** *NONE* \
***location:*** *TODO*\
***documented:*** *TODO*

### Where can I find all the dojo commands?

The `+help` command will print out all dojo commands, as well as a short documentation blurb (if they have one). In
addition, tab-based auto-completion works in the dojo (including STL functions).

***source:*** *`~rovnys-ricfer`*\
***context:*** *NONE* \
***location:*** *TODO*\
***documented:*** *TODO*
