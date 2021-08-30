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

Option 1. Sending log data through logstash

```
## Make sure to comment the Elasticsearch output configs before enable the logstash output configs as follows,

# ---------------------------- Elasticsearch Output ----------------------------
#output.elasticsearch:
  # Array of hosts to connect to.
  #hosts: ["localhost:9200"]
```

Change logstash output as follows,
```
output.logstash:
  # The Logstash hosts
  hosts: ["logstash_node_ip:5044"]

  # Optional SSL. By default is off.
  # List of root certificates for HTTPS server verifications
  # ssl.certificate_authorities: ["/etc/ssl/certs/logstash.crt"]

  # Certificate for SSL client authentication
  # ssl.certificate: "/etc/ssl/certs/logstash.crt"

  # Client Certificate Key
  # ssl.key: "/etc/ssl/private/logstash.key"
```
  
Option 2. Sending log data into elasticsearch directly,
  
```
# ---------------------------- Elasticsearch Output ----------------------------
output.elasticsearch:
  # Array of hosts to connect to.
  hosts: ["elasticsearch_node_ip:9200"]  
```
  
  #### Usefull filebeat commands
  
  list available & enabled modules
  ```
  $ sudo filebeat modules list
  ```
  enable a module
  ```
  $ sudo filebeat modules enable "module-name"   # i.e : filebeat modules enable apache
  ```
  configuration changes testing
  ```
  $ sudo filebeat test config
  ```
  Logstash node network connectivity check
  ```
  $ sudo filebeat test output
  ```

