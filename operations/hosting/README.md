# Hosting

This section contains information about hosting ships.

## Contents

[Access web interface over SSH](#access-web-interface-over-ssh) \
[Event log: `chop` vs. `roll`](#event-log-chop-vs-roll) \
[Hosting on a Raspberry Pi](#hosting-on-a-raspberry-pi) \
[Hosting on a VPS](#hosting-on-a-vps) \
[Monitoring uptime of ships](#monitoring-uptime-of-ships)

### Access web interface over SSH

If you create a host definition for your planet in your `~/.ssh/config` like so:
```
Host urbit
Hostname <your server IP>
User urbit
Port 22
IdentityFile ~/.ssh/<your ssh key to the server>
LocalForward 9000 127.0.0.1:8080
ServerAliveInterval 60
```
then if you establish an SSH connection to the server hosting your ship, you can connect to `localhost:9000` in your
local browser to access the Landscape web interface of your ship over the SSH tunnel, completely bypassing HTTP.

***source:*** *`~matwel-mitreb`*\
***context:*** *NONE* \
***location:*** *TODO*

### Event log: `chop` vs. `roll`

`roll` creates a new event log "epoch", whereas `chop` creates a new event log epoch and then deletes all but the two
most recent epochs.

***source:*** *`~mastyr-bottec`*\
***context:*** https://github.com/urbit/vere/releases/tag/vere-v3.0 \
***location:*** https://docs.urbit.org/manual/running/vere#utilities

### Hosting on a Raspberry Pi

A Raspberry Pi would work for hosting a star, depending on what else is running on the Pi, competing with Urbit for
RAM and SD reader access. Most likely, you would want a Pi with 4GB+ RAM and an SSD attached by USB.

***source:*** *`~monted-tallex`*\
***context:*** https://botter-nidnul.github.io/Steps_to_Urbit_on_Raspberry_Pi.html \
***location:*** *TODO*

### Hosting on a VPS

A GitHub repo containing easily-configurable files for automatically setting up a VPS environment to host Urbit ships,
as well as (optionally) websites and MinIO is available
[here](https://github.com/ashelkovnykov/urbit-hosting).

***source:*** *`~datder-sonnet`, `~finmep-lanteb`*\
***context:*** *NONE*\
***location:*** *TODO*

### Monitoring uptime of ships

Using the script available [here](https://github.com/mrdomino/urbit-sysops/blob/master/sbin/urbit-exporter), you can
set up [Prometheus](https://prometheus.io/) to monitor any number of running Urbit instances. Prometheus is a re-write
of Borgmon, a Google-internal, time-series monitoring system.

***source:*** *`~silsyn-wathep`*\
***context:*** *NONE*\
***location:*** *TODO*
