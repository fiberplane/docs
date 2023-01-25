---
title: Providers
category: 63d137589bdc7d088088086c
---

# Providers

Providers are full-stack plugins in Fiberplane that allow you to pull in live data directly from your infrastructure data sources into your Notebooks. They are compiled to WebAssembly and can run both on the Studio and the Proxy.

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

## Prometheus

Query, filter, and visualize your Prometheus metrics.

[Prometheus](Providers%2003e49d201c4445a1bd07da6fad4d9d89/Prometheus%204ea172902216439a92f252b327aae004.md)

## Elasticsearch

Query, search, and analyze your Elastic logs.

[Elasticsearch](Providers%2003e49d201c4445a1bd07da6fad4d9d89/Elasticsearch%205fb0bc4d48064d71bb85a56ba43fa4e1.md)

## Loki

Query, search, and analyze your Loki logs.

[Grafana Loki](Providers%2003e49d201c4445a1bd07da6fad4d9d89/Grafana%20Loki%20a72f5b28e04340299e1f6d8293dcdbe8.md)
