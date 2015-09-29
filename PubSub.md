---
layout: article
permalink: /pubsub/
title: "Pub/Sub"
description: "Publish / Subscribe using ZMQ"
category: 
tags: []
---

Making use of the, by default, enabled plugin: _pubsub_.

### Programming language independent

One way to get standard _candle-records_ in your application is by subscribing
for data in your application. The _OANDAd_ daemon _pubsub_ plugin publishes data
via ZMQ.

ZMQ has support for a lot of languages, check [ZeroMQ](http://zeromq.org). Use of ZMQ in the _pubsub_ plugin
makes it possible to create trading applications in different programming languages.


Example:

{% highlight python %}

import zmq
import json

context = zmq.Context()
socket = context.socket(zmq.SUB)
socket.connect("tcp://127.0.0.1:5550")
socket.setsockopt(zmq.SUBSCRIBE, "")

socket.setsockopt(zmq.RCVBUF, 1000)
while True:
    msg = socket.recv()
    rec = json.loads(msg)
    print json.dumps(rec, indent=4, sort_keys=True)

{% endhighlight %}

With the OANDAd configured :

{% highlight yaml %}
# etc/OANDA/config/OANDAd.cfg
...
streamer:
  fabricate: dancingBearHighLowExtreme
...

{% endhighlight %}

### Resulting output 

OANDAd will publish _completed_ and _dancing bear_ records, as below. The first
3 records with granularity M1, M5 and M15 are all completed at the same time, at 07:45. The new records for the same granularities are all _dancing bear_ records until 
the first granularity M1 gets completed first again at 07:46.

{% highlight json %}

{
    "data": {
        "completed": true, 
        "data": {
            "high": 10170.75, 
            "last": 10170.75, 
            "low": 10164.75, 
            "open": 10168.3, 
            "volume": 103
        }, 
        "end": "2015-09-15 07:45:00", 
        "granularity": "M1", 
        "instrument": "DE30_EUR", 
        "start": "2015-09-15 07:44:00"
    }
}
{
    "data": {
        "completed": true, 
        "data": {
            "high": 10175.25, 
            "last": 10170.75, 
            "low": 10162.75, 
            "open": 10173.25, 
            "volume": 564
        }, 
        "end": "2015-09-15 07:45:00", 
        "granularity": "M5", 
        "instrument": "DE30_EUR", 
        "start": "2015-09-15 07:40:00"
    }
}
{
    "data": {
        "completed": true, 
        "data": {
            "high": 10177.25, 
            "last": 10170.75, 
            "low": 10162.75, 
            "open": 10174.0, 
            "volume": 723
        }, 
        "end": "2015-09-15 07:45:00", 
        "granularity": "M15", 
        "instrument": "DE30_EUR", 
        "start": "2015-09-15 07:30:00"
    }
}
{
    "data": {
        "completed": false, 
        "data": {
            "high": 10170.75, 
            "last": 10170.75, 
            "low": 10170.5, 
            "open": 10170.5, 
            "volume": 2
        }, 
        "end": "2015-09-15 07:46:00", 
        "granularity": "M1", 
        "instrument": "DE30_EUR", 
        "start": "2015-09-15 07:45:00"
    }
}
{
    "data": {
        "completed": false, 
        "data": {
            "high": 10170.75, 
            "last": 10170.75, 
            "low": 10170.5, 
            "open": 10170.5, 
            "volume": 2
        }, 
        "end": "2015-09-15 07:50:00", 
        "granularity": "M5", 
        "instrument": "DE30_EUR", 
        "start": "2015-09-15 07:45:00"
    }
}
{
    "data": {
        "completed": false, 
        "data": {
            "high": 10170.75, 
            "last": 10170.75, 
            "low": 10170.5, 
            "open": 10170.5, 
            "volume": 2
        }, 
        "end": "2015-09-15 08:00:00", 
        "granularity": "M15", 
        "instrument": "DE30_EUR", 
        "start": "2015-09-15 07:45:00"
    }
}
{
    "data": {
        "completed": false, 
        "data": {
            "high": 10170.95, 
            "last": 10170.95, 
            "low": 10170.5, 
            "open": 10170.5, 
            "volume": 3
        }, 
        "end": "2015-09-15 07:46:00", 
        "granularity": "M1", 
        "instrument": "DE30_EUR", 
        "start": "2015-09-15 07:45:00"
    }
}
{
    "data": {
        "completed": false, 
        "data": {
            "high": 10170.95, 
            "last": 10170.95, 
            "low": 10170.5, 
            "open": 10170.5, 
            "volume": 3
        }, 
        "end": "2015-09-15 07:50:00", 
        "granularity": "M5", 
        "instrument": "DE30_EUR", 
        "start": "2015-09-15 07:45:00"
    }
}
{
    "data": {
        "completed": false, 
        "data": {
            "high": 10170.95, 
            "last": 10170.95, 
            "low": 10170.5, 
            "open": 10170.5, 
            "volume": 3
        }, 
        "end": "2015-09-15 08:00:00", 
        "granularity": "M15", 
        "instrument": "DE30_EUR", 
        "start": "2015-09-15 07:45:00"
    }
}
{
    "data": {
        "completed": false, 
        "data": {
            "high": 10171.2, 
            "last": 10171.2, 
            "low": 10170.5, 
            "open": 10170.5, 
            "volume": 4
        }, 
        "end": "2015-09-15 07:46:00", 
        "granularity": "M1", 
        "instrument": "DE30_EUR", 
        "start": "2015-09-15 07:45:00"
    }
}
{
    "data": {
        "completed": false, 
        "data": {
            "high": 10171.2, 
            "last": 10171.2, 
            "low": 10170.5, 
            "open": 10170.5, 
            "volume": 4
        }, 
        "end": "2015-09-15 07:50:00", 
        "granularity": "M5", 
        "instrument": "DE30_EUR", 
        "start": "2015-09-15 07:45:00"
    }
}
{
    "data": {
        "completed": false, 
        "data": {
            "high": 10171.2, 
            "last": 10171.2, 
            "low": 10170.5, 
            "open": 10170.5, 
            "volume": 4
        }, 
        "end": "2015-09-15 08:00:00", 
        "granularity": "M15", 
        "instrument": "DE30_EUR", 
        "start": "2015-09-15 07:45:00"
    }
}
{
    "data": {
        "completed": false, 
        "data": {
            "high": 10171.25, 
            "last": 10171.25, 
            "low": 10170.5, 
            "open": 10170.5, 
            "volume": 5
        }, 
        "end": "2015-09-15 07:46:00", 
        "granularity": "M1", 
        "instrument": "DE30_EUR", 
        "start": "2015-09-15 07:45:00"
    }
}
{
    "data": {
        "completed": false, 
        "data": {
            "high": 10171.25, 
            "last": 10171.25, 
            "low": 10170.5, 
            "open": 10170.5, 
            "volume": 5
        }, 
        "end": "2015-09-15 07:50:00", 
        "granularity": "M5", 
        "instrument": "DE30_EUR", 
        "start": "2015-09-15 07:45:00"
    }
}
{
    "data": {
        "completed": false, 
        "data": {
            "high": 10171.25, 
            "last": 10171.25, 
            "low": 10170.5, 
            "open": 10170.5, 
            "volume": 5
        }, 
        "end": "2015-09-15 08:00:00", 
        "granularity": "M15", 
        "instrument": "DE30_EUR", 
        "start": "2015-09-15 07:45:00"
    }
}
{% endhighlight %}

#### Other messaging ?

If you don't like ZeroMQ you can easily write your own plugin on another messaging library.
Check the _plugin_ directory for examples.


