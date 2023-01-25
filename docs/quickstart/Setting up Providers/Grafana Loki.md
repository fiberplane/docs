---
title: Grafana Loki
category: 63ce4f6008ed7e0016ef2eaa
---
# Grafana Loki

Use Loki to view logs from your infrastructure and application. If you don’t have a running Loki instance, you can use our starter kit to start it up - instructions below!

---

# Setup

In order to enable Grafana Loki Provider in Fiberplane you need to either add it via direct access (for an openly accessible URL) or add it as a valid entry in the `data_source.yaml` configuration.

The `data_sources.yaml` format for adding a Loki Provider looks like this:

```yaml
# data_sources.yaml
#
# Replace the following line with the name of the data source
- name: loki-staging
  description: Loki (Staging)
  providerType: loki
  config:
    # Replace the following line with your Loki URL
    url: http://loki:3100
    # If accessing your Grafana Loki requires authentication - add the token (Bearer or Basic) below
    token: <TOKEN>
```

# Starter kit

If you don’t have a running Loki instance, you can use the guide below to spin up a simple instance in a locally running Docker machine.

The Loki starter-kit uses docker to run the following as docker containers:

- [loki](https://grafana.com/docs/loki/latest/)
- [promtail](https://grafana.com/docs/loki/latest/clients/promtail/)
- [fiberplane proxy](https://docs.fiberplane.com/quickstart/set-up-the-fiberplane-proxy)

### Pre-requisites

- [ ]  **[docker](https://docs.docker.com/get-docker/) and [docker-compose](https://docs.docker.com/compose/install/)** - Installed and running. These are used  to run containers locally.
- [ ]  **[Fiberplane CLI](https://docs.fiberplane.com/quickstart/set-up-the-fiberplane-proxy-with-the-cli#3b87d09b5321477cb6268caed0b5f9ae)** - Enables you to create & manage templates, notebooks, proxy API keys and more via cli.
- [ ]  **Proxy API token** - Can be created either on [Fiberplabe Studio (UI)](https://docs.fiberplane.com/quickstart/set-up-the-fiberplane-proxy#cae32ee6460b490a98aa0ecf7fd82a71) or via [CLI](https://docs.fiberplane.com/quickstart/set-up-the-fiberplane-proxy-with-the-cli#5bcaeea073a043f3a384d3f35640ca1e).

It's possible to replace docker with [podman](https://podman.io/) when using our starter kits. If you wish to do so, we recommend installing [podman-compose](https://github.com/containers/podman-compose) and replacing all `docker-compose` commands with `podman-compose`. All other aspects remain the same.

## docker-compose

- `git clone https://github.com/fiberplane/quickstart.git`
- `cd starter-kits/loki/docker-compose`
- Edit the .env file (`vi .env`) and add in the proxy API token generated in step 3 of the [Pre-Requisites](Grafana%20Loki%20108c2f8f54ae49ba8f67fd527f1149c8.md). eg : `TOKEN=yourtokenhere`
- `docker-compose up -d`

## Verifying the installation

Verify that the containers are running (run `docker ps`).

Here is a sample output of what you should see

![Untitled](Grafana%20Loki%20108c2f8f54ae49ba8f67fd527f1149c8/Untitled.png)

## Exploring the data on Fiberplane

Now that you have everything setup, you are ready to create your first notebook. We recommend following the instructions below to do so using our starter kit template.

1. Get the template`cd ../` (loki directory)
2. Add the template jsonnet file to Fiberplane: `fp templates create --title="My loki template" fp-loki-starter-template.jsonnet` >> Make a note of the template id.
3. Now you can create a notebook using the template you just created. On Fiberplane studio, click on templates >> your template >> New notebook >> Create Notebook. This is also doable using CLI, just make sure you have the template id from step 2 - `fp templates expand <templateid>`
4. Click the notebook link and you will have a notebook with few loki queries pre-populated. Select your datasource `my Loki`

![Untitled](../Set%20up%20Prometheus%20with%20Fiberplane%20d40f05dec1f74ad280420473c4ed4ad6/Untitled%202.png)

1. Now execute each query by hitting CTRL + ↩︎ (Mac: ⌘ + ↩︎) or by clicking the play button next to each query cell.

## Next Steps : Prometheus + Loki

Why not add Loki to the mix to see some logs as well on Fiberplane? 

There is a Prometheus + Loki starter kit that does this. To do so:

- `cd starter-kits/prometheus+loki`
- Edit the .env file and add the Proxy API token generated in step 2. e.g : `FP_PROXY_TOKEN=<yourtokenhere>`. (You can use the same token you already used for the Prometheus starter kit.)
- `docker-compose up -d`

You will now see that there 4 containers running,

![Untitled](../Set%20up%20Prometheus%20with%20Fiberplane%20d40f05dec1f74ad280420473c4ed4ad6/Untitled.png)

### Now to see both metrics and logs data in Fiberplane

- `git clone [https://github.com/fiberplane/quickstart.git](https://github.com/fiberplane/quickstart.git)` (skip this step if you have already cloned it)
- Get the template `cd ../prometheus+loki`
- Add the template jsonnet file to Fiberplane: `fp templates create --title="Prometheus and Loki template" fp-prom-loki-starter-template.jsonnet` >> Make a note of the template id.
- Now you can create a notebook using the template you just created. On Fiberplane studio, click on templates >> your template >> New notebook >> Create Notebook. This is also doable using CLI, just make sure you have the template id from step 2 - `fp templates expand <templateid>`
- Click the notebook link and you will have a notebook with few prometheus & loki queries pre-populated. Select your datasources `my Prometheus` and `my Loki`

![Untitled](../Set%20up%20Prometheus%20with%20Fiberplane%20d40f05dec1f74ad280420473c4ed4ad6/Untitled%201.png)

![Untitled](../Set%20up%20Prometheus%20with%20Fiberplane%20d40f05dec1f74ad280420473c4ed4ad6/Untitled%202.png)

- Now execute each query by hitting CTRL + ↩︎ (Mac: ⌘ + ↩︎) or by clicking the play button next to each query cell.

The full documentation for using notebooks and templates can be found on our [documentation site](https://docs.fiberplane.com/)
