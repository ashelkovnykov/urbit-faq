# Generators

This section contains information about Vere, the Nock interpreter.

## Contents

[Jets: `bail` error code usage](#jets-bail-error-code-usage) \
[What is the difference between the Vere interpreter and Vere daemon?](#what-is-the-difference-between-the-vere-interpreter-and-vere-daemon) \
[What is the directory structure of `urbit/pkg/urbit`?](#what-is-the-directory-structure-of-urbitpkgurbit) \
[What is Vere?](#what-is-vere)

### Jets: `bail` error code usage

In a jet, `bail:exit` usage must exactly match the crash semantics of the Nock being jetted. All the other failures are
semantically safe at any time.

***source:*** *`~master-morzod`*\
***context:*** *TODO*\
***location:*** *TODO*\
***documented:*** *TODO*

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
***context:*** *TODO*\
***location:*** *TODO*\
***documented:*** *TODO*

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
***location:*** *TODO*\
***documented:*** *TODO*

### What is Vere?

"Vere" is currently a floating signifier that could mean one of three things:
1. The Urbit runtime, abstractly
2. A particular instance of the Urbit runtime, including both the daemon and worker ("king" and "serf")
   processes
3. A particular instance of the daemon process

***source:*** *`~master-morzod`*\
***context:*** *TODO*\
***location:*** *TODO*\
***documented:*** *TODO*
