---
layout: article
permalink: /about/
---

About
===================================

If you've ever traded financial instruments, you probably found that it is hard to pay attention to a larger number of instruments you would like follow. 
Automated trading tools can make a difference. There are lots of such tools on the market. With markets open 24/7 from sunday till friday, you can monitor instruments on these markets full-time. You want to be notified when certain events take place ?, or act on it automatically ? automated trading tools facilitate this.

OANDA REST-API
----------------

In 2014 OANDA released it's REST-API. It gives full access to the OANDA system. This enables a user with programming skills to create his own tooling in the most flexible way. OANDA provides support for the API for a number of programming languageges. For Python there is a wrapper for the REST-API named _oandapy_. It can be found on github: [oandapy](https://github.com/oanda/oandapy). 

Developing tradingtools
-----------------------------

Mostly, tradingsystems perform calculations and charting  based on [Timestamp, Open, High, Low, Close, Volume] records from a certain time granularity. Although the OANDA API provides this data, it is not streaming and you would end-up making a request every time you granularity period has completed.

It would be nice to have a source for pricedata that provides us with the
 _candle records_, but still allows us access to the data as OANDA provides.


### Scalability

The _OANDA Trading Environment_ provides a daemon, **OANDAd**, to process the 
tick-stream and fabricate these OHLC-records. It
publishes the records right at the moment the time-period is completed or
during the processing of the timeframe as _dancing-bear_ records. This is
a benefit for the short timeframes. It also allows you to cache these records,
store them in a database for charting / calculations.

You can write multiple applications all subscribing for the same data. The
subscription takes only a few lines of code.

###  ZMQ support in other languages 

Although the _OANDA Trading Environment_ is written in Python, ZMQ support
in other languages makes it possible to make use of the _pub/sub_ facility
in those languages aswell. So, you can program your tools in the programming language of your choice and still make use of the _OANDA trading environment_.

