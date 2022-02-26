# Hosting

This section contains information about hosting ships.

## Contents

[Configuring SSL/TLS for remote host](#configuring-ssltls-for-remote-host) \
[Hosting on a Raspberry Pi](#hosting-on-a-raspberry-pi) \
[Hosting on a VPS](#hosting-on-a-vps) \
[Monitoring uptime of ships](#monitoring-uptime-of-ships)

### Configuring SSL/TLS for remote host

See the guide linked below for details.

***source:*** *`~silsyn-wathep` `~sitful-hatred` `~tinnus-napbus`*\
***context:*** *NONE*\
***location:*** \
https://urbit.org/using/os/basics#dns-setup \
https://subject.network/posts/caddyserver-urbit-tls/ \
***documented:*** *yes*

### Hosting on a Raspberry Pi

A Raspberry Pi would work for hosting a star, depending on what else is running on the Pi, competing with Urbit for
RAM and SD reader access. Most likely, you would want a Pi with 4GB+ RAM and an SSD attached by USB.

***source:*** *`~monted-tallex`*\
***context:*** https://botter-nidnul.github.io/Steps_to_Urbit_on_Raspberry_Pi.html \
***location:*** *TODO*\
***documented:*** *TODO*

### Hosting on a VPS

A GitHub repo containing easily-configurable files for automatically setting up a VPS environment to host one or more
Urbit ships (and, optionally, one or more websites) is available
[here](https://github.com/ashelkovnykov/urbit-hosting).

***source:*** *`~datder-sonnet`, `~finmep-lanteb`*\
***context:*** *NONE*\
***location:*** *TODO*\
***documented:*** *TODO*

### Monitoring uptime of ships

Using the script available [here](https://github.com/mrdomino/urbit-sysops/blob/master/sbin/urbit-exporter), you can
set up [Prometheus](https://prometheus.io/) to monitor any number of running Urbit instances. Prometheus is a re-write
of Borgmon, a Google-internal, time-series monitoring system.

***source:*** *`~silsyn-wathep`*\
***context:*** *NONE*\
***location:*** *TODO*\
***documented:*** *TODO*
