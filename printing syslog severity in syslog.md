Open main syslog config file,

```
$ vim /etc/rsyslog.conf
```

First commentout default template,
```
#$ActionFileDefaultTemplate RSYSLOG_TraditionalFileFormat
```

Then add following template, You can config variable places as your wish,

```
$template TemplateName,"[Severity-%syslogpriority%][%pri-text%] ____ %timegenerated%  %HOSTNAME%  %syslogtag%  %msg%\n"
$ActionFileDefaultTemplate TemplateName
```

Restart the syslog service,

```
$ systemctl restart syslog.service
```

Chech changes,

```
$ tail -f /var/log/syslog
```
