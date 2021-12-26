# prometheus-kafka-exporter

A Prometheus exporter for [Apacher Kafka](https://kafka.apache.org/) metrics.

This chart bootstraps a [Kafka Exporter](https://github.com/danielqsj/kafka_exporter) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.


## Install/Upgrade Chart:

```bash
helm upgrade -i kafka-exporter . -n platform --values=environments/shared-values.yaml --values=environments/prod/prod-values.yaml
```

## Verification

run `kubectl port-forward svc/kafka-exporter -n platform 9308` and check the metrics available on localhost:9308 the page should display metrics
