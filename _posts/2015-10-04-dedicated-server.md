---
layout: article
title: "Dedicated Server on Raspberry Pi"
description: ""
image:
  teaser: raspberrypi-ansible-logo-transp-400x250.png
categories: articles
#comment: true
comment: true
share: true
tags: [OANDA python REST API real-time trading]
---

Use `Ansible` and install the OANDA Trading Environment in no-time on a [Raspberry Pi](https://www.raspberrypi.org) and have a dedicated, `low cost`, `low power` server 24/7 available. 

<img style="float: left;" src="{{site.baseurl}}/images/raspberrypi-transp.png">

Running the `OANDA-trading-environment` on as Raspberrypi works like a charm. This small single-board computer comes with 1G of RAM and a quad-core ARM CPU. Running the _OANDA Trading Environment_ it has a load almost never exceeding 0.10. 

-------

[<img style="float: right;" src="{{site.baseurl}}/images/ansible-logo-transp.png">](http://www.ansible.com)

This document describes the full procedure to get the OANDA Trading Environment
installed on a Raspberry Pi. The installation procedure is automated for the biggest part
using `Ansible`. The Ansible playbook to install OANDA Trading Environment can be downloaded
from Github. For details see the installation procedure.


{% include toc.html %}

{% include docs/raspberry/raspi.md %}


