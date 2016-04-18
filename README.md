# Collect-System-Metrics---topbeat
system metrics using topbeat and elasticsearch
topbeat commands used-
1. for setup
cd 'C:\Program Files\Topbeat'
.\install-service-topbeat.ps1
2. on config file
  output:
  ### Elasticsearch as output
  elasticsearch:
    # Array of hosts to connect to.
     hosts: ["127.0.0.1:9200"]. basically no change on config file.
3.template 
   Invoke-WebRequest -Method Put -InFile topbeat.template.json -Uri http://localhost:9200/_template/topbeat?pretty
  also uncomment the template part under elasticsearch output.
4.Start-Service topbeat
5.for checking process working of topbeat
  .\topbeat.exe -c topbeat.yml -e -v -d "*"

setting output to elasticsearch - changes done in config file.
but can also use logstash as output to topbeat and setting output of logstash as input to elastic search.Makes data more presentable.
creating a config file and adding this data to it
input {
  beats {
    port => 5044
  }
}

output {
  elasticsearch {
    hosts => "127.0.0.1:9200"
    manage_template => false
    index => "%{[@metadata][beat]}-%{+YYYY.MM.dd}"
    document_type => "%{[@metadata][type]}"
  }
}
 before adding config file
bin\logstash-plugin install logstash-input-beats

after adding config file
bin\logstash-plugin update logstash-input-beats

starting
bin\logstash.bat -f logstash.conf

For elaastic search-
cd C:\Program Files\elasticsearch-2.3.1
bin\elasticsearch.bat

for configuration 
1.go to http://127.0.0.1:9200
result  
{
  "name" : "Typhon",
  "cluster_name" : "elasticsearch",
  "version" : {
    "number" : "2.3.1",
    "build_hash" : "bd980929010aef404e7cb0843e61d0665269fc39",
    "build_timestamp" : "2016-04-04T12:25:05Z",
    "build_snapshot" : false,
    "lucene_version" : "5.5.0"
  },
  "tagline" : "You Know, for Search"
}


