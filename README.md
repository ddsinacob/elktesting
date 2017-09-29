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

    yum-config-manager --add-repo https://docs.docker.com/engine/installation/linux/repo_files/oracle/docker-ol7.repo
    yum install -y docker-engine

Docker Proxy
------------

  ::

    Ubuntu 14 / Redhat

    cat /etc/default/docker
    # If you need Docker to use an HTTP proxy, it can also be specified here.
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


Pull kibana Image
-----------------

  ::

    docker pull kibana

Launch kiabana Container
------------------------

  ::

    docker run -d -p 5601:5601 -h kibana --name kibana --link elasticsearch:elasticsearch kibana

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


Test
---

  ::
  
    telnet localhost 8888
    Hello World!
    Wonderfull day at Bangalore
    
    
