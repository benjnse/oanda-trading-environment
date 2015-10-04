

# Setting it up

Follow the steps on [https://www.raspberrypi.org](https://www.raspberrypi.org)
to install the `raspbian wheezy` linux on it.

### SSH pre-enabled

**Optionally** you can pre-enable SSH. This allows you to configure the
Raspberry Pi remotely at the firsttime boot. You can do this if you
are on a linux system where you prepare the SD-card. To do so
perform the following:

{% highlight bash %}
# chdir to the root-directory of the raspbian-wheezy image
cd <your-SD-card-mountpoint>
for i in 2 3 4 5
do 
  ln -s etc/init.d/ssh etc/rc$i.d/S02ssh
done
{% endhighlight %}

When you have the SD-card with the image installed, boot
the Raspberry Pi and it will show a 'first-time' boot screen:


* choose :  1 Expand Filesystem
* choose :  2 Change Password
* choose :  4 
  * I2 : Timezone
  * I3 : Keyboard
* choose : 8 
  * A2 : set hostname
  * A4 : enable SSH

Setup the networking with settings reflecting your network. Probably fine
by default due to DHCP.

... reboot and you will be prompted to log in. First we want to upgrade the OS and 
install some extra packages needed to use `Ansible`.


{% highlight bash %}
# suppose you named it 'ote' ...

pi@ote ~ $ 

# update your system (takes a while)
pi@ote ~ $ sudo apt-get update
pi@ote ~ $ sudo apt-get upgrade
# upgrade takes approx. 5 minutes 

# now we need to install some packages to setup python
pi@ote ~ $ sudo apt-get install python-pip
pi@ote ~ $ sudo apt-get install python-dev

{% endhighlight %}

With `pip` installed we fetch the latest Ansible package. It requires `gcc` which is
installed by default on Rasbian Wheezy.

Ansible allows us to install the OANDA Trading Environment completely automatically with
all dependencies. In the `deployment branch` of the `oanda-trading-environment` repository
on Github, there is an Ansible playbook to achieve this. So ...lets install `Ansible`:

{% highlight bash %}
# install Ansible to take care of the rest of the installation
# this will take about a minute
pi@ote ~ $ sudo pip install ansible
...
Successfully installed ansible paramiko jinja2 PyYAML pycrypto ecdsa MarkupSafe
Cleaning up...

{% endhighlight %}


{% include top.html %}

# Iptables


There are some issues regarding IP-tables on the raspbian_wheezy image.
Ideally these packages would be part of the Ansible deployment procedure,
but due to the installation issues we will
install these packages manually. You will receive
the `error message` as shown below. Somehow the kernel is not initialized
correctly.
A simple reboot at this point will fix the issue.

{% highlight bash %}
pi@ote ~ $ sudo apt-get install iptables
pi@ote ~ $ sudo apt-get install iptables-persistent
...
Setting up iptables-persistent (0.5.7) ...
libkmod: ERROR ../libkmod/libkmod.c:554 kmod_search_moddep: could not open moddep file '/lib/modules/3.18.11-v7+/modules.dep.bin'
iptables v1.4.14: can't initialize iptables table `filter': Table does not exist (do you need to insmod?)
Perhaps iptables or your kernel needs to be upgraded.
IPv4: Unable to save (table filter isn't available or module not loadable)
Loading iptables rules... skipping IPv4 (no rules to load)... skipping IPv6 (no rules to load)...done.
{% endhighlight %}


{% include top.html %}

# Ansible

With Ansible installed, we can simply run the `playbook` to install
the OANDA Trading Environment. The playbook to install the `OANDA-trading-environment` can be downloaded from Github. See instructions below.

The Ansible playbook takes care of :

* creating local user account for the environment
* installing packages
* configuring of iptables
* building ZMQ from source
* installing oanda-trading-environment
* install oandapy
* configure sudo rules
* enabling start of OANDAd at boot-time

Get the Ansible playbook from Github:

{% highlight bash %}

# fetch the deployment branch from the repository ...
pi@ote ~ $ 
pi@ote ~ $ git clone -b deployment https://github.com/hootnot/oanda-trading-environment.git deployment

pi@ote ~ $ cd deployment
pi@ote ~ $ ls   
# ... you will see a couple of directories holding `roles`, `handlers` and `group_vars`.
# and some files: a `hosts` file and the playbook `ote.yml`.
{% endhighlight %}

{% include top.html %}

## Install OANDA Trading Environment using Ansible

Lets install the OANDA Trading Environment with Ansible by running the playbook.
If for some reason the procedure falls into error you can `re-run` the playbook and
it will skip all the parts that are done already.

The complete procedure takes about `20 minutes` to complete. Below you find the logging 
of a playbook run / re-run.

{% highlight bash %}
# playbook file: ote.yml

# Now run the playbook to setup the oanda-trading-environment automatically
# You should see a logfile like below
# there are some timeconsuming parts (like compiling ZMQ Python) so please
# be patient
# if you want more verbose output: add one or more -v switches
#

pi@oate ~ $ sudo ansible-playbook -i hosts ote.yml 

PLAY [all] ******************************************************************** 

GATHERING FACTS *************************************************************** 
ok: [localhost]

TASK: [common | install common packages] ************************************** 
ok: [localhost] => (item=python-virtualenv,screen)

TASK: [common | getrules] ***************************************************** 
changed: [localhost]

TASK: [common | iptables rules] *********************************************** 
changed: [localhost] => (item={'name': 'all', 'rule': '-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT'})
changed: [localhost] => (item={'name': 'ssh', 'rule': '-A INPUT -p tcp -m tcp --dport 22 -m state --state NEW -j ACCEPT'})

PLAY [ote_servers] ************************************************************ 

GATHERING FACTS *************************************************************** 
ok: [localhost]

TASK: [ote | install required packages] *************************************** 
ok: [localhost] => (item=libffi-dev,autoconf,automake,libtool)

TASK: [ote | setup OANDA application group] *********************************** 
changed: [localhost]

TASK: [ote | setup OANDA application user] ************************************ 
changed: [localhost]

TASK: [ote | account permissions] ********************************************* 
changed: [localhost]

TASK: [ote | setup virtualenv for OTE under user oanda] *********************** 
changed: [localhost]

TASK: [ote | venv upgrade pip] ************************************************ 
changed: [localhost]

TASK: [ote | venv build pyzmq (takes a while!)] ******************************* 
changed: [localhost]

TASK: [ote | oandapy (from fork!)] ******************************************** 
changed: [localhost]

TASK: [ote | check for local package] ***************************************** 
ok: [localhost]

TASK: [ote | install from local OTE-package] ********************************** 
skipping: [localhost]

TASK: [ote | install OTE from pypi] ******************************************* 
changed: [localhost]

TASK: [ote | make venv enabled by .profile] *********************************** 
changed: [localhost]

TASK: [ote | enable oanda user to sudo] *************************************** 
ok: [localhost]

TASK: [ote | enable start of daemon at system boot] *************************** 
changed: [localhost]

TASK: [ote | getrules] ******************************************************** 
changed: [localhost]

TASK: [ote | iptables rules] ************************************************** 
changed: [localhost] => (item={'name': 'pubsub', 'rule': '-A INPUT -p tcp -m tcp --dport 5550 -m state --state NEW -j ACCEPT -m comment --comment "pubsub"'})

PLAY RECAP ******************************************************************** 
localhost                  : ok=20   changed=14   unreachable=0    failed=0 

{% endhighlight %}

{% include top.html %}

### Configure the OANDA Trading Environment

To make it operational you have to configure the `token` and `accountId`
in the configuationfile:

{% highlight bash %}
# this will take you to the application account
pi@ote ~ $ sudo su - oanda
(oanda)oanda@ote:~$

# now, with an editor of your choice, open the configfile
(oanda)oanda@ote:~$ vi etc/OANDA/config/OANDAd.cfg

# alter the lines ... and replace the values with your token and accountid
access_token:  ___here___
account_id:  ___here___
# by default the environment is set to 'practice' you may choose for 'live'
# optionally

# save the file

{% endhighlight %}

{% include top.html %}

### Start up and verify 

Below are the instructions to configure the environment. You can verify
if it is up-and-running by checking the `daemon status` and tailing the
`logfiles` _var/log/OANDA/OANDAd.log_ and the streamlogs _var/log/OANDA/streamlog..._

The playbook creates `sudo-rules` that allow you to control the `OANDAd` as user
`oanda`. See below for `OANDAd start`, `OANDAd status` and `OANDAd stop` commands.

{% highlight bash %}
# while your are still 'oanda' user or (become by: sudo su - oanda) ..
#
# start the daemon WITH sudo, this allows the OANDAd to write its pid file
# under /var/run/OANDA
(oanda)oanda@ote:~$ sudo /opt/oanda/bin/OANDAd start
Starting OANDAd ... OK

# verifying status
(oanda)oanda@ote:~$ sudo /opt/oanda/bin/OANDAd status
OANDAd -- pid: 19711, status: sleeping, uptime: 13m, %cpu: 0.0, %mem: 3.6

# verify ...
# this tail will show you the live tick records en heartbeat records
(oanda)oanda@ote:~$ tail -f var/log/OANDA/streamdata.20150927-17
{"bid": 9629.9, "mid": 9631.15, "value": 9631.15, "instrument": "DE30_EUR", "time": "2015-09-25T19:59:00.422000Z", "ask": 9632.4}
{"bid": 155.444, "mid": 155.458, "value": 155.458, "instrument": "DE10YB_EUR", "time": "2015-09-25T19:59:00.433774Z", "ask": 155.472}
{"bid": 1.59296, "mid": 1.59396, "value": 1.59396, "instrument": "EUR_AUD", "time": "2015-09-25T20:59:58.552672Z", "ask": 1.59496}
{"time": "2015-09-27T15:41:24.919810Z"}
{"time": "2015-09-27T15:41:26.923828Z"}

# stopping the daemon
(oanda)oanda@ote:~$ sudo /opt/oanda/bin/OANDAd stop
Stopping OANDAd ... OK

{% endhighlight %}

{% include top.html %}

## Virtualenv

The `oanda-trading-environment` is setup under the **oanda** account. This
account is setup as an application account.

The Ansibble-playbook sets up the virtual environment to build the latest ZMQ
library and python bindings for it. Within this virtualenv it also
upgrades the **pip** package to the latest. There is a major gap between
the standard and the latest.

Manipulating packages in the environment requires this is done as user **oanda**.

{% include top.html %}

## iptables

The iptables playbook part configures the iptables.
Check the `vars/main.yml` files in the `roles` directories. You may want to control
IP-tables by creating additonal rules in these config files.


{% include top.html %}

# Next steps ...

Either program your applications locally on the Raspberry Pi, or make use of
the `pub/sub` facility and use the Raspberry Pi as a quote server for
your (auto)trading applications. See [pub/sub]({{site.baseurl}}/pubsub/) for details.

{% include top.html %}

# Note: SD-card reliability

The Raspberry Pi runs from an SD-card.
My personal experience using the Raspberry Pi is that it is very reliable. In
case you experience issues, it may be caused by the SD-card.

I've had this issue with the first SD-card I used. Strange things happened
like spontaneous reboots, corrupt filesystem etc.

Installation on a new SD-card solved this.
