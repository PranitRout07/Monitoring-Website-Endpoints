# Monitoring-Website-Endpoints
1. Creation of Docker Network
```
docker network create monitor-app
```
2. Prometheus Server Deployment
```
docker run -d -p 9090:9090 --network monitor-app --name prometheus prom/prometheus
```
3. Grafana Server Deployment
```
docker run -d -p 3000:3000 --network monitor-app --name grafana grafana/grafana
```
4. Blackbox Exporter Setup
```
docker run -d -p 9115:9115 --network monitor-app --name blackbox-exporter bitnami/blackbox-exporter:latest
```
<img width="448" alt="run docker containers'" src="https://github.com/PranitRout07/Monitoring-Website-Endpoints/assets/102309095/17c07c02-a63c-406e-8347-94435f15f0be">


5. Prometheus Configuration
```
  - job_name: 'blackbox'
    metrics_path: /probe
    params:
      module: [http_2xx]  # Look for a HTTP 200 response.
    static_configs:
      - targets:
        - http://prometheus.io     
        - http://host.docker.internal:80 
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 192.168.56.1:9115 
```
<img width="689" alt="edit prometheus configuration" src="https://github.com/PranitRout07/Monitoring-Website-Endpoints/assets/102309095/ba5c4658-0944-432b-b5b5-0762fec503fe">

6. Verification and Connection
<img width="1280" alt="prom" src="https://github.com/PranitRout07/Monitoring-Website-Endpoints/assets/102309095/e860550d-89df-4f4d-9f7d-130827cc5171">

7. Dashboard Creation with Grafana
<img width="1280" alt="visualization-1" src="https://github.com/PranitRout07/Monitoring-Website-Endpoints/assets/102309095/341b711f-172e-4e94-b2b0-fc686a7351b2">

<img width="1280" alt="visualization-2" src="https://github.com/PranitRout07/Monitoring-Website-Endpoints/assets/102309095/70143226-7041-45c7-9624-c6670241db06">
<img width="1280" alt="visualization-3" src="https://github.com/PranitRout07/Monitoring-Website-Endpoints/assets/102309095/9d1f4be4-5813-4520-bf31-802e05ad089e">

