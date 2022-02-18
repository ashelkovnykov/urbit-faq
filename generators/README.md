# Generators

This section contains information about Urbit generators.

## Contents

[Passing tasks to `arvo` from a generator](#passing-tasks-to-arvo-from-a-generator) \
[Purpose of a generator](#purpose-of-a-generator)

### Passing tasks to `arvo` from a generator

Generators are purely stateless functions which take an argument and produce a result, without side-effects. Therefore,
they cannot pass tasks to `arvo`.

***source:*** *`~hastuc-dibtux`, `~tinnus-napbus`*\
***context:*** *TODO*\
***location:*** *TODO*\
***documented:*** *TODO*

### Purpose of a generator

Generators are pure functions designed to be invoked from the dojo (and sometimes `eyre`). They're particularly useful
for building nouns to be poked into apps.

***source:*** *`~tinnus-napbus`*\
***context:*** *TODO*\
***location:*** *TODO*\
***documented:*** *TODO*
