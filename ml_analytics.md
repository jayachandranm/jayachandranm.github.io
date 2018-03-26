Install kibana,  
Download .deb and install using Ubuntu softare  
The installation location is /usr/share/kibana (/bin)  

Install elasticsearch,  
Download .deb and install using Ubuntu software  
The installation folder is /usr/share/elasticsearch  
sudo service elasticsearch start  
curl -XGET 'localhost:9200/?pretty'  

Install x-plugin for Kibana and ES,  
sudo bin/elasticsearch-plugin install x-pack  
sudo service elasticsearch restart  
sudo bin/x-pack/setup-passwords auto  
(Note down user accounts and passwords)  

Go to Kibana installation folder,  
bin/kibana-plugin install x-pack  
Edit /etc/kibana/kibana.yml  
elasticsearch.username: "kibana"   
elasticsearch.password:  "<pwd>"  

Start kibana,   
bin/kibana   
From browser, http://localhost:5601  

Changed password for user kibana, PASSWORD kibana = JZD03lX9c4uCqJBdw8bw  
Changed password for user logstash_system, PASSWORD logstash_system = Dq6zFV2gp9nU1KFjKEnQ  
Changed password for user elastic, PASSWORD elastic = N5UOCAadQLn0xSGHbsWq  

Download sample data,  
https://www.elastic.co/guide/en/kibana/6.x/tutorial-load-dataset.html  
cd Downloads,  
curl --user elastic:<pwd> -H 'Content-Type: application/x-ndjson' -XPOST 'localhost:9200/shakespeare/doc/_bulk?pretty' --data-binary   @shakespeare_6.0.json  

curl -XGET 'localhost:9200/_cat/indices?v&pretty'  

Set mappings for sample data,  
http://localhost:5601/app/kibana#/dev_tools/console?  load_from=https:%2F%2Fwww.elastic.co%2Fguide%2Fen%2Fkibana%2F6.x%2Fsnippets%2Ftutorial-load-dataset%2F1.json&_g=()  

curl --user elastic:<pwd> -XDELETE 'localhost:9200/logstash-*'  


