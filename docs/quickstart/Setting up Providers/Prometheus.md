---
title: Prometheus
category: 63ce4f6008ed7e0016ef2eaa
---
# Prometheus

Use Prometheus metrics to monitor your infrastructure and application. If you don’t have a running Prometheus instance, you can use our starter kit to start it up - instructions below!

---

# Setup

In order to enable Prometheus Provider in Fiberplane you need to either add it via direct access (for an openly accessible URL) or add it as a valid entry in the `data_source.yaml` configuration.

The `data_sources.yaml` format for adding a Prometheus Provider looks like this:

```yaml
# data_sources.yaml
#
# Replace the following line with the name of the data source
- name: prometheus-prod
  description: Prometheus (Production)
  providerType: prometheus
  config:
    # Replace the following line with your Prometheus URL
    url: http://prometheus
    # If accessing your Prometheus requires authentication - add the token below
    token: <TOKEN>
```

# Starter kit

The Prometheus starter-kit uses Docker to run the following as Docker containers:

- [prometheus](https://prometheus.io/)
- [prometheus node exporter](https://prometheus.io/docs/guides/node-exporter/)
- [fiberplane proxy](https://docs.fiberplane.com/quickstart/set-up-the-fiberplane-proxy)

### Pre-Requisites

- [ ]  **[docker](https://docs.docker.com/get-docker/) and [docker-compose](https://docs.docker.com/compose/install/)** - Installed and running. These are used  to run containers locally.
- [ ]  **[Fiberplane CLI](https://docs.fiberplane.com/quickstart/set-up-the-fiberplane-proxy-with-the-cli#3b87d09b5321477cb6268caed0b5f9ae)** - Enables you to create & manage templates, notebooks, proxy API keys and more via cli.
- [ ]  **Proxy API token** - Can be created either on [Fiberplane Studio (UI)](../../Quickstart%20cce7b6fa53144989b6835feb900e32dc.md) or via [CLI](../../Quickstart%20cce7b6fa53144989b6835feb900e32dc.md).

It's possible to replace docker with [podman](https://podman.io/) when using our starter kits. If you wish to do so, we recommend installing [podman-compose](https://github.com/containers/podman-compose) and replacing all `docker-compose` commands with `podman-compose`. All other aspects remain the same.

## docker-compose

- Clone the [quickstart](http://github.com/fiberplane/quickstart) repository `git clone https://github.com/fiberplane/quickstart.git`
- Go to the relevant directory: `cd quickstart/starter-kits/prometheus/`
- Create an `.env` file and add in the proxy API token generated in step 3 of the [Pre-Requisites](Prometheus%208a01211885ac42659637a274d6b4c38d.md). e.g : `FP_PROXY_TOKEN=yourtokenhere`
- Spin it up! `docker-compose up -d`

## Verify the installation

Verify that the containers are up and running (run `docker ps`).

Here is a sample output of what you should see

![Untitled](Prometheus%208a01211885ac42659637a274d6b4c38d/Untitled.png)

## Explore the data on Fiberplane

Now that you have everything setup, you are ready to create your first notebook. We recommend following the instructions below to do so using our starter kit template.

1. Get the template - `cd ../` (prometheus directory)
2. Add the template jsonnet file to Fiberplane: `fp templates create fp-prom-starter-template.jsonnet` >> Make a note of the template id.
3. Now you can create a notebook using the template you just created. On Fiberplane studio, click on templates >> your template >> New notebook >> Create Notebook. This is also doable using CLI, just make sure you have the template id from step 2 - `fp templates expand <templateid>`
4. Click the notebook link and you will have a notebook with few prometheus queries pre-populated. Select your datasource `my Prometheus`

![Untitled](Prometheus%208a01211885ac42659637a274d6b4c38d/Untitled%201.png)

1. Now run each query by hitting CTRL + ↩︎ (Mac: ⌘ + ↩︎) or by clicking the play button next to each query cell.

![Untitled](Prometheus%208a01211885ac42659637a274d6b4c38d/Untitled%202.png)

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
