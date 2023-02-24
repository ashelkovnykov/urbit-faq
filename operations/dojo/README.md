# Dojo

This section contains information about using the Dojo (the Urbit ship terminal).

## Contents

[Commands: calling threads from another desk](#commands-calling-threads-from-another-desk) \
[Commands: poke an agent on another ship from dojo](#commands-poke-an-agent-on-another-ship-from-dojo) \
[Commands: what do the different command symbols mean?](#commands-what-do-the-different-command-symbols-mean) \
[Error: dojo stuck on `%dy-no-prompt`](#error-dojo-stuck-on-dy-no-prompt) \
[What does `|meld` do?](#what-does-meld-do) \
[What does `|pack` do?](#what-does-pack-do) \
[What does the output of `|mass` mean?](#what-does-the-output-of-mass-mean)

### Commands: calling threads from another desk

To call a thread `-foo` from the desk `%bar`, use the following command in the dojo:
```
> -bar!foo
```

***source:*** *`~tinnus-napbus`*\
***context:*** *TODO*\
***location:*** *TODO*

### Commands: poke an agent on another ship from dojo

You can poke an agent on another ship from your dojo using the following format:
`:[SHIP]/[AGENT] [MARK] [VASE]`

Ex: `:~bacdun/faucet &faucet-action [%open 0x0 0xdead.beef]`

***source:*** *`~hodzod-walrus`, `rabsef-bicrym`*\
***context:*** *NONE*\
***location:*** *TODO*

### Commands: what do the different command symbols mean?

```
+foo 'some' 'args'        ::  run %/gen/foo.hoon with args
|foo 'some' 'args'        ::  poke %hood with output of %/gen/hood/foo.hoon with args
:foo|bar 'some' 'args'    ::  poke %foo with output of %/gen/bar.hoon with args
:foo ['some' 'args']      ::  poke %foo with args as a %noun mark
:foo &bar ['some' 'args'] ::  poke %foo with args as a %bar mark
:~zod/foo ['some' 'args'] ::  poke %foo on ~zod with args as a %noun mark
-foo 'some' 'args'        ::  run thread %foo with args
-foo!bar 'some' 'args'    ::  run thread %bar in desk %foo with args
&foo 'arg'                ::  cast arg to mark foo (in %/mar/foo.hoon) from noun
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
