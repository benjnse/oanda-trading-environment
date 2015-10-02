{% include top.html %}

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
