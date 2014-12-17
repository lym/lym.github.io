---
layout: post
title: "Install monit on Ubuntu box"
description: ""
category: Dev Ops
tags: [Dev Ops, Tools]
---
{% include JB/setup %}

# Installing [monit](http://mmonit.com/monit/) on an Ubuntu box

- Update the repositories
    $ sudo apt-get update

- Install monit
    $ sudo apt-get install monit

- Copy configuration file from `/etc/monit/monitrc` to `~/.monitrc`
    $ cp /etc/monit/monitrc ~/.monitrc

- Change the owner to yourself, just owner, leave permissions alone
    $ chown [your use name] .monitrc

- Start monit
    $ monit

- Point your web browser to `https:/localhost:2812`

- Bask in the glory that is your fresh install
