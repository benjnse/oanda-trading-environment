---
layout: article
title: "OANDA Trading Environment: getting started"
permalink: /getting-started/
---

Using the OANDA REST-API to produce records in a configurable way that suits your
needs for providing data for your trading applications and enable you to
auto-trade via the REST-API.

<a id="top"></a>

{% include toc.html %}

OANDA Trading Environment: the picture
======================================

![alt text]({{site.baseurl}}/images/blockdiagram-oanda-te.svg)

The `OANDAd` daemon of the OANDA Trading Environment serves as a local quoteserver generating
OHLC-records / _candle_ records for different timeframes. It uses the OANDA
REST-API to get streaming quotes by using the _oandapy_ API wrapper.

Client applications
can subscribe for quotes via a `ZMQ subscription`. The **OANDAd** will provide
these applications continuously with `candle-records` by `publishing` these
records. 

The daemon provides the concept of `plugins`. This allows pieces of Python code
te be run as integrated code when the daemon has produced a record. Plugins are 
used to _publish_
records, `store records` in flat file stucture or store records in a database.
See [plugins](#plugins) for details.

{% include top.html %}

Streaming Candles
-----------------

Main part of the OANDA Trading Environment is the `OANDAd` daemon that 
parses the streaming quotes from the `OANDA-API` in 
configurable timeframes, by 1 minute, 5 minutes, 15 minutes etc. This makes it
produce streaming candles.
This is mostly useful for the shorter timeframes.
The larger timeframes can be requested using the API.

Candle data:

{% highlight json %}

      {"data": {"instrument": "EUR_JPY",
                "granularity" : "M1",
                "start": "2015-09-02 15:36:00"
                "end": "2015-09-02 15:37:00",
                "completed": true,
                "data": {"high": 134.967, 
                         "open": 134.962,
                         "last": 134.9565,
                          "low": 134.9475,
                       "volume": 19
                 },
               }
       }
{% endhighlight %}


Streaming data can be controlled by the `fabricate` setting in the `streamer` 
config section of `OANDAd.cfg`. See [controlling record types](#controlling_record_types) for details.

### Dataformat

The dataformat as shown can be easily altered. With a few lines of code in the plugin
you are free to change the format and add attributes before you publish the record.


{% include docs/getting-started/plugins.md %}
{% include docs/getting-started/security.md %}
{% include docs/getting-started/installing-ote.md %}
{% include docs/getting-started/configuring-oandad.md %}
{% include docs/getting-started/controlling-oandad.md %}
{% include docs/getting-started/controlling-recordtypes.md %}
{% include docs/getting-started/logging.md %}
