
elastic user: elastic
elastic pass: N6EzPOWJEimjE1IvC0n+

##### Steps to setup ELK with docker:

1) First, we have created a docker network called "elastic".
2) Second, create elasticsearch container and network will be "elastic" (as we have created already).
3) Third, create kibana container and network will be "elastic" (as we have created already).

##### Reset elasticsearch Token:
a) This token is valid for for 30 minutes.
b) Need to define which user is logged in into elasticsearch container and we defined "/usr/share/elasticsearch" here elasticsearch as user:

    docker exec -it es-container /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana       (resetting token for "kibana")

##### Reset elasticsearch password:

    docker exec -it es-container /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic   (it will ask for elastic pass)

##### Elasticsearch certificate path:

This cert used to loggin into elastic search (also used username and password)

cert path:
config/certs/http_ca.crt

copy the cert path from container to local:
docker cp es-container:/usr/share/elasticsearch/config/certs/http_ca.crt .      (copying to current path)

loggin to elastic search by curl:
curl --cacert http_ca.crt -u elastic https://localhost:9200                 (will ask for elastic password)


##### create elasticsearch container:
first, pull the elasticsearch image:
    docker pull docker.elastic.co/elasticsearch/elasticsearch:8.4.1

then, create elasticsearch container (note:- we have passed -t flag to get the outputs into terminal mode, like elasticsearch password, token etc. )
    docker run --rm --name es-container --net elastic -p 9200:9200 -p 9300:9300 -t docker.elastic.co/elasticsearch/elasticsearch:8.4.1


##### create kibana container:
first, pull the kibana image:
    docker pull docker.elastic.co/kibana/kibana:8.4.1

then, create kibana container:
    docker run --rm --name kibana-container --net elastic -p 5601:5601 docker.elastic.co/kibana/kibana:8.4.1

Note:- after creating kibana container, it gives verification code (6 digits), put this while setting-up kibana for the first time. 

here, we are not reffering the docker-compose file to create any elk containers.
