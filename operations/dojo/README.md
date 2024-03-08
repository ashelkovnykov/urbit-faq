# Dojo

This section contains information about using the Dojo (the Urbit ship terminal).

## Contents

[Command: call thread from custom desk](#command-call-thread-from-custom-desk) \
[Command: check if ship is live or fake](#command-check-if-ship-is-live-or-fake) \
[Command: crash the pretty printer](#command-crash-the-pretty-printer) \
[Command: get docket for application](#command-get-docket-for-application) \
[Command: get latest hash of desk](#commands-get-latest-hash-of-desk) \
[Command: poke an agent on another ship from dojo](#commands-poke-an-agent-on-another-ship-from-dojo) \
[Command: print jammed noun to dojo](#commands-print-jammed-noun-to-dojo) \
[Command: replace a broken vane on a ship]() \
[Command: what do the different command symbols mean?](#commands-what-do-the-different-command-symbols-mean) \
[Error: dojo stuck on `%dy-no-prompt`](#error-dojo-stuck-on-dy-no-prompt) \
[Error: "leak" messages in dojo after Vere crash](#error-leak-messages-in-dojo-after-vere-crash) \
[What does `|meld` do?](#what-does-meld-do) \
[What does `|pack` do?](#what-does-pack-do) \
[What does the output of `|mass` mean?](#what-does-the-output-of-mass-mean) \
[What is the "base" hash in `+vats`?](#what-is-the-base-hash-in-vats) \
[What is the "%cz" hash in `+vats`?](#what-is-the-cz-hash-in-vats)

### Command: call thread from custom desk

To call a thread `-foo` from the desk `%bar`, use the following command in the dojo:
```
> -bar!foo
```

***source:*** *`~tinnus-napbus`*\
***context:*** *TODO*\
***location:*** *TODO*

### Command: check if ship is live or fake

The following scry will return true (a.k.a. `%.y` a.k.a. `0`) if a ship is fake (i.e. using fake network keys):
```
.^(? %j /=fake=)
```

***source:*** *`~tinnus-napbus`*\
***context:*** *TODO*\
***location:*** *TODO*

### Command: crash the pretty printer

- `!>(sign-arvo)`
- `|.(1)`
- Generally, printing any type or trap will work

***source:*** *`~sidnym-ladrut`, `~wispem-wantex`*\
***context:*** *TODO*\
***location:*** *TODO*

### Command: get docket for application

For an application local to your own ship:
```
=docket -build-file /=landscape=/sur/docket/hoon
=file -read %q [ship] [desk] [revision] /desk/docket-0
(docket.docket file)
```

***source:*** *`~dinleb-rambep`*\
***context:*** *TODO*\
***location:*** *TODO*

### Command: get latest hash of desk

Example - get the latest hash of `%app` on ship `~sampel-palnet`:
```
-read %z ~sampel-palnet %app %da now /
```

***source:*** *`~midden-fabler`*\
***context:*** *TODO*\
***location:*** *TODO*

### Command: poke an agent on another ship from dojo

You can poke an agent on another ship from your dojo using the following format:
`:[SHIP]/[AGENT] [MARK] [VASE]`

Ex: `:~bacdun/faucet &faucet-action [%open 0x0 0xdead.beef]`

***source:*** *`~hodzod-walrus`, `rabsef-bicrym`*\
***context:*** *NONE*\
***location:*** *TODO*

### Command: print jammed noun to dojo

1. Save the jammed noun to a file
2. Commit the file to a desk in a ship
3. Load the file using Clay: `=j .^(@ %cx %/tang/jam)`
4. `cue` the loaded data: `(cue j)`
    1. Might need to skip over the first 5 bytes if it's a newt-encoded jammed noun: `(cue (rsh [3 5] j))`

***source:*** *`~wicdev-wisryt`*\
***context:*** *NONE*\
***location:*** *TODO*

### Command: replace a broken vane on a ship

0. Shutdown ship (assuming it's bootable and running)
1. Generate replacement vane code
2. Boot a fake ship
3. Create a new desk in the fake ship
``` 
|new-desk %sandbox
```
4. Mount the new desk to the external file system
```
|mount %sandbox
```
5. Copy new vane code to the sandbox desk
```
cp /path/to/new-vane.hoon /path/to/fake/ship/sandbox/
```
6. Commit the changes to the fake ship
```
|commit %sandbox
```
7. Create a jam file of the ovum used to replace the existing vane with the new code
```
.new-vane/jam [[%$ %arvo ~] %what [[%sys %vane %whatever ~] [%hoon .^(cord %cx /=sandbox=/new-vane/hoon)]] ~]
```
8. Inject the ovum into the ship event log
```
/path/to/broken/ship/.run -I /path/to/fake/ship/.urb/put/new-vane.jam
```
OR
```
/path/to/urbit/binary -I /path/to/fake/ship/.urb/put/new-vane.jam /path/to/broken/ship
```

***source:*** *`~finmep-lanteb`, `~master-morzod`*\
***context:*** *NONE*\
***location:*** *TODO*

### Command: what do the different command symbols mean?

```
+foo 'some' 'args'         :: run %/gen/foo.hoon with args
+foo!bar 'some' 'args'     :: run /=foo=/gen/bar.hoon with args
|foo 'some' 'args'         :: poke %hood with output of %/gen/hood/foo.hoon with args
:foo|bar 'some' 'args'     :: poke %foo with output of %/gen/bar.hoon with args
:foo ['some' 'args']       :: poke %foo with args as a %noun mark
:foo &bar ['some' 'args']  :: poke %foo with args as a %bar mark
:~zod/foo ['some' 'args']  :: poke %foo on ~zod with args as a %noun mark
-foo 'some' 'args'         :: run thread %foo with args
-foo!bar 'some' 'args'     :: run thread %bar in desk %foo with args
&foo 'arg'                 :: cast arg to mark foo (in %/mar/foo.hoon) from noun
```

***source:*** *`~tinnus-napbus`, `~watter-parter`, `~wicdev-wisryt`*\
***context:*** *NONE*\
***location:*** *TODO*

### Error: dojo stuck on `%dy-no-prompt`

It's possible for the dojo (terminal) to get stuck on `%dy-no-prompt`, most likely due to an issue with a thread. If it
does, pressing any key will result in a `%dy-edit-busy` message. `Ctrl+C` or forcefully stopping the ship then rebooting
will do nothing.

Disconnect the dojo from the thread by pressing `backspace` or `Ctrl+H`. Then, run `:spider|kill` in the dojo to stop
whichever thread was responsible. Note that this command will kill all currently-running threads. Unfortunately, while
there is a way to list the IDs of all currently-running threads (`:spider/tree`), there is no way to tie those IDs to a
particular thread.

***source:*** *`~finmep-lanteb`, `~tinnus-napbus`*\
***context:*** *NONE*\
***location:*** https://urbit.org/using/os/shell#troubleshooting

### Error: "leak" messages in dojo after Vere crash

This error usually appears as a list of messages like these:
```
2024-02-06T21:39:12.119737301Z leak: 0x2c1caa190 mug=0 swept=1
2024-02-06T21:39:12.119737301Z    size: B/24
2024-02-06T21:39:12.119737301Z    data: [%spot [%sys %vane %ames %hoon 0] [3422 13] 3422 39]
```

These errors are caused by a `bail:meme` (OOM error) while copying results from a Nock computation to the home road.
Running `pack` and `meld` commands then restarting should resolve the error on next boot.

***source:*** *`~master-morzod`*\
***context:*** *NONE*\
***location:*** *TODO*

### What does `|meld` do?

`|meld` is a memory deduplication routine.

Just like `|pack`, `|meld` will reduce the `free lists` down to 0. In addition, it will also attempt to reduce `sweep`
by finding identical nouns and replacing the newer one with a reference to the older one.

***source:*** *`~wicdev-wisryt`*\
***context:*** *[What does the output of `|mass` mean?](#what-does-the-output-of-mass-mean)*\
***location:*** https://operators.urbit.org/manual/os/dojo-tools#meld

### What does `|pack` do?

`|pack` is a memory defragmentation routine.

It reduces the `free lists` down to 0 (though they'll quickly fill up to a few KB just from calling `|mass` to confirm
this).

***source:*** *`~wicdev-wisryt`*\
***context:*** *[What does the output of `|mass` mean?](#what-does-the-output-of-mass-mean)*\
***location:*** https://operators.urbit.org/manual/os/dojo-tools#pack

### What does the output of `|mass` mean?

```
~zod:dojo> |mass
.....(truncated for brevity).....
total road stuff: B/376
space profile: KB/12.936
total marked: MB/176.019.156
free lists: MB/35.927.760
sweep: MB/176.019.156
```

`sweep` is the total amount of memory used. `free lists` is the total amount of fragmentation. If you add those two
together, you get a number for which there is no name, but the difference between that number and the loom size is how
much contiguous free memory your ship has available in its loom. This number is important because every event must be
run in the contiguous free space of your ship's loom. The memory in the `free lists` can only be used at the end of the
event, when the results of the event are being saved.

The rest of the `|mass` output is used for drilling down to determine what exactly is using a ship's memory.

***source:*** *`~wicdev-wisryt`*\
***context:*** *NONE*\
***location:*** https://operators.urbit.org/manual/os/dojo-tools#mass

### What is the "base" hash in `+vats`?

The "base" hash is a `tako`, a commit hash representing the merge-base of the local desk and its remote source. A commit
hash is a concatenation of two 128-bit truncated SHA-256 hashes of a jammed noun of the head-tagged commit contents
(see `+make-yaki` in `lull`).

***source:*** *`~master-morzod`*\
***context:*** *NONE*\
***location:*** *TODO*

### What is the "%cz" hash in `+vats`?

The "%cz" hash is a Merkle-like hash of the contents of the desk (see `+content-hash` in `clay`).

***source:*** *`~master-morzod`*\
***context:*** *NONE*\
***location:*** *https://developers.urbit.org/reference/arvo/clay/scry#z---content-hash*
