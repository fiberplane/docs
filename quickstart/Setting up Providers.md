---
title: Setting up Providers
category: 63d13748941c7a001b58934a
---
# Setting up Providers

Providers are full-stack plugins in Fiberplane that allow you to pull in live data directly from your infrastructure data sources into your Notebooks.

## Add a data source

Your data sources can be added from the Studio using direct access (if your data sources URL are open) or using the `data_sources.yaml` file that is then passed in the Docker or Kubernetes configuration. Here’s an example configuration

```yaml
# data_sources.yaml
#
# Replace the following line with the name of the data source
- name: prometheus-prod
  description: Prometheus (Production)
  providerType: prometheus
  config:
    # Replace the following line with your Prometheus URL and port
    url: http://prometheus:9090
		# If accessing your data source requires authentication - add the token below
		token: <TOKEN>
```

## Data Source Names

Note that data source names (such as `prometheus-prod` in the example above) must follow the [Fiberplane name format](Configuration%20help%20FAQ%2018941c30bc32404785f767ab1892c0ce.md). Data source names must also be unique per proxy.

Currently the following Provider types are supported:

[Prometheus](Setting%20up%20Providers%20d28e28323f40453abee907f37dbfd2fb/Prometheus%208a01211885ac42659637a274d6b4c38d.md)

[Elasticsearch](Setting%20up%20Providers%20d28e28323f40453abee907f37dbfd2fb/Elasticsearch%2067e7fae5cc4041a2bd1c894085d86141.md)

[Grafana Loki](Setting%20up%20Providers%20d28e28323f40453abee907f37dbfd2fb/Grafana%20Loki%20108c2f8f54ae49ba8f67fd527f1149c8.md)
