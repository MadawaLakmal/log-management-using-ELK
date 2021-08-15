

# Install ELK Stack

#### Every component of the the ELK stack needs *JAVA* as it's runtime env. So first of all we need to install java.

## Install java

Check available java versions, 

``` $ java -version ```

Install stable version 

``` $ sudo apt install openjdk-11-jdk-headless ```

## Install Elastic search
[ElsaticSearch] (https://www.elastic.co/downloads/elasticsearch)

Make sure you have installed java on your system

Add Elastic Repository to the system,
```
$ wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
$ sudo apt-get install apt-transport-https
$ echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list
$ sudo apt-get update
```
Install ElasticSearch package,
```
$ sudo apt-get install elasticsearch
```

#### Configure Elasticsearch

Open main elasticsearch config file,

```$ sudo vim /etc/elasticsearch/elasticsearch.yml```

Uncomment and Config following lines,
```
network.host: 192.168.0.1       # Elasticsearch node IP address
http.port: 9200                 # Elastic Service Port	
discovery.type: single-node     # Without this service will recheck for prod configurations.
```

Enable and start the service,
```
$ systemctl enable elasticsearch.service
$ systemctl start elasticsearch.service
```

Check elasticsearch service 
```
$ curl -X GET "192.168.112.3:9200"
```

# Install Logstash

Make sure you have installed java on your system

Add Elastic Repository to the system,

```
$ wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
$ sudo apt-get install apt-transport-https
$ echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list
$ sudo apt-get update
```

Install Logstash package
```
$ sudo apt install logstash
```

#### Configure Logstash for beat inputs

Open logstash main config file
```
$ sudo vim /etc/logstash/conf.d/main-logstash.conf
```

Sample template lines,

```
input {
    beats {
        port => "5044"
    }
}

# Filter part not necessary

filter {
    grok {
        match => { "message" => "%{COMBINEDAPACHELOG}"}
    }
    geoip {
        source => "clientip"
    }
}

output {
    elasticsearch {
        hosts => [ "192.168.112.3:9200" ]
        index => "%{[@metadata][beat]}-%{[@metadata][version]}-%{+YYYY.MM.dd}"
    }
}
```

Enable and start the service
```
$ sudo systemctl enable logstash.service
$ sudo systemctl start logstash.service
```
Logstash config test
```
$ sudo logstash -f /etc/logstash/conf.d/main-logstash.conf --config.test_and_exit
```

# Install Kibana

Make sure you have installed java on your system

Add Elastic Repository to the system,

```
$ wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
$ sudo apt-get install apt-transport-https
$ echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list
$ sudo apt-get update
```
Install kibana package,
```
$ sudo apt install kibana
```

Change main config file to enable outside connections for kibana and to connect to elasticsearch service,

Open main config file,
```
$ sudo vim /etc/kibana/kibana.yml
```
Change followin entries,
```
server.port: 5601                        		# Kibana serving port config
server.host: "192.168.112.21"				# Kibana host IP
elasticsearch.hosts: ["http://192.168.112.3:9200"]	# ElasticSearch host IP & Port
```

Enable and start the service
```
$ sudo systemctl enable kibana.service
$ sudo systemctl start kibana.service
```
