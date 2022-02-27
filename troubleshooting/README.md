# Troubleshooting

This section contains information about troubleshooting problems that may arise with your ship.

## Contents

[Dojo stuck on `%dy-no-prompt`](#dojo-stuck-on-dy-no-prompt)

### Dojo stuck on `%dy-no-prompt`

It's possible for the dojo (terminal) to get stuck on `%dy-no-prompt`, most likely due to an issue with a thread. If it
does, pressing any key will result in a `%dy-edit-busy` message. `Ctrl+C` or forcefully stopping the ship then rebooting
will do nothing.

Disconnect the dojo from the thread by pressing `backspace` or `Ctrl+H`. Then, run `:spider|kill` in the dojo to stop
whichever thread was responsible. Note that this command will kill all currently-running threads. Unfortunately, while
there is a way to list the IDs of all currently-running threads (`:spider/tree`), there is no way to tie those IDs to a
particular thread.

***source:*** *`~finmep-lanteb`, `~tinnus-napbus`*\
***context:*** *NONE*\
***location:*** https://urbit.org/using/os/shell#troubleshooting \
***documented:*** *partially*
