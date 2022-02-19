---
published: false
---
## From Nginx Log to Elastic Stack no BS.

Nginx Log are the most common format for Webserver access log, in this tutorial we are going to use Elastic Stack to perform simple analysis on the Nginx Log.

## Requirements

1. Docker
2. Nginx Log with default format([Combined Log Format](http://fileformats.archiveteam.org/wiki/Combined_Log_Format#:~:text=The%20Combined%20Log%20Format%20is,the%20referer%20and%20user%20agent.))

## Installation ELK

Execute below operation on the same directory for smooth operations.
\* if you create a new terminal you might not be in the right place ;)

1. Create a **new terminal** for ElasticSearch Server
```bash
docker pull docker.elastic.co/elasticsearch/elasticsearch:8.0.0;

docker network create elastic;

docker run --name es01 --net elastic -p 9200:9200 -it docker.elastic.co/elasticsearch/elasticsearch:8.0.0;
```

2. Find the `password`, `ca fingerprint` and Kibana `enrollment token` in the section.

```
... logs on above ...
-> Elasticsearch security features have been automatically configured!
-> Authentication is enabled and cluster connections are encrypted.

->  Password for the elastic user (reset with `bin/elasticsearch-reset-password -u elastic`):
  ELASTIC_SEARCH_PASSWORD

->  HTTP CA certificate SHA-256 fingerprint:
  ELASTIC_CA_FINGERPRINT

->  Configure Kibana to use this cluster:
* Run Kibana and click the configuration link in the terminal when Kibana starts.
* Copy the following enrollment token and paste it into Kibana in your browser (valid for the next 30 minutes):
  eyJ2ZXIiOiI4LjAuMCIsImFkciI6WyIxNzIuMTguMC4yOjkyMDAiXSwiZmdyIjoiZjZkYTZjNTdiYmMyY2IxMjYzZmNkZmNiNmQ5ZGI5NzcyMGFiZjFkY2U5Mjg3NTM2NTYzMzIxNDRlNTc4MTA4ZSIsImtleSI6IkU3MWpDSDhCMlhwSmRILTJUTE9FOm9RbzFzTnIxU2V1Z1lnaDhfcXY3RWcifQ==

... multinode instructions below ...
```

3. Create a **new terminal** to test the connection to the ElasticSearch server

	3a. Get path to cert
```bash
docker exec -it es01 /bin/bash -c "find /usr/share/elasticsearch -name http_ca.crt";
```
	3b. Copy file to local machine
```bash
docker cp es01:PATH_OUTPUT_FROM_3A .
```
	3c. test the connection
```bash
curl --cacert http_ca.crt -u elastic https://localhost:9200
```
	3d. if you see the below message you are successful.
```json
{
  "name" : "Cp8oag6",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "AT69_T_DTp-1qgIJlatQqA",
  "version" : {
    "number" : "8.0.0",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "f27399d",
    "build_date" : "2021-11-04T12:35:26.989068569Z",
    "build_snapshot" : false,
    "lucene_version" : "9.0.0",
    "minimum_wire_compatibility_version" : "7.16.0",
    "minimum_index_compatibility_version" : "7.0.0"
  },
  "tagline" : "You Know, for Search"
}
```

4. Create a **new terminal** to start the Kibana server.
```bash
docker run --name kibana --net elastic -p 5601:5601 docker.elastic.co/kibana/kibana:8.0.0
```

5. Copy the link from terminal to browser
	5a. input your `enrollment token` from step 2;
	5b. input your username(user is `elastic`) and 
    5c. `password` obtain from step 2
```
i Kibana has not been configured.

Go to http://0.0.0.0:5601/?code=054967 to get started.
```

6. Create a file call `filebeat.docker.yml` with the `password` and `ca fingerprint`
```yml
filebeat.autodiscover:
  providers:
    - type: docker
      hints.enabled: true

filebeat.inputs:
- type: log #Change value to true to activate the input configuration
  enabled: true
  paths:
    - "/var/log/nginx/access.log"

filebeat.modules:
- module: nginx
  access:
    var.paths: ["/var/log/nginx/access.log"]

processors:
- add_cloud_metadata: ~

output.elasticsearch:
  # Array of hosts to connect to.
  hosts: ["https://192.168.128.118:9200"]

  # Protocol - either `http` (default) or `https`.
  #protocol: "https"

  # Authentication credentials - either API key or username/password.
  #api_key: "id:api_key"
  username: "elastic"
  password: "`password`"
  ssl:
    enable: true
    ca_trusted_fingerprint: "`ca fingerprint`"
```
	6a. for the `hosts` you need to obtain your computer's ip address, you may use `ifconfig` command or `ip` command depending on your machine.
    ```
    > ifconfig
    	eth0 XXXXXXXXX
        	 inet addr:192.168.1.29
        OR
        wlan0 XXXXXXXX
        	  inet addr:192.168.1.64
    ```
7. Create a **new terminal** to start the Filebeat server.
```bash
docker pull docker.elastic.co/beats/filebeat:8.0.0;
docker run docker.elastic.co/beats/filebeat:8.0.0 setup -E setup.kibana.host=localhost:5601 -E output.elasticsearch.hosts=localhost:9200;
```
docker run -d \
  --network=elastic \
  --name=filebeat \
  --user=root \
  --volume="$(pwd)/filebeat.docker.yml:/usr/share/filebeat/filebeat.yml:ro" \
  --volume="/var/lib/docker/containers:/var/lib/docker/containers:ro" \
  --volume="/var/run/docker.sock:/var/run/docker.sock:ro" \
  --volume="/Users/eugene/Desktop/access.log:/var/log/nginx/access.log:ro" \
  docker.elastic.co/beats/filebeat:8.0.0 filebeat -e -strict.perms=false


