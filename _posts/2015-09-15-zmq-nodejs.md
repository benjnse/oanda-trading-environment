---
layout: article
title: "Subscription with nodejs to OANDAd"
description: ""
image:
  teaser: nodejs-400x250.png
categories: articles
comment: true
tags: []
---

Example to demonstrate language indepency to OANDAd.

Nodejs subscription for pricedata
-----------------------------------

Example subscriber in **nodejs**:

{% highlight js %}
// ZMQ subscription example using node.js 
// 
var zmq = require('zmq')
   , sock = zmq.socket('sub');
    
    sock.connect('tcp://127.0.0.1:5550');
    sock.subscribe('');
    console.log('Subscriber connected to port 5550');
     
    sock.on('message', function(topic, message) {
    console.log('received:', topic.toString());
});
{% endhighlight %}

## Output

{% highlight json %}

{
    "data": {
        "completed": true, 
        "data": {
            "high": 1.578515, 
            "last": 1.5779100000000001, 
            "low": 1.577855, 
            "open": 1.578515, 
            "volume": 17
        }, 
        "end": "2015-09-15 19:22:00", 
        "granularity": "M1", 
        "instrument": "EUR_AUD", 
        "start": "2015-09-15 19:21:00"
    }
}
{
    "data": {
        "completed": true, 
        "data": {
            "high": 1.1272350000000002, 
            "last": 1.1268150000000001, 
            "low": 1.1268150000000001, 
            "open": 1.127205, 
            "volume": 23
        }, 
        "end": "2015-09-15 19:22:00", 
        "granularity": "M1", 
        "instrument": "EUR_USD", 
        "start": "2015-09-15 19:21:00"
    }
}
{
    "data": {
        "completed": false, 
        "data": {
            "high": 135.71300000000002, 
            "last": 135.71300000000002, 
            "low": 135.709, 
            "open": 135.709, 
            "volume": 2
        }, 
        "end": "2015-09-15 19:23:00", 
        "granularity": "M1", 
        "instrument": "EUR_JPY", 
        "start": "2015-09-15 19:22:00"
    }
}
{
    "data": {
        "completed": true, 
        "data": {
            "high": 0.7349600000000001, 
            "last": 0.734755, 
            "low": 0.73473, 
            "open": 0.7349600000000001, 
            "volume": 12
        }, 
        "end": "2015-09-15 19:22:00", 
        "granularity": "M1", 
        "instrument": "EUR_GBP", 
        "start": "2015-09-15 19:21:00"
    }
}
{
    "data": {
        "completed": false, 
        "data": {
            "high": 1.1268799999999999, 
            "last": 1.1268799999999999, 
            "low": 1.12684, 
            "open": 1.12684, 
            "volume": 2
        }, 
        "end": "2015-09-15 19:23:00", 
        "granularity": "M1", 
        "instrument": "EUR_USD", 
        "start": "2015-09-15 19:22:00"
    }
}
{
    "data": {
        "completed": false, 
        "data": {
            "high": 1.5780150000000002, 
            "last": 1.5780150000000002, 
            "low": 1.577965, 
            "open": 1.577965, 
            "volume": 2
        }, 
        "end": "2015-09-15 19:23:00", 
        "granularity": "M1", 
        "instrument": "EUR_AUD", 
        "start": "2015-09-15 19:22:00"
    }
}
{% endhighlight %}

## Notes

The are some differences in the implemention of the ZMQ bindings.

The Python binding does:

{% highlight python %}

socket.send("%s %s" % (topic, msg))

{% endhighlight %}

Regardless the datatypes and formatting it always sends a string.

When we take a look at the _Nodejs_ bindings, we see that it allows sending
an array "[ topic, message ]". The subscriber also uses these arguments.

Therefore do we see the message from the OANDAd daemon appear in
the _topic_ variable of the _handler_ function. The _message_ variable itself
is **undefined**.

If implementing _topics_ you probably do have to find a solution in splitting
the message. Same kind of handling the way the Python binding does.

Since it is for a demo purpose I did not further look into this.

