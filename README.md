# microservices-prometheus-demo
Docker based application to pull metrics of Microservices


Let's build and run both microservices
       
```aidl
docker-compose up -d
```
Let's understand prometheus configuration file

* After every 5s Prometheus will invoke microservice1 & microservice2 for metrics.
```aidl
global:
  scrape_interval: 5s

scrape_configs:
  - job_name: 'microservices1'
    metrics_path: '/prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['192.168.43.163:6666']

  - job_name: 'microservices2'
    metrics_path: '/prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['192.168.43.163:8888']
```

```aidl
cd prometheus
docker-compose up -d
```
curl -s -XPOST localhost:9090/-/reload