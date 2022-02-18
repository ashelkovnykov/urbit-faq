# Testing

This section contains information about testing changes to Urbit.

## Contents

[Do I need to compile a new pill when I make changes to the Urbit source code?](#do-i-need-to-compile-a-new-pill-when-i-make-changes-to-the-urbit-source-code) \
[How do I compile changes to my Urbit's source code?](#how-do-i-compile-changes-to-my-urbits-source-code) \
[How do I compile changes to the runtime source code?](#how-do-i-compile-changes-to-the-runtime-source-code) \
[How do I run unit tests?](#how-do-i-run-unit-tests) \
[How do I test that my `lib`/`sur` files are correct?](#how-do-i-test-that-my-libsur-files-are-correct)

### Do I need to compile a new pill when I make changes to the Urbit source code?

No, but depending on the size of the changes that you're making, to how many ships you need to apply the changes, and
with how many people you need to share the changes, it may make development much faster.

***source:*** *`~master-morzod`*\
***context:*** *TODO*\
***location:*** *TODO*\
***documented:*** *TODO*

### How do I compile changes to my Urbit's source code?

Once you've modified some Urbit source code files, you can test if they compile by applying the changes to your test
ship. To do so, launch your test ship and run the following set of commands:

```
In dojo:
~zod:dojo> |mount %base

Then in terminal:
user@linux:~$ cp -rfL path/to/source/arvo/zuse.hoon path/to/piers/zod/base/

Then in dojo:
~zod:dojo> |commit %base
```

Applications/agents that depend on the modified files will be recompiled using the new source code. It's possible that
an error may occur during this process (due to a bug in the new code). In this case, the underlying issue will need to
be fixed, and the above commands (excluding `|mount %base`) repeated.

Some changes (for example changes to particularly core code or changes to large chunks of the code in multiple desks)
may require much of the ship to be recompiled. For such critical/massive changes, the following set of commands is more
useful, as it will split the recompilation into multiple phases:

```
In dojo:
~zod:dojo> |mount %base
~zod:dojo> |mount %garden
~zod:dojo> |mount %landscape
~zod:dojo> |mount %bitcoin
~zod:dojo> |mount %webterm
~zod:dojo> 
~zod:dojo> |suspend %webterm
~zod:dojo> |suspend %bitcoin
~zod:dojo> |suspend %landscape
~zod:dojo> |suspend %garden

Then in terminal:
user@linux:~$ cp -rfL path/to/source/arvo/* path/to/piers/zod/base/
user@linux:~$ cp -rfL path/to/source/garden/* path/to/piers/zod/garden/
user@linux:~$ cp -rfL path/to/source/landscape/* path/to/piers/zod/landscape/
user@linux:~$ cp -rfL path/to/source/bitcoin/* path/to/piers/zod/bitcoin/
user@linux:~$ cp -rfL path/to/source/webterm/* path/to/piers/zod/webterm/

Then in dojo:
~zod:dojo> |commit %
~zod:dojo> 
~zod:dojo> |commit %garden
~zod:dojo> |revive %garden
~zod:dojo> 
~zod:dojo> |commit %landscape
~zod:dojo> |revive %landscape
~zod:dojo> 
~zod:dojo> |commit %bitcoin
~zod:dojo> |revive %bitcoin
~zod:dojo> 
~zod:dojo> |commit %webterm
~zod:dojo> |revive %webterm
~zod:dojo> 
~zod:dojo> |unmount %webterm
~zod:dojo> |unmount %bitcoin
~zod:dojo> |unmount %landscape
~zod:dojo> |unmount %garden
~zod:dojo> |unmount %base
```

***source:*** *`~finmep-lanteb`, `~master-morzod`, `~timluc-miptev`*\
***context:*** *TODO*\
***location:*** *TODO*\
***documented:*** *TODO*

### How do I compile changes to the runtime source code?

To test if changes to Vere (the Urbit runtime environment) compile, you need to perform the following steps:

1. Install [nix](https://nix.dev/tutorials/install-nix)
2. Run `make build` from the root of the Urbit source code directory
3. Launch your test ship with the newly compiled binaries:
```shell
user@linux:~$ chmod 770 /nix/store/[some hash]-urbit-[version]/urbit
user@linux:~$ /nix/store/[some hash]-urbit-[version]/urbit path/to/piers/zod
```

***source:*** *`~finmep-lanteb`*\
***context:*** *TODO*\
***location:*** *TODO*\
***documented:*** *TODO*

### How do I run unit tests?

The `test` thread accepts a `list` of paths within a desk, recursively builds all the files within those paths, and
calls all arms that start with "test". Examples:
```
> -test %/tests/sys/zuse ~
> -test %/tests/sys/zuse/html/hoon %/tests/sys/zuse/format/hoon ~
```

***source:*** *`~timluc-miptev`*\
***context:*** https://github.com/urbit/urbit/blob/master/pkg/arvo/tests/sys/zuse/html.hoon \
***location:*** *TODO*\
***documented:*** *TODO*

### How do I test that my `lib`/`sur` files are correct?

An easy way to check from inside an Urbit ship is to run the following commands:
```
> =l -build-file %/lib/my-file/hoon
> (my-gate:l 'some' 'args')
```

***source:*** *`~timluc-miptev`*\
***context:*** *NONE*\
***location:*** *TODO*\
***documented:*** *TODO*
