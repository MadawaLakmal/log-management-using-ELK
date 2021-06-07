

# Install ELK Stack

Every component of the the ** ELK ** stack needs java as it's runtime env. So first of all we need to install java.

## Install java

Check available java versions 
> $ java -version
Install stable version 
> $ sudo apt install openjdk-8-jre-headless

## Install Elastic search [] [ https://www.elastic.co/downloads/elasticsearch ]

```
Add Elastic Repository and install
	$ wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
	$ sudo apt-get install apt-transport-https
  $ echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list
  $ sudo apt-get update
	$ sudo apt-get install elasticsearch
```

###### Configure Elasticsearch

```
$ sudo vim /etc/elasticsearch/elasticsearch.yml
	Config following lines,
		network.host: 192.168.0.1
		http.port: 9200
	
	Configs for single-node
  	discovery.type: single-node #without this it will recheck for prod configurations.
```
```
	Recommended configs for java heap, (Half from the total memory)

		$ sudo vim /etc/elasticsearch/jvm.options

		Edit,
		-Xms512m # minimum size
		-Xmx512m # maximum size
```

Enable and start the service,
>    $ systemctl enable elasticsearch.service
>    $ systemctl start elasticsearch.service

Check elasticsearch service 
	$ curl -X GET "192.168.112.3:9200"

Install Logstash

Install java
Check available java versions 
$ java -version
Install stable version 
$ sudo apt install openjdk-8-jre-headless

Add Elastic Repository and install
	$ wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
	$ sudo apt-get install apt-transport-https

		$ echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list

		$ sudo apt-get update
		$ sudo apt install logstash

Configure Logstash for beat inputs
	$ sudo vim /etc/logstash/conf.d/main-logstash.conf

	Add these lines,
input {
    beats {
        port => "5044"
    }
}
# filter {
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

Enable and start the service
Install Kibana

Install java
Check available java versions 
$ java -version
Install stable version 
$ sudo apt install openjdk-8-jre-headless

Add Elastic Repository and install
	$ wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
	$ sudo apt-get install apt-transport-https

		$ echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list

		$ sudo apt-get update
		$ sudo apt install kibana

Add following configs to enable outside connections for kibana and to connect to elasticsearch service,
	$ sudo vim /etc/kibana/kibana.yml

	#server.port: 5601
	#server.host: "192.168.112.21"
            #elasticsearch.hosts: ["http://192.168.112.3:9200"]

Enable and start the service
