# microservices-prometheus-demo
Docker based application to pull metrics of Microservices

Let's build and run both microservices
       
```aidl
mvn clean install
docker-compose up -d --build
```

Let's understand prometheus configuration file prometheus.yml

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

Run Prometheus 
```aidl
cd prometheus
docker-compose up -d
```


In case you make any changes in prometheus.yml run below command it will reload the scrape configuration.
```aidl
curl -s -XPOST localhost:9090/-/reload
```


```aidl
docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
944d5cf71bb8        prom/prometheus     "/bin/prometheus --c…"   3 minutes ago       Up 3 minutes        0.0.0.0:9090->9090/tcp   prometheus
0f3d8a8402fa        microservice1       "java -Djava.securit…"   3 minutes ago       Up 3 minutes        0.0.0.0:8080->8080/tcp   microservice1
667b6fb1b406        grafana/grafana     "/run.sh"                3 minutes ago       Up 3 minutes        0.0.0.0:3000->3000/tcp   grafana
9f0d56e5e4ac        microservice2       "java -Djava.securit…"   3 minutes ago       Up 3 minutes        0.0.0.0:7070->7070/tcp   microservice2
```

```aidl
 docker kill 9f0d56e5e4ac
9f0d56e5e4ac
```