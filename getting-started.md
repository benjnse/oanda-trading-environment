---
layout: article
title: Getting Started with OANDA Trading Environment
permalink: /getting-started/
---

* auto-gen TOC:
{:toc}

Using the OANDA REST-API to produce records in a configurable way that suit your
needs for providing data for your trading applications and enable you to
auto-trade via the REST-API.

OANDA Trading Environment: the picture
--------------------------------------------

![alt text]({{site.baseurl}}/images/blockdiagram-oanda-te.svg)

The daemon of the OANDA Trading Environment serves as a local quoteserver generating
OHLC-records / _candle_ records for different timeframes. It uses the OANDA
REST-API to get streaming quotes by using the _oandapy_ API wrapper.

Client applications
can _subscribe_ for quotes via a ZMQ subscription. The **OANDAd** will provide
these applications continuously with _candle-records_ by _publishing_ these
records.

The daemon provides the concept of plugins. This allows pieces of Python code
te be run when the daemon has produced a record. Plugins are used to _publish_
records, store records in flat file stucture or store records in a database.


Streaming Candles
-----------------

Main part of the OANDA Trading Environment is the **OANDAd** daemon that 
parses the streaming quotes in 
configurable timeframes, by 1 minute, 5 minutes, 15 minutes etc. This makes it
produce streaming candles.

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

The larger timeframes can be requested using the API.

