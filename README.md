# Microservices Prometheus Demo
Docker based application to pull metrics from Microservices.

#### Pre-requisite
* Basic knowledge of docker & docker-compose
* Basic knowledge of maven project
* Basic idea of Prometheus (TSDB) and Grafana's role.


### Let's build and run both microservices and Prometheus+grafana

```aidl
mvn clean install
docker-compose up -d --build
```

#### Let's understand prometheus configuration file prometheus.yml

* After every 5s Prometheus will invoke microservice1 & microservice2 for metrics.
```aidl
global:
  scrape_interval: 5s

scrape_configs:
  - job_name: 'microservices1'
    metrics_path: '/prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['192.168.43.163:8080']

  - job_name: 'microservices2'
    metrics_path: '/prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['192.168.43.163:9090']
```

#### Steps to configure Grafana panel
![Alt text](images/prometheus-metric-cursor-select-up.png?raw=true "Select prometheus Up Metric")
![Alt text](images/prometheus-metric-cursor-select-up-result.png?raw=true "After Selection - showing 2 microservices")

![Alt text](images/grafana-homepage.png?raw=true "Access Grafana")
![Alt text](images/grafana-add-datasource.png?raw=true "Access Grafana - Configure Data source")
![Alt text](images/new-panel.png?raw=true "Create new panel")
![Alt text](images/select-guage.png?raw=true "Select guage")
![Alt text](images/Define-name.png?raw=true "Define name")
![Alt text](images/configure-query.png?raw=true "Configure query")
![Alt text](images/microservices.png?raw=true "Show running microservices")
Check running docker instances
```aidl
docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
944d5cf71bb8        prom/prometheus     "/bin/prometheus --c…"   3 minutes ago       Up 3 minutes        0.0.0.0:9090->9090/tcp   prometheus
0f3d8a8402fa        microservice1       "java -Djava.securit…"   3 minutes ago       Up 3 minutes        0.0.0.0:8080->8080/tcp   microservice1
667b6fb1b406        grafana/grafana     "/run.sh"                3 minutes ago       Up 3 minutes        0.0.0.0:3000->3000/tcp   grafana
9f0d56e5e4ac        microservice2       "java -Djava.securit…"   3 minutes ago       Up 3 minutes        0.0.0.0:7070->7070/tcp   microservice2
```
Kill microservice2
```aidl
 docker kill 9f0d56e5e4ac
9f0d56e5e4ac
```
![Alt text](images/microservice2-show-down.png?raw=true "Show microservices up and down both microservices")


In case you make any changes in prometheus.yml run below command it will reload the scrape configuration.
```aidl
curl -s -XPOST localhost:9090/-/reload
```

