# Install and Config rsyslog server/client

## 1. Sending logs from client to server

#### server side configs

Open main config file and enable port 514 for UDP and 5514 for TCP connections
```
$ sudo vim /etc/rsyslog.conf
```
Uncomment following lines,
```
module(load="imudp")
input(type="imudp" port="514")

module(load="imtcp")
input(type="imtcp" port="5514")
```
Resstart the service
```
$ sudo systemctl restart syslog
```

#### client side configs

Open main config file and add following configs,

```
$ sudo vim /etc/rsyslog.conf

*.* @@syslogserverIPorDNS:TCPport   # For TCP
*.* @syslogserverIPorDNS:UDPport    # For UDP
```

Restart the service
```
$ sudo systemctl restart syslog
```














