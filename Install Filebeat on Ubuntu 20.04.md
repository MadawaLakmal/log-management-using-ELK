# Install Filebeat

#### Donwload and Install 
For the latest : https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-installation-configuration.html
```
$ wget https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-oss-7.12.0-amd64.deb

$ sudo dpkg -i filebeat-oss-7.12.0-amd64.deb

```
#### Filebeat Configuration

Open main config file
```
$ sudo vim /etc/filebeat/filebeat.yaml
```
Change following entries to send log data into logstash
```
output.logstash:
  # The Logstash hosts
  hosts: ["192.168.112.6:5044"]

  # Optional SSL. By default is off.
  # List of root certificates for HTTPS server verifications
  # ssl.certificate_authorities: ["/etc/ssl/certs/logstash.crt"]

  # Certificate for SSL client authentication
  # ssl.certificate: "/etc/ssl/certs/logstash.crt"

  # Client Certificate Key
  # ssl.key: "/etc/ssl/private/logstash.key"
  ```
  #### Usefull filebeat commands
  
  list available & enabled modules
  ```
  $ sudo filebeat modules list
  ```
  enable a module
  ```
  $ sudo filebeat modules enable "module-name"
  ```
  configuration changes testing
  ```
  $ sudo filebeat test config
  ```
  Logstash node network connectivity check
  ```
  $ sudo filebeat test output
  ```

