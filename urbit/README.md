# Urbit

This section contains miscellaneous tidbits about Urbit.

## Contents

[Can I build a foreign desk using Clay?](#can-i-build-a-foreign-desk-using-clay) \
[Debug dashboard](#debug-dashboard)\
[How do I update to a specific OTA?](#how-do-i-update-to-a-specific-ota) \
[How does Vere bootstrap the Hoon compiler?]() \
[I messaged my ship from a comet and saw a breach notification. What happened?](#i-messaged-my-ship-from-a-comet-and-saw-a-breach-notification-what-happened) \
[Is a sequence of moves in an event guaranteed to terminate?](#is-a-sequence-of-moves-in-an-event-guaranteed-to-terminate) \
[What are the Hoon naming conventions?](#what-are-the-hoon-naming-conventions) \
[What is an event?](#what-is-an-event) \
[What is "remote scry"?](#what-is-remote-scry)

### Can I build a foreign desk using Clay?

Yes. If a copy of the foreign desk is downloaded, then `%a` and `%x` `care`s to Clay work for foreign `beak`s. If the
desk is not downloaded, you would need to manually scry with care `%v` first. 

***source:*** *`~wicdev-wisryt`* \
***context:*** https://developers.urbit.org/reference/arvo/clay/scry \
***location:*** https://developers.urbit.org/reference/arvo/clay/scry

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

### How do I update to a specific OTA?

If a ship misses several OTAs, applying them all at once can become a blocking issue. The solution is to apply a single
OTA at a time.

Let's say we want to OTA specifically to the version of Arvo with `zuse` `%420`:
1. Determine the `cas` (version) of your sponsor's `%kids` desk that you need:
   1. On the host run `.^(* %cw /=kids=)` to get a cell with the form `[X Y]`. Both are the `cas`: the first number is
      the numeric version of the current code, and the second is the date since which that version is active. 
      - Alternatively, it's possible to do the same using `%base` on a ship that has successfully updated past the desired
        update: `.^(* %cw /=base=)`
   2. Call `cat /=kids/<cas>/sys/kelvin` with the numeric `cas` from above to see what the `zuse` version is:
      ```
      > cat /=kids/130/sys/kelvin
      [%zuse 415]
      ```
   3. Walk the `cas` down until you find the lowest `cas` that returns the desired version of `zuse`. For example:
      ```
      > cat /=kids/101/sys/kelvin
      [%zuse 419]
      > cat /=kids/100/sys/kelvin
      [%zuse 420]
      > cat /=kids/99/sys/kelvin
      [%zuse 420]
      > cat /=kids/98/sys/kelvin
      [%zuse 421]
      ```
      In this case, the `cas` we need is `99`.
2. Upgrade to the specific version using the `cas` from above:
   ```
   |merge %base <sponsor> %kids, =cas [%ud <cas>], =gem %only-that
   ```
   For example:
   ```
   |merge %base ~litzod %kids, =cas [%ud 99], =gem %only-that
   ```

***source:*** *`~dovsem-bornyl`*\
***context:*** *TODO*\
***location:*** *TODO*

### How does Vere bootstrap the Hoon compiler

The first Hoon compiler was written in some other language. This compiler was then used to compile `hoon.hoon` to Nock.
Vere is a Nock interpreter, and therefore is able to use the Nock version of `hoon.hoon` to henceforth further compile
Hoon.

When attempting to boot a ship, the third event in the solid or brass pill being used injects a precompiled Nock
`hoon.hoon`. The fourth event is `hoon.hoon` as source, which is compiled by the Nock from the third event into new Nock
which replaces the old Nock from the third event.

***source:*** *`~dozreg-toplud`*\
***context:*** https://docs.urbit.org/glossary/pill \
***location:*** *TODO*

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

### Is a sequence of moves in an event guaranteed to terminate?

No. For example, this is what a "resubscribe loop" is. This will eventually result in a crash due to stack overflow
(usually a graceful crash: `bail %meme`).

***source:*** *`~wicdev-wisryt`*\
***context:*** *NONE*\
***location:*** *TODO*

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

### What is an event?

An event is one Unix move and all of its children. A Unix move is an external interaction with an Urbit ship that may
modify its state. Examples include:
- An Ames packet arrives
- A Behn timer expires
- The Clay files for a mounted desk are changed
- A user types something in the dojo
- An HTTP request comes in over Eyre

All of these individual moves can set in motion additional moves: the Ames packet might contain a poke to a Gall agent which sets
off a Clay recompilation. Thus, an event is the entire set of moves set in motion from the initial move.

***source:*** *`~wicdev-wisryt`*\
***context:*** *NONE* \
***location:*** *TODO*

### What is "remote scry"?

"Remote scry" is an alternative networking protocol to Ames that is implemented in the Fine (fee-nay) vane. It allows
ships to request data from foreign ships in a more efficient way and without affecting the state or event log of the
foreign ship.

Using Ames, every message affects the foreign ship state, even if only by updating sequence numbers. Using Fine,
requests are read-only and globally referentially transparent (responses to identical requests are always identical).
Since read requests do not affect state, they can be spawned in parallel threads and the results can be cached directly
in the runtime, short-circuiting even the requirements that the request be handled by Fine in Nock.

***source:*** *`~wicdev-wisryt`*\
***context:*** *NONE*\
***location:*** *TODO*

### What is `tiny.hoon'?

`tiny.hoon` is the standard library used by the ship implementing the naive rollup smart contract code.

***source:*** *`~ritpub-sipsyl`*\
***context:*** *NONE*\
***location:*** *TODO*
