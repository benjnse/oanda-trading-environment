{% include top.html %}

# Requirements

* Python 2.7.3 
* oandapy
* pyyaml > 3.10
* daemonocle >= 0.8
* pyzmq >= 14.7

# Installation

Currently there are two ways to install: **pip** and **git**.

{% include top.html %}

## Using **Git** ...

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

{% include top.html %}

## Using **pip** and the latest _pypi-package_

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
