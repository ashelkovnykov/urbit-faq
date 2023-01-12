# Web

This section contains information about operational tricks using the web interface.

## Contents

[Read chat/groups messages as JSON using HTTP scries](#read-chatgroups-messages-as-json-using-http-scries)

### Read chat/groups messages as JSON using HTTP scries 

You can use the Eyre interface to scry chat messages as JSON using the following URL:

```
[your urbit hostname]/~/scry/chat/chat/[group host]/[channel]/writs/newest[num].json
```

The above URL will return the `num` newest messages from the specified chat. More concrete examples below:

```
https://finmep.ashelkov.com/~/scry/chat/chat/~walbex/chat-3138/writs/newest/20.json
http://localhost:8080/~/scry/chat/chat/~darrux-landes/general/writs/newest/10.json
```

***source:*** *`~litmus-ritten`*\
***context:*** *TODO* \
***location:*** *TODO*
