docker run -dit -p 7574:7574 -p 8983:8983 -p 8984:8984 -p 3000:3000 -p 9090:9090 -p 9854:9854 -p 9100:9100 --name solr_graf ubuntu
docker exec -it solr_graf bash

## Basic installations
    apt update
    apt install default-jre -y
    apt-get install wget -y
    apt install nano -y
    apt install vim -y
    apt install sudo -y
    apt install screen -y

## installation steps for grafana
    sudo apt update
    sudo apt install -y gnupg2 curl software-properties-common
    curl -fsSL https://packages.grafana.com/gpg.key|sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/grafana.gpg
    sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
    sudo apt update
    sudo apt -y install grafana
    sudo service grafana-server start

## installation steps for solr
    wget https://archive.apache.org/dist/lucene/solr/8.11.2/solr-8.11.2.tgz
    tar -xzf solr-8.11.2.tgz
    cd solr-8.11.2
    bin/solr -e cloud -force
***
### start prometheus-exporter
    cd contrib/prometheus-exporter/
    bin/solr-exporter -p 9854 -z localhost:9983 -f ./conf/solr-exporter-config.xml -n 16 # -> For single Zk
    bin/solr-exporter -p 9854 -z zk1:2181,zk2:2181,zk3:2181 -f ./conf/solr-exporter-config.xml -n 16 # -> For multiple Zk
    

## installation steps for node_exporter
    wget https://github.com/prometheus/node_exporter/releases/download/v1.2.1/node_exporter-1.2.1.linux-amd64.tar.gz
    tar -xzf node_exporter-1.2.0.linux-amd64.tar.gz
    cd node_exporter-1.2.1.linux-amd64
    ./node_exporter



## installation steps for prometheus
    wget https://github.com/prometheus/prometheus/releases/download/v2.50.1/prometheus-2.50.1.linux-amd64.tar.gz
    tar xvfz prometheus-2.50.1.linux-amd64.tar.gz
    cd prometheus-2.50.1.linux-amd64
    vim prometheus.yml
    # prometheus.yml Configurations for node_exporter and Solr    (Add in the end) 
         - job_name: 'node'
              static_configs:
              - targets: ['localhost:9100']
         - job_name: 'solr'
              static_configs:
              - targets: ['localhost:9854']
    ./prometheus


http://localhost:9090/targets -> Conformation
![image](https://github.com/Krishna4802/Monitoring-Solr-With-Grafana/assets/139359113/b379241e-1718-400b-b811-86b18be3c9af)


### Finally start promothes 
    ./prometheus
http://localhost:3000/ ->  For grafana

<img width="1440" alt="Screenshot 2024-03-05 at 9 37 05 PM" src="https://github.com/Krishna4802/Monitoring-Solr-With-Grafana/assets/139359113/db406804-cbab-4499-aa88-e32c5289fa4e">


# dashboard links
    https://grafana.com/grafana/dashboards/1860 -> Node_extracter
    https://grafana.com/grafana/dashboards/405  -> Node_extracter
    
    https://grafana.com/grafana/dashboards/12456 -> Solr
    https://grafana.com/grafana/dashboards/2551  -> Solr
    https://grafana.com/grafana/dashboards/3888  -> Solr

