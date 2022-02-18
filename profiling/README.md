# Profiling

This section contains information about profiling changes to Urbit.

## Contents

[AddressSanitizer](#addresssanitizer) \
[JSON Tracing](#json-tracing) \
[Valgrind](#valgrind)

### AddressSanitizer

AddressSanitizer (ASan) is a tool for detecting various memory bugs in C/C++ code, such as buffer overflows, dangling
pointers, and reading beyond allocation. Since the only current major implementation of Vere is written in C, it can be
useful for identifying issues with the Urbit source code.

ASan is built into the `clang` and `gcc` compilers. ASan is enabled during compilation for a binary by adding the
`-fsanitize=address` flag to the build **and** linker flags. For Urbit, this means adding the following lines after line
182 in `urbit/pkg/urbit/configure`:
```
CFLAGS="${CFLAGS-} -fsanitize=address"
LDFLAGS="${LDFLAGS-} -fsanitize=address"
```
In addition, the tests in `urbit/pkg/urbit/Makefile` need to be disabled, or else the build will fail due to
LeakSanitizer errors during testing.

***source:*** *`~finmep-lanteb`*\
***context:***  *TODO*\
***location:*** *TODO*\
***documented:*** *TODO*

### JSON Tracing

Urbit is setup to create Chromium-tracing style JSON reports for profiling the wall-clock time spent in each function
(an example is pictured below).

![JSON Tracing](https://raw.githubusercontent.com/ashelkovnykov/urbit-faq/master/profiling/resources/jtrace.png 'Urbit JSON Tracing Example')

To enable JSON tracing reports, launch a ship with the `-j` option (e.g. `./urbit -j zod`). After you're done running
the code that you want profiled, shut down the Urbit instance. The produced report will be located in the
`.urb/put/trace` directory of the pier of the ship (e.g. `piers/zod/.urb/put/trace/0.json`). This report can be opened
from the tracing interface (`chrome://tracing`) of any Chromium-based browser (Google Chrome, Brave, Vivaldi, etc.).

***source:*** *`~master-morzod`, `~ritpub-sipsyl`*\
***context:*** https://www.chromium.org/developers/how-tos/trace-event-profiling-tool/ \
***location:*** *TODO*\
***documented:*** *TODO*

### Valgrind

Valgrind is a profiling tool for C/C++ debugging and memory leak detection. Since the only current major implementation
of Vere is written in C, it can be useful for identifying issues with the Urbit source code.

To run Vere with Valgrind requires the following process:
1. Install Valgrind
2. Make changes to Vere source
3. Drop `siz_i` on line 754 in `urbit/pkg/urbit/vere/disk.c` to `32212254720`
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
valgrind --tool=memcheck --leak-check=yes --track-origins=yes --log-file=memcheck.log ./urbit piers/zod/
```

***source:*** *`~finmep-lanteb`, `~master-morzod`*\
***context:*** https://valgrind.org \
***location:*** *TODO*\
***documented:*** *TODO*
