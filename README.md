# elktesting
Testing ELK using docker


ELK
===

  Elasticsearch, Logstash and Kibana.

  This configuration and testing we will be doing using docker



Install Docker
--------------

  ::

    apt install docker.io

  ::

    yum install -y docker-engine

Docker Proxy
------------

  ::

    Ubuntu 14 / Redhat

    cat /etc/default/docker
    export http_proxy="http://proxy.example.com:80"
    export https_proxy="http://proxy.example.com:80"

  ::

    Docker proxy setup in ubuntu 16

    mkdir /etc/systemd/system/docker.service.d
    cat /etc/systemd/system/docker.service.d/http-proxy.conf
    [Service]
    Environment="HTTP_PROXY=http://proxy.example.com:80/"
    Environment="HTTP_PROXY=http://proxy.example.com:80/"
    Environment="NO_PROXY=localhost,127.0.0.0/8,docker-registry.somecorporation.com"
    $ sudo systemctl daemon-reload
    $ sudo systemctl show --property Environment docker
    $ sudo systemctl restart docker




Pull elasticsearch Image
------------------------

  ::

    docker pull elasticsearch

Launch elasticsearch Container
------------------------------

  ::

    docker run -d -p 9200:9200 -p 9300:9300 -it -h elasticsearch --name elasticsearch elasticsearch

Verify Elastic Search is running
--------------------------------

  ::
  
    curl localhost:9200
    {
      "name" : "7mH_i9n",
      "cluster_name" : "elasticsearch",
      "cluster_uuid" : "Kj5Wj2WrQx64J197-vmW1w",
      "version" : {
        "number" : "5.5.2",
        "build_hash" : "b2f0c09",
        "build_date" : "2017-08-14T12:33:14.154Z",
        "build_snapshot" : false,
        "lucene_version" : "6.6.0"
      },
      "tagline" : "You Know, for Search"
    }


Pull kibana Image
-----------------

  ::

    docker pull kibana

Launch kiabana Container
------------------------

  ::

    docker run -d -p 5601:5601 -h kibana --name kibana --link elasticsearch:elasticsearch kibana
    
Kibana Dashboard
----------------

  ::
  
    http://localhost:5601/
   

Pull logstash Image
-------------------

  ::

    docker pull logstash


Launch logStash Container
-------------------------
    Simple configuration fiel, which listen on tcp port 8888 and send the log to elasticsearch
    ::

      logstash.conf
      --------------
      input {
        tcp {
          port => 8888
          }
      }
      output {
        elasticsearch {hosts => ["elasticsearch:9200"]}
      }

    ::

      docker run -d -p 9500:9500 -h logstash1 --name logstash1 --link elasticsearch:elasticsearch -it -v "$(pwd)"/configuration_files:/configuration_files logstash -f /configuration_fileslogstash.conf


Test/Send message to elasticsearch using logstash
-------------------------------------------------

  ::
  
    telnet localhost 8888
    Hello World!
    Wonderfull day at Bangalore
    
    
