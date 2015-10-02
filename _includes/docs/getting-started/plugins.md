{% include top.html %}

<a name="plugins"></a>

## Plugins: actions on candlerecords 

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

If you want to use a plugin then you need to enabled it in the config file, see the `OANDAd.cfg`:

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

{% include top.html %}

### Publish/Subscribe - plugin

This plugin can be configured to `publish` the candlerecord using a publisher/subscriber mechanism. This is achieved by using the `0MQ` library and the python binding for it: pyzmq, see
 [http://zeromq.org](http://zeromq.org) for details.
Trading applications can easily subscribe to receive the candle data with just a few lines
of code, see [pubsub](/pubsub/) for details.

This plugin is enabled by default and publishes on 127.0.0.1, localhost. If you want
to subscribe with clients from other hosts, reconfigure the `pubsub` plugin by changing
the `pubsub.cfg` file and change the IP-address to the address of your OTE-host.

{% include top.html %}

### Plainfile - plugin

This plugin can be configured to write candle records to a flatfile in a directory structure,
which will result in a structure like below:

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


{% include top.html %}

### MySQL - plugin

The `MySQL` plugin can be configured to insert records into a database. This plugin is provided as an example, since it needs details that depend on your
databasemodel.

{% include top.html %}

## Trading applications as ZMQ client

Write your trading applications by making use of the `ZMQ` pub/sub facility of `OANDAd`.
This way you can completely isolate your trading code from the `OANDAd` daemon. It 
also enables you to
write applications in a different language then Python, the only requirement is
that is must support ZMQ.
See, [example](/pubsub/) for a ZMQ subscription example. 
