LEK INSTALLATION
=====================================================

#1 Logstash
http://logstash.net/docs/1.4.2/tutorials/getting-started-with-logstash

#1.1 Prerequisite: Java installed and set JAVA_HOME
$ sudo add-apt-repository ppa:webupd8team/java
$ sudo apt-get install oracle-java7-installer

Test Java!
$ java -version

#1.2 Go!
$ curl -O https://download.elasticsearch.org/logstash/logstash/logstash-1.4.2.tar.gz
$ tar zxvf logstash-1.4.2.tar.gz
$ cd logstash-1.4.2

Run logstash like:
$ bin/logstash -e 'input { stdin { } } output { stdout {} }'
or
$ bin/logstash -f logstash-demo.conf


#2 ElasticSearch
http://www.elasticsearch.org/overview/elkdownloads/
https://www.digitalocean.com/community/tutorials/how-to-install-elasticsearch-on-an-ubuntu-vps
http://www.elasticsearch.cn/
http://www.blogjava.net/ivanwan/archive/2013/10/04/404680.html

We can
2.1 Install from the zip or tar.gz archive.
$wget https://download.elasticsearch.org/elasticsearch/elasticsearch/elasticsearch-1.3.4.tar.gz
$ tar -zxvf elasticsearch-1.3.4.tar.gz
or
$ wget https://download.elasticsearch.org/elasticsearch/elasticsearch/elasticsearch-1.3.4.zip
$ apt-get install unzip
$ unzip elasticsearch-1.3.4.zip

Binary in "elasticsearch-1.3.4/bin/"
Config in "elasticsearch-1.3.4/config/"

2.2 Install from the deb package.
$ wget https://download.elasticsearch.org/elasticsearch/elasticsearch/elasticsearch-1.3.4.deb
$ dpkg -i elasticsearch-1.3.4.deb

Binary in "/usr/share/elasticsearch"
Init script in "/init.d/elasticsearch"
Config in "/etc/elasticsearch"

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