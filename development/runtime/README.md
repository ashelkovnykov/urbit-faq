# Runtime

This section contains information about runtime (Vere) development.

## Contents

[Errors: `bail`ing on an inner road](#errors-bailing-on-an-inner-road) \
[Errors: How are deterministic errors handled?](#errors-how-are-deterministic-errors-handled) \
[Errors: How are non-deterministic errors handled?](#errors-how-are-non-deterministic-errors-handled) \
[Errors: Why do some deterministic error cases emit non-deterministic errors?](#errors-why-do-some-deterministic-error-cases-emit-non-deterministic-errors) \
[Hints: Is it desirable to have hint hooks that can be called before the dynamic hint formula is computed?](#hints-is-it-desirable-to-have-hint-hooks-that-can-be-called-before-the-dynamic-hint-formula-is-computed) \
[Hints: What should be done with malformed or unrecognized hints?](#hints-what-should-be-done-with-malformed-or-unrecognized-hints) \
[How does Vere track that a ship is "fake"?](#how-does-vere-track-that-a-ship-is-fake) \
[Jets: allocating in a jet](#jets-allocating-in-a-jet) \
[Jets: `bail` error code usage](#jets-bail-error-code-usage) \
[Jets: `lth`, `gth`, and derived code](#jets-lth-gth-and-derived-code) \ 
[Jets: Matching `?-` (wuthep) behaviour](#jets-matching---wuthep-behaviour) \
[Jets: When would a jet ever punt?](#jets-when-would-a-jet-ever-punt) \
[Known issues: Vere memory leak](#known-issues-vere-memory-leak) \
[Profiling: AddressSanitizer](#profiling-addresssanitizer) \
[Profiling: JSON Tracing](#profiling-json-tracing) \
[Profiling: Valgrind](#profiling-valgrind) \
[Testing: how should we test jets, since Vere and Arvo live in different repos?](#testing-how-should-we-test-jets-since-vere-and-arvo-live-in-different-repos) \
[What is the difference between the Vere interpreter and Vere daemon?](#what-is-the-difference-between-the-vere-interpreter-and-vere-daemon) \
[What is the directory structure of `urbit/pkg/urbit`?](#what-is-the-directory-structure-of-urbitpkgurbit) \
[What is the kelvin decrement process?](#what-is-the-kelvin-decrement-process)

### Errors: `bail`ing on an inner road

U3 allocations take the road into account, therefore it's safe to `bail` on an inner road at any time. The road will be
obliterated and all allocations safely deallocated. 

***source:*** *`~master-morzod`*\
***context:*** *TODO*\
***location:*** *TODO*

### Errors: How are deterministic errors handled?

When a deterministic error is encountered, the interpreter should perform clean-up tasks and then return to the previous
virtualization layer.

***source:*** *`~wicdev-wisryt`*\
***context:*** *TODO*\
***location:*** *TODO*

### Errors: How are non-deterministic errors handled?

In Vere, a non-deterministic error is handled by a `longjmp` back to the home road and the entire event being killed
(note that this is [safe to do at any time](#errors-bailing-on-an-inner-road)).

In Ares, a non-deterministic error is handled by killing the event thread and injecting the stack trace (note that this
is safe, so long as no heap-allocation has been performed).

***source:*** *`~wicdev-wisryt`, `~ritpub-sipsyl`* \
***context:*** *TODO*\
***location:*** *TODO*

### Errors: Why do some deterministic error cases emit non-deterministic errors?

Consider the case where a Nock formula attempts to create a list of `(2^64 + 1)` elements. There's nothing wrong with
this formula, but no generally available computer of our day would be able to represent this list because it wouldn't
be able to index it in the interpreter. Therefore, the failure is non-deterministic because the formula is correct but
an external constraint of which Arvo is unaware is preventing it from being computed.

***source:*** *`~finmep-lanteb`, `~wicdev-wisryt`*\
***context:*** *TODO*\
***location:*** *TODO*

### Hints: Is it desirable to have hint hooks that can be called before the dynamic hint formula is computed?

Yes, and they can even return shortcut results. However, the interpreter must still run the dynamic hint formula unless
it has run this exact code (i.e. dynamic hint formula, nock formula, and subject) successfully once before (or unless
it's trivially successful, e.g. Nock 1). Otherwise, an interpreter that knows how to handle the hint may incorrectly
skip malformed hint formulas, whereas an interpreter that doesn't will correctly crash.

***source:*** *`~wicdev-wisryt`*\
***context:*** *TODO*\
***location:*** *TODO*

### Hints: What should be done with malformed or unrecognized hints?

Formally, both malformed and unrecognized hints are ignored. It's possible that a hint in the Nock is not for the
particular interpreter that has encountered it. Just because the current interpreter doesn't recognize the hint, or
expects it to be formed differently, doesn't mean that all other interpreters do. Technically, the spec also allows the
interpreter to kill the event, but in practice it's much better to just ignore the hint.

There are two exceptions to the above:
1. A dynamic hint with a malformed hint Nock formula is invalid
2. A jet hint that produces an unhandled error in the jet is invalid

***source:*** *`~wicdev-wisryt`*\
***context:*** *TODO*\
***location:*** *TODO*

### How does Vere track that a ship is "fake"?

A fake bit is persisted in the LMDB "META" database. This can be checked by the `fak_o` member in a `u3_pier` object,
e.g. `c3y == pir_u->fak_o`.

***source:*** *`~master-morzod`*\
***context:*** *TODO*\
***location:*** *TODO*

### Jets: allocating in a jet

Allocation outside of the loom is prohibited while in a jet. Only the on-loom allocation functions `u3a_malloc`,
`u3a_realloc`, and `u3a_free` are okay. Vendored libraries that are used for jets either have functions that allocate
removed, or have their allocators replaced.

***source:*** *`~dinleb-rambep`, `~master-morzod`*\
***context:*** *TODO*\
***location:*** *TODO*

### Jets: `bail` error code usage

In a jet, `bail:exit` usage must exactly match the crash semantics of the Nock being jetted. All the other failures are
semantically safe at any time.

This requires understanding the crashes in the Nock specification, inspecting the Nock emitted by the code being jetted,
and verifying the behaviour via untyped calls (test code fragments using the `slum` gate from the stdlib).

***source:*** *`~master-morzod`*\
***context:*** *TODO*\
***location:*** *TODO*

### Jets: `lth`, `gth`, and derived code

```
++  lth
  ~/  %lth
  |=  [a=@ b=@]
  ^-  ?
  ?&  !=(a b)
      |-
      ?|  =(0 a)
          ?&  !=(0 b)
              $(a (dec a), b (dec b))
  ==  ==  ==
```

Despite `lth` specifying that the gate requires its input to be two atoms, the type system doesn't actually exist at the
Nock level. According to the Nock standard, the code should only fail when a Nock instruction attempts to use a cell as
an atom. Therefore, attempting to call the Nock equivalent of the `lth` code with cells as one or both of the inputs
should only fail at the instruction `$(a (dec a), b (dec b))`; resolving the Nock code for the decrement operation is
the first point at which point cell input will be treated as an atom.

However, this means that input where one of `a` or `b` is a cell, and the other is `0` should not bail: this is valid
input for the above code. Similarly, if `a` and `b` are both cells but are identical, this is also valid input.
Therefore, the jet for `lth` needs to take this into account, and does:
```
u3_noun
u3wa_lth(u3_noun cor)
{
  u3_noun a, b;

  if (  (c3n == u3r_mean(cor, u3x_sam_2, &a, u3x_sam_3, &b, 0))
     || (c3n == u3ud(b) && 0 != a)
     || (c3n == u3ud(a) && 0 != b) )
  {
    return u3m_bail(c3__exit);
  }
  else {
    return u3qa_lth(a, b);
  }
}
```

The above also applies to `gth`. Jets for code that use `lth` or `gth` indirectly also need to keep this in mind.
Examples include `lte`, `gte`, `min`, and `max`. 

***source:*** *`~master-morzod`, `~fodwyt-ragful`*\
***context:*** *TODO*\
***location:*** *TODO*

### Jets: Matching `?-` (wuthep) behaviour

When attempting to match `?-` behaviour in a jet using a `switch` statement, it's necessary to make sure that the jet
captures the exact behaviour of the rune. `?-` is basically syntactic sugar for a sequence of `?:` runes with conditions
and matching expressions that ends in a `~|(%mint-lost !!)` expression if none of the conditionals were met.

The simplest way to do so is to include a `default` case which calls `u3m_bail(c3__fail)`.

***source:*** *`~master-morzod`*\
***context:*** *NONE* \
***location:*** *TODO*

### Jets: When would a jet ever punt?

It's perfectly acceptable for a jet to only operate on a subset of a function's domain. Therefore, if it encountered
input that it was unable to jet, it would punt back to Nock to evaluate the solution using the naive approach.

Note that it is always safe to punt from within a jet. 

***source:*** *`~wicdev-wisryt`*\
***context:*** *TODO*\
***location:*** *TODO*

### Known issues: Vere memory leak

There is currently one known memory leak that occurs in Vere: when attempting to recover from an exception generated in
the signal handler while on the home road. In practice, the particular issue that gets reported is `recover top: meme`,
but that's not important: any exception generated in the signal handler on the home road will trigger the leak.

***source:*** *`~master-morzod`*\
***context:*** *NONE* \
***location:*** *TODO*

### Profiling: AddressSanitizer

AddressSanitizer (ASan) is a tool for detecting various memory bugs in C/C++ code, such as buffer overflows, dangling
pointers, and reading beyond allocation. Since the only current major implementation of Vere is written in C, it can be
useful for identifying issues with the Urbit source code.

ASan is built into the `clang` and `gcc` compilers. ASan is enabled during compilation for a binary by adding the
`-fsanitize=address` flag to the build **and** linker flags. This used to be simple using the old dynamic Nix build
system, but in the new static Bazel system it's a massive issue that has yet to be resolved. However, there
[is a workaround](https://github.com/urbit/vere/issues/351) that allows ASan to be enabled.

***source:*** *`~finmep-lanteb`*\
***context:***  *TODO*\
***location:*** *TODO*

### Profiling: JSON Tracing

Urbit is setup to create Chromium-tracing style JSON reports for profiling the wall-clock time spent in each function
(an example is pictured below).

![JSON Tracing](https://raw.githubusercontent.com/ashelkovnykov/urbit-faq/master/development/runtime/resources/jtrace.png 'Urbit JSON Tracing Example')

To enable JSON tracing reports, launch a ship with the `-j` option (e.g. `./urbit -j zod`). After you're done running
the code that you want profiled, shut down the Urbit instance. The produced report will be located in the
`.urb/put/trace` directory of the pier of the ship (e.g. `piers/zod/.urb/put/trace/0.json`). This report can be opened
from the tracing interface (`chrome://tracing`) of any Chromium-based browser (Google Chrome, Brave, Vivaldi, etc.).

***source:*** *`~master-morzod`, `~ritpub-sipsyl`*\
***context:*** https://www.chromium.org/developers/how-tos/trace-event-profiling-tool/ \
***location:*** *TODO*

### Profiling: Valgrind

Valgrind is a profiling tool for C/C++ debugging and memory leak detection. Since the only current major implementation
of Vere is written in C, it can be useful for identifying issues with the Urbit source code.

To run Vere with Valgrind requires the following process:
1. Install Valgrind
2. Make changes to Vere source
3. Drop `siz_i` on line `896` in `urbit/pkg/urbit/vere/disk.c` to `0x780000000`
4. Rebuild the Urbit binaries

#### Callgrind

To run the Callgrind tool, run the following command to launch your ship:
```shell
valgrind --tool=callgrind --dump-instr=yes --simulate-cache=yes --collect-jumps=yes --log-file=callgrind.log ./urbit piers/zod/
```
The Callgrind report produced must be visualized with another tool, most commonly
[kcachegrind](https://kcachegrind.github.io/html/Home.html).

Alternatively, to reduce the size of the report produced, you can recompile the binaries with manual calls to the
instrumentation initialization and enable/disable macros:
```
CALLGRIND_START_INSTRUMENTATION
CALLGRIND_TOGGLE_COLLECT
```
Then, the shell command to launch your ship would instead be:
```shell
valgrind --tool=callgrind --dump-instr=yes --simulate-cache=yes --collect-jumps=yes --instr-atstart=no --collect-atstart=no --log-file=callgrind.log ./urbit piers/zod/
```
Note that to do this, you will need to include the Callgrind macros in the Vere source, by adding the following files to
the project: [valgrind.h](https://sourceware.org/git/?p=valgrind.git;a=blob_plain;f=include/valgrind.h.in;hb=HEAD) and
[callgrind.h](https://sourceware.org/git/?p=valgrind.git;a=blob_plain;f=callgrind/callgrind.h;hb=HEAD).

#### Memcheck

To run the Memcheck tool, run the following command to launch your ship:
```shell
valgrind --tool=memcheck --leak-check=yes --track-origins=yes --trace-children=yes --log-file=memcheck.log ./urbit piers/zod/
```

***source:*** *`~finmep-lanteb`, `~master-morzod`*\
***context:*** https://valgrind.org \
***location:*** *TODO*

### Testing: how should we test jets, since Vere and Arvo live in different repos?

The jets tests run in both repos, under CPU_DEBUG and MEMORY_DEBUG. Both repos can refer to the specific version of the
other repo that it wants. In `urbit/urbit`, the `vere-version` file specifies the binary against which it should be
tested. In `urbit/vere`, the `solid_pill`, `ivory_pill`, and `urbit` settings in `WORKSPACE.bazel` specify the version
of the kernel against which it should be tested.

***source:*** *`~wicdev-wisryt`*\
***context:*** https://github.com/urbit/urbit, https://github.com/urbit/vere \
***location:*** *TODO*

### What is the difference between the Vere interpreter and Vere daemon?

Abstractly, an Urbit runtime needs to do 6 things:
1. represent nouns
2. persist arvo state
3. compute Nock
4. accelerate Nock
5. source events
6. persist events

The Vere interpreter (also known as "worker" or "serf") does 1-4, whereas the Vere daemon (also known as "king") does
all 6.

There was a bunch of planned modularity for Vere that never materialized. The roles of the two processes are going to
change in an upcoming update, and eventually the two binaries will likely be combined.

***source:*** *`~master-morzod`*\
***context:*** https://github.com/urbit/urbit/pull/5596 \
***location:*** *TODO*

### What is the directory structure of `urbit/pkg/urbit`?

```
bench:   basic benchmarks (currently just de/serialization)
daemon:  entrypoint to the daemon/king process
include: all headers
jets:    all jets
noun:    loom, noun representation and apis, checkpoints, nock/bytecode interpreter, jet dashboard
tests:   unit tests
ur:      off-loom noun representation and de/serialization
vere:    event scheduling, i/o drivers, event persistence, daemon/king implementation
worker:  entrypoint and implementation of the worker/serf process

king = daemon + include + jets + noun + ur + vere
serf = worker + include + jets + noun + ur
```

***source:*** *`~master-morzod`*\
***context:*** https://github.com/urbit/urbit/tree/master/pkg/urbit \
***location:*** *TODO*

### What is the kelvin decrement process?

For the purposes of example, we'll assume that we're decrementing the `hoon.hoon` from kelvin `10` to kelvin `9`.

Note that any time a kelvin is decremented, all subordinate kelvins must also be decremented. The order is: `nock` ->
`hoon` -> `arvo` -> `lull` -> `zuse`. Therefore, a decrement to the Nock kelvin would decrement all of the kelvins,
whereas a decrement to the `lull.hoon` kelvin would only additionally decrement the `zuse.hoon` kelvin.

1. Copy jets for the new kelvin in `pkg/noun/jets/tree.c`, if decrementing Hoon kelvin
    1. Practically, this means creating a new entry in `_d[]` for the new kelvin, which should look similar to the one
        above it: `{ "k9", 0, 0, _k9_d, no_hashes, 0, (u3j_core*) 9, 0 }`
    2. It's unlikely that many of the jets changed between kelvins, so keep as much of the existing state as possible.
        For example, if only the jet for `dec` changed, then you would only need to define new `u3j_core` objects for
        `_k9_d` and `_9_one_d`:
        1. ```u3j_core _k9_d[] = { { "one", 3, 0, _139_one_d, no_hashes }, {} };```
        2.  ```
            static u3j_core _139_one_d[] = {
              { "two", 3, 0, _10_two_d, no_hashes },
              { "add", 7, _10_one_add_a, 0, no_hashes },
              { "dec", 7, _9_one_add_a,  0, no_hashes },
              { "div", 7, _10_one_add_a, 0, no_hashes },
              ...
              {}
            };
            ```
    3. Remove any old jets code from `pks/noun/jets/tree.c` (e.g. for kelvin `11` or higher)
2. Decrement the kelvin version(s) in `pkg/vere/pier.c`, object `u3_noun kel`
3. Decrement the Hoon kelvin version in `pkg/vere/serf.c`, if necessary
4. Compile the new binary
5. Check that the current Arvo still works on the binary
   1. The binary must support Arvo both before and after the update
6. Decrement the kelvin version(s) in `hoon.hoon`, `arvo.hoon`, `lull.hoon`, and `zuse.hoon` - whichever are necessary
7. Compile a new ivory pill
8. Test the new ivory pill on the new binary
9. Upload the new ivory pill

***source:*** *`~finmep-lanteb`, `~wicdev-wisryt`*\
***context:*** *TODO* \
***location:*** *TODO*
