# Logix - Send your logs from Syslog to Graylog2 via AMQP
* http://graylog2.org/about

## Usage:
### Setup your AMQP and Graylog2
* http://www.rabbitmq.com/getstarted.html
* https://github.com/Graylog2/graylog2-server/wiki/AMQP

### config file
    edit the config file, setup port and transport

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
    $ LOGIX_CONF=src/etc/logix.conf src/bin/logix &
    $ logger test

## Depends:
* python-kombu (>= 1.4.3)
* python-gevent (>= 0.13.6) 
* python-supay
* syslog or rsyslog :D

## Todo
* needs tweak on amqp pool
* would benefit of an internal backlog queue

## to test
LOGIX_CONF=../etc/logix.conf python logix --action nodaemon


