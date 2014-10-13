LEK INSTALLATION
=====================================================

## Prerequisite: Java installed and set JAVA_HOME
```bash
$ sudo add-apt-repository ppa:webupd8team/java
$ sudo apt-get install oracle-java7-installer
```

Let's test Java!
```bash
$ java -version
```

## ElasticSearch
[LEK stack downloads](http://www.elasticsearch.org/overview/elkdownloads/)

[Install ES on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-install-elasticsearch-on-an-ubuntu-vps)

[ES Chinese site](http://www.elasticsearch.cn/)

[ES guide Chinese gitbook](http://learnes.net)

We can download and install ES from [homepage](http://www.elasticsearch.org/overview/elkdownloads/)

Or, We have two options to install on terminal.
### For Ubuntu, install ES from the deb package:
```bash
$ wget https://download.elasticsearch.org/elasticsearch/elasticsearch/elasticsearch-1.3.4.deb
$ sudo dpkg -i elasticsearch-1.3.4.deb
$ sudo service elasticsearch restart
```

* Binary files in dir "/usr/share/elasticsearch"
* Init script in dir "/init.d/elasticsearch"
* Config files in dir "/etc/elasticsearch"

### For Ubuntu and Mac OS X, install from the zip or tar.gz archive.
```bash
$ wget https://download.elasticsearch.org/elasticsearch/elasticsearch/elasticsearch-1.3.4.tar.gz
$ tar -zxvf elasticsearch-1.3.4.tar.gz
```
or
```bash
$ wget https://download.elasticsearch.org/elasticsearch/elasticsearch/elasticsearch-1.3.4.zip
$ apt-get install unzip
$ unzip elasticsearch-1.3.4.zip
```

* Binary files in dir "elasticsearch-1.3.4/bin/"
* Config files in dir "elasticsearch-1.3.4/config/"

### Change ES cluster name
Edit config file "elasticsearch.yml".
* Installation from deb package: config file in "/etc/elasticsearch/elasticsearch.yml"
* Installation from zip/tarball archive: config file in "elasticsearch-1.3.4/config/elasticsearch.yml"

Find out the line of 
```bash
#cluster.name: elasticsearch
```
Uncomment it and change the name of anything you like.
```bash
cluster.name: elasticsearch-xxx
```

*NOTE: If all of ES nodes in the LAN have same cluster name, they would act as one cluster and share data.


### Intallation of plugin head
This plugin help to view state of cluster and do some queries on UI

* Installation from deb package:
```bash
$ sudo /usr/share/elasticsearch/bin/plugin -install mobz/elasticsearch-head
```

* Installation from zip/tarball archive:
```bash
$ elasticsearch/bin/plugin -install mobz/elasticsearch-head
```


Visit "http://localhost:9200/_plugin/head/" in browser. [More details](http://blog.csdn.net/laigood/article/details/8193758)



## Logstash
[Reference - Getting started with logstash](http://logstash.net/docs/1.4.2/tutorials/getting-started-with-logstash)

```bash
$ curl -O https://download.elasticsearch.org/logstash/logstash/logstash-1.4.2.tar.gz
$ tar zxvf logstash-1.4.2.tar.gz
```

###Test logstash:
```bash
$ cd logstash-1.4.2
$ bin/logstash -e 'input { stdin { } } output { stdout {} }'
```

* NOTE: "-e" means it sets configurations in command line.

Type in "hello world" in, and check if you see another "hello wolrd" from terminal. 

Congratulations! Logstash works well! Let's go ahead.

###Run logstash with certain config file:
[logstash docs](http://logstash.net/docs/1.4.2/)


* Create one config file:
```bash
vim logstash-demo.conf
```

Copy and paste contents below:
```bash
input {
    # receive data from tcp connection
    tcp {
        type => "tcp8888"
	      port => 8888
        mode => "server"
        ssl_enable => false
        # input data is json format
	      codec => json
    }
    file {
        path => "/PATH/TO/YOUR/INPUT/FILE"
        # scan file from beginning or end, default value is "end"
        #start_position => "beginning"
        codec => json
    }
}

filter {
    # only handle certain inputs
    if [type] == "tcp8888" {
        # parse json data
        json {
            source => "message" 
        }
    }
}

output {
    # only handle valid inputs and output results
    if [type] == "tcp8888" and "_jsonparsefailure" not in [tags] {
        # redirect results to elasticsearch server
        elasticsearch {
            # name of cluster that you have changed in "elasticsearch.yml"
            cluster => "elasticsearch-xxx"
            index => "db"
            index_type => "table"
        }
    }
}
```

Save it. Run logstash with config file:

```bash
$ bin/logstash -f logstash-demo.conf
```

* NOTE: "-f" means it sets configurations from file.

2.3 Run as linux service
elasticsearch-servicewrapper?

2.4 Cluster config
!!!TODO!!!

3 Kibana
Download from homepage, and decompress...

The easiest way to run Kibana:
$ cd ${KIBANA_HOME}/
$ python -m SimpleHTTPServer

Visit it in browser: http://localhost:8000

Done.
