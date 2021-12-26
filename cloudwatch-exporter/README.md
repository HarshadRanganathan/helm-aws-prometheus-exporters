# Prometheus Cloudwatch Exporter

An exporter for [Amazon CloudWatch](http://aws.amazon.com/cloudwatch/), for Prometheus.

This chart bootstraps a [cloudwatch exporter](http://github.com/prometheus/cloudwatch_exporter) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- [IAM Role for service account](https://aws.amazon.com/blogs/opensource/introducing-fine-grained-iam-roles-service-accounts/) attached to a service account with an annotation. If you run the pod as nobody in `securityContext.runAsUser` then also set `securityContext.fsGroup` to the same value so it will be able to access to the mounted secret.

## Install the chart

Run the following command in the platform namespace of the platform cluster
`helm upgrade -i prometheus-cloudwatch-exporter . -n platform --values=environments/shared-values.yaml`

## Verification

Run `kubectl port-forward svc/prometheus-cloudwatch-exporter -n platform 9106` when logged in to the cluster/aws localhost:9106 will now host a link to the metrics this should be populated and not log an error.
This is important to check when adding new metrics as some may be populated
Check the bottom of the metrics

```
# HELP cloudwatch_exporter_scrape_error Non-zero if this scrape failed.
# TYPE cloudwatch_exporter_scrape_error gauge
cloudwatch_exporter_scrape_error 0.0
```

## Reloading Configuration

You may need to reload configuration when adding metrics
There are two ways to reload configuration:

Send a SIGHUP signal to the pid: kill -HUP 1234
POST to the reload endpoint: curl -X POST localhost:9106/-/reload
If an error occurs during the reload, check the exporter's log output.
### AWS Credentials or Role

For Cloudwatch Exporter to operate properly, you must configure either AWS credentials or an AWS role with the [correct policy](https://github.com/prometheus/cloudwatch_exporter#credentials-and-permissions).

- To configure AWS credentials by value, set `aws.aws_access_key_id` to your [AWS_ACCESS_KEY_ID], and `aws.aws_secret_access_key` to [AWS_SECRET_ACCESS_KEY].
- To configure AWS credentials by secret, you must store them in a secret (`kubectl create secret generic [SECRET_NAME] --from-literal=access_key=[AWS_ACCESS_KEY_ID] --from-literal=secret_key=[AWS_SECRET_ACCESS_KEY]`) and set `aws.secret.name` to [SECRET_NAME]
- To configure an AWS role (with correct policy linked above), set `awsRole` to [ROLL_NAME]
