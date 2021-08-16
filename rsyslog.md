# Install and Config rsyslog server/client

### 1. Sending logs from client to server

#### server side configs

Enable UFW ports
```
$ sudo ufw allow 514/udp
$ sudo ufw allow 5514/tcp
```

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

### 2. Templating and create separate log path

#### server side configs

Open the main config file and add the following configs [ after the Global Directives, otherwise it wont work ]

```
$ sudo vim /etc/rsyslog.conf

$template RemoteLogs,"/var/log/RemoteHosts/%HOSTNAME%/%$now%.log"   # create separate directories for hosts and daily log files
if $fromhost-ip != '127.0.0.1' then -?RemoteLogs                    # filter localhost logs
& stop                                                              # stop logging same log line to syslog file
```

Restart the service
```
$ sudo systemctl restart syslog
```

#### client side

Check the server side configs by creating test logs

```
$ sudo logger -t "Testing Log message" $(date)
```












