# Urbit

This section contains miscellaneous tidbits about Urbit.

## Contents

[Debug dashboard](#debug-dashboard)\
[Fake-ship networking](#fake-ship-networking) \
[I messaged my ship from a comet and saw a breach notification. What happened?](#i-messaged-my-ship-from-a-comet-and-saw-a-breach-notification-what-happened) \
[What are brass, solid, and ivory pills?](#what-are-brass-solid-and-ivory-pills) \
[What are the Hoon naming conventions?](#what-are-the-hoon-naming-conventions) \
[What is `lens`?](#what-is-lens) \
[What is `herb`?](#what-is-herb)

### Debug dashboard

Every ship has a debugging dashboard available at `http://localhost:8080/~debug/`. It contains information about:
- Incoming & outgoing app subscriptions
- Running threads
- Status of Ames communication with peers
- Behn timers
- Incoming Eyre HTTP requests

***source:*** *`~nospur-sontud`, `~wicdev-wisryt`*\
***context:*** *NONE*\
***location:*** *TODO*

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

### I messaged my ship from a comet and saw a breach notification. What happened?

When a comet (or any ship) first boots, it needs to bootstrap a library of public keys for other ships registered via
Azimuth. Originally, this meant reading the entire history of the ETH blockchain from the date that the Azimuth contract
was published to the present day, recording who registered what keys when.

In order to improve efficiency, a snapshot of the Azimuth state (and all keys registered at the time) was included in
the solid pill, so that ships boot with a large collection of keys pre-installed. However, the ship still needs to catch
up from that snapshot to the present day, in case new keys were since registered.

Therefore, if your ship breached between the date of the snapshot and the current date, then as your comet is catching
up to the present day, it will receive that breach notification as its updating. This is nothing about which to be
concerned: it's just your comet catching up to the present state of the network.

***source:*** *`~finmep-lanteb`*\
***context:*** *NONE*\
***location:*** *TODO*

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

### What is `%lens`?

`%lens` is an agent which runs on your ship and allows you to send dojo commands to a local ship over HTTP. By default,
it listens on port `12321`.

See the [What is `herb`?](#what-is-herb) section below for more info.

***source:*** *`~finmep-lanteb`, `~hastuc-dibtux`*\
***context:*** *NONE*\
***location:*** *TODO*

### What is `herb`?

`herb` is a Python CLI tool for sending commands to a local ship over HTTP. It supports most of the dojo syntax. `herb`
is closely coupled with `%lens`; in fact, `herb` is just a thin client for formatting HTTP requests to `%lens`.

Together, `herb` and `%lens` have a number of shortcomings:
- Though their functionality is tightly coupled with the dojo, much of the syntactic sugar of the dojo doesn't work
over HTTP
- The output from commands sent via `herb` is not available externally to the ship: all you can see outside the ship is
whether the HTTP request was received or not
- A bad request can cause the `%lens` agent to hang until it is manually cancelled, either from inside the ship or
through a `%cancel` request to `herb`
- If an `herb` HTTP request is rejected by a ship, it's unknowable from outside the ship if this is because `%lens` is
busy or because `%lens` is stuck
- Requires going through Eyre, using HTTP and JSON

For all of these reasons, `herb`/`%lens` are in the process of being replaced by the `conn.c` I/O driver in the runtime
and the Khan thread-management vane.

***source:*** *`~finmep-lanteb`, `~master-morzod`*\
***context:*** *NONE* \
***location:*** *TODO*
