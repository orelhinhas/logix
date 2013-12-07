# Logix - Transports your local syslog to Graylog2 via AMQP dd
* http://graylog2.org/about

## Why should I use it?

When you are sending lots of udp log events over the network packet loss can happen, or even
using a tcp log sender you can get a slow response on your server depending on how much logs
your remote log server is receiving simultaneously.

So... what can you do to avoid it?

logix can help you using its daemon receiving your log events
and queueing you messages on AMQP. You can easily get rid of log event
losses caused by udp and any performance issue that could be caused by
concurrency using tcp remote syslog.

logix queues your log events on any AMQP Server and you can easy setup
your <a href="https://github.com/Graylog2/graylog2-server">graylog2-server</a> to consume this queue and index your logs on demand.

<img src="https://raw.github.com/ncode/logix/master/logix.png">

## Usage:
### Setup your AMQP and Graylog2
* http://www.rabbitmq.com/getstarted.html
* https://github.com/Graylog2/graylog2-server/wiki/AMQP

### Add to your graylog2.conf

    # AMQP
    amqp_enabled = true
    amqp_subscribed_queues = logix:gelf
    amqp_host = localhost
    amqp_port = 5672
    amqp_username = guest
    amqp_password = guest
    amqp_virtualhost = /

### logix.conf

    [transport]
    connection_pool_enabled = False
    connection_pool_size = 10
    url = amqp://127.0.0.1:5672
    queue = logix

    [server]
    port = 6660
    max_syslog_line_size = 1023
    bind_addr = 127.0.0.1

### on MacOS X:

    $ vim /etc/syslog.conf
    *.notice;authpriv,remoteauth,ftp,install,internal.none  @127.0.0.1:6660
    $ launchctl unload /System/Library/LaunchDaemons/com.apple.syslogd.plist
    $ launchctl load /System/Library/LaunchDaemons/com.apple.syslogd.plist

### on Linux:

    $ vim /etc/rsyslog.d/logix.conf
    *.*  @127.0.0.1:6660
    $ /etc/init.d/rsyslog restart

### Running:

    $ Usage: ./logix
    $   -h help
    $   -u username
    $   -d debug
    $   -a <start|stop|status|foreground>

    $ LOGIX_CONF=src/etc/logix.conf src/bin/logix -u $USER -a foreground -d &
    $ logger test

## Depends:
* python-kombu (>= 1.4.3)
* python-gevent (>= 0.13.6)
* python-supay
* syslog or rsyslog :D

## Todo
* would benefit of an internal backlog queue
