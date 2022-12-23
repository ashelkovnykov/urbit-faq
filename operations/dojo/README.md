# Dojo

This section contains information about using the Dojo (the Urbit ship terminal).

## Contents

[Commands: calling threads from another desk](#commands-calling-threads-from-another-desk) \
[Commands: poke an agent on another ship from dojo](#commands-poke-an-agent-on-another-ship-from-dojo) \
[Commands: what do the different command symbols mean?](#commands-what-do-the-different-command-symbols-mean) \
[Error: dojo stuck on `%dy-no-prompt`](#error-dojo-stuck-on-dy-no-prompt)

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
