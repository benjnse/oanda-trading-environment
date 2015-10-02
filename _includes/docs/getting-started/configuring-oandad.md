{% include top.html %}

## Configuring OANDAd

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
