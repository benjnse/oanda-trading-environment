{% include top.html %}

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
