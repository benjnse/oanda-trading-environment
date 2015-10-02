{% include top.html %}

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