Streaming data can be controlled by the 'fabricate' setting in the 'streamer:' 
config section. See [controlling record types](#controlling_record_types) for details.

Actions / Plugins
------------------

When a timeframe is completed it can be handled by one or more plugins.
Plugins have a configfile based on the name of the plugin-file, but in lowercase.
The plugins can be found under:

{% highlight bash %}
 etc/OANDA/plugins
{% endhighlight %}

and the plugin configs under 

{% highlight bash %}
etc/OANDA/config/plugins
{% endhighlight %}

If you want to use a plugin then you need to enabled it in the config file, see the:

{% highlight bash %}
# etc/OANDA/config/OANDAd.cfg
...
plugins:
  ...
  enabled:
    - plainfile
    - pubsub

{% endhighlight %}


The environment comes with a few plugins:

### Publish/Subscribe - plugin

This plugin can be configured to 'publish' the candle using a publisher/subscriber mechanism. This is achieved by using the [0MQ](http://zeromq.org)
library and the python binding for it: pyzmq

Other trading applications can easily subscribe to receive the candle data. See [here](#zmq_example) for a ZMQ subscription example.

This plugin is enabled by default.

### Plainfile - plugin

This plugin can be configured to write candle records to a flatfile in a directory structure. 

Example:

{% highlight bash %}

     /tmp/oandadb
     |-- BCO_USD
     |   |-- M1
     |   |   `-- cache
     |   |-- M15
     |   |   `-- cache
     |   `-- M5
     |       `-- cache
     |-- DE30_EUR
     |   |-- M1
     |   |   `-- cache
     |   |-- M15
     |   |   `-- cache
     |   `-- M5
     |       `-- cache
     |-- EUR_CHF
     |   |-- M1
     |   |   `-- cache
     |   |-- M15
     |   |   `-- cache
     |   `-- M5
     |       `-- cache
     |-- EUR_GBP
     |   |-- M1
     |   |   `-- cache
     |   |-- M15
     |   |   `-- cache
     |   `-- M5
     |       `-- cache
     |-- EUR_JPY
     |   |-- M1
     |   |   `-- cache
     |   |-- M15
     |   |   `-- cache
     |   `-- M5
     |       `-- cache
     |-- EUR_USD
         |-- M1
         |   `-- cache
         |-- M15
         |   `-- cache
         `-- M5
             `-- cache

{% endhighlight %}


### MySQL - plugin

The MySQL plugin can be configured to insert records into a database. This plugin is provided as an example, since it needs details that depend on your
databasemodel.

Auto trading
-------------

### By using a ZMQ client

The desired approach is to create stand-alone applications that subscribe for quotes,
see, [example](#zmq_example) for a ZMQ subscription example.

### By using plugins

Though it is possible to use the plugin facility to perform auto-trading, the
way to go is to use ZMQ client and subscribe for quotes. This way you can
completely isolate your trading code from the OANDAd daemon.



## Prerequisites

Before installing the OANDA Trading Enviroment you need to have:

* a _token_ to use the OANDA REST-API. So please check for that at [developer.oanda.com](https://developer.oanda.com). Tokens are needed to access the **practice** account aswell as the **live** environment.
* an account_id, check the OANDA-webinterface for that after you've registered, or when you have the token you could create/list the accounts for your token.

The account_id is needed to configure the OANDAd daemon to get streaming quotes.

## Security

The environment makes use of a token that gives access to crucial information.

Please pay attention to where you install this software. **Never** use this on
a system that is not owned and controlled by yourself.

Make sure to secure your system as much as possible:

* restrict network access
* make no use of, or limit other network services (NFS, SAMBA, printerserver etc.)
* limit or deny user access, preferrable only you
* use encryption to access the system (SSH)
* how is physical access ?

## Installation

Currently there are two ways to install: **pip** and **git**.

### Using **Git** ...

{% highlight bash %}

user $ mkdir <somewhere>
user $ cd <somewhere>
user $ mkdir OANDA
user $ virtualenv OANDA_env

# optionally use virtualenv --system-site-packages to use the standard
# available packages for the python modules available on your distribution:
#   _pyyaml_, _pyzmq_. Check for the packages on the distribution.

user $ . ./OANDA_env/bin/activate
user $ git clone https://github.com/hootnot/oanda-trading-environment.git
user $ cd oanda-trading-environment
user $ python setup.py install
{% endhighlight %}

OANDA has not (yet) made the _oandapy_ module **pip** installable, either from git or pypi.python.org.

To install the latest _oandapy_ you can use this fork to install it with **pip**:

{% highlight bash %}

user $ pip install git+https://github.com/hootnot/oandapy

user $ pip list | grep oanda
oanda-trading-environment (0.0.1)
oandapy (0.1)

{% endhighlight %}

### Using **pip** and the latest _pypi-package_

{% highlight bash %}

user $ mkdir <somewhere>
user $ cd <somewhere>
user $ mkdir OANDA
user $ virtualenv [--system-site-packages] OANDA_env

# optionally use virtualenv --system-site-packages to use the standard
# available packages for the python modules available on your distribution:
#   _pyyaml_, _pyzmq_. Check for the packages on the distribution.

user $ . ./OANDA_env/bin/activate
user $ pip install oanda-trading-environment
user $ pip install git+https://github.com/hootnot/oandapy

{% endhighlight %}

Using a systemwide install:

{% highlight bash %}
user $ sudo pip install oanda-trading-environment
user $ sudo pip install git+https://github.com/hootnot/oandapy

{% endhighlight %}

### Configuring OANDAd

By default OANDAd is set with a number of instruments to follow for different
granularities. 

For the initial configuaration you only need to enter the _token_ and _account_id_.

Edit the config file and replace the placeholders:

{% highlight bash %}
# etc/OANDA/config/OANDAd.cfg
...
access_token: _token_from_oanda_here
account_id: _accountid_here_
...
{% endhighlight %}


## Controlling OANDAd

OANDAd is built using [daemonocle](https://github.com/jnrbsn/daemonocle). The 
'start', 'status' and 'stop' commands are implemented. The daemon forks itself
and the child will process the stream. When there are issues, TIME-OUT for instance,
the child will exit and a new child will be spawned.

#### Starting the OANDAd

{% highlight bash %}
user $ OANDAd start
Starting OANDAd ... OK
{% endhighlight %}

The daemon will process streaming quotes now and process the timeframes as
 configured. Timeframes are currently based on the midprice of bid/ask.
During processing _candle_ records will be produced. 
The way timeframes will be processed to produce _candle_ records can be controlled.

#### Status of the OANDAd

{% highlight bash %}
user $ OANDAd status
OANDAd -- pid: 51931, status: sleeping, uptime: 0m, %cpu: 0.0, %mem: 1.8
{% endhighlight %}

#### Stopping the OANDAd

{% highlight bash %}
user $ OANDAd stop
Stopping OANDAd ... OK
{% endhighlight %}


## Controlling recordtypes

<a name="controlling_record_types"></a>

Records can be produced in different ways by changing the _fabricate_ setting of
the _streamer_:

{% highlight bash %}
streamer:
  ...
  fabricate: atEndOfTimeFrame|dancingBear|dancingBearHighLowExtreme

{% endhighlight %}

### _atEndOfTimeFrame_

The setting generating least number of records is: _atEndOfTimeFrame_. A record
is only created when the timeperiod of the timeframe has passed.

### _dancingBear_

This setting generates a record for each tick it processes. Mostly a lot and in 
volatile markets a lot more.

### _dancingBearHighLowExtreme_

This setting generates also _dancingBear_ records but only when a new high or a
new low is set for the timeframe. When a market is volatile you still get a lot 
of records because it is likely that a lot of ticks will lead to new highs or lows
within the timeframe.

But in less volatile markets you will have a lot of ticks that will stay between
the high and low set at a certain moment. Therefore this will generate a lot less
records but still give you the relevant details at the moment they occur.

If you want _dancingBear_ records consider this option instead of the
_dancingBear_ option.


## Logging

The ticks received from the stream are written to a logfile:

{% highlight bash %}
 <installation_root>/var/log/OANDA/streamdata.<date>
{% endhighlight %}

The OANDAd itself logs to:
{% highlight bash %}
 <installation_root>/var/log/OANDA/OANDAd.log
{% endhighlight %}

Loglevel and the streamdata logfile extension is configurable. Check the _OANDAd.cfg_
for details.

