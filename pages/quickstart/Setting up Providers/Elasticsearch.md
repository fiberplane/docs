# Elasticsearch

Use Elasticsearch to view logs from your infrastructure and application. If you don’t have a running Elasticsearch instance, you can use our starter kit to start it up - find instructions below!

---

# Setup

In order to enable Elasticsearch Provider in Fiberplane you need to either add it via direct access (for an openly accessible URL) or add it as a valid entry in the `data_source.yaml` configuration.

The `data_sources.yaml` format for adding a Elasticsearch Provider looks like this:

```yaml
# data_sources.yaml
#
# Replace the following line with the name of the data source
- name: elasticsearch-dev
  description: Elasticsearch (Dev)
  providerType: elasticsearch
  config:
    # Replace the following line with your Elasticsearch URL (including Basic Auth) and port
    url: http://username:password@elasticsearch:9200
    # If accessing your Elasticsearch data source requires an API key - add it here
    token: <ApiKey>
```

# Starter kit

The Elasticsearch starter-kit uses docker to run the following as docker containers:

- [elasticsearch](https://www.elastic.co/elasticsearch/)
- [fluentbit](https://docs.fluentbit.io/manual)
- [fiberplane proxy](https://docs.fiberplane.com/quickstart/set-up-the-fiberplane-proxy)

## Pre-Requisites

- [ ]  **[docker](https://docs.docker.com/get-docker/) and [docker-compose](https://docs.docker.com/compose/install/)** - Installed and running. These are used  to run containers locally.
- [ ]  **[Fiberplane CLI](https://docs.fiberplane.com/quickstart/set-up-the-fiberplane-proxy-with-the-cli#3b87d09b5321477cb6268caed0b5f9ae)** - Enables you to create & manage Templates, Notebooks, Proxy API keys and more via the CLI.
- [ ]  **Proxy API token** - Can be created either on [Fiberplane Studio (UI)](https://docs.fiberplane.com/quickstart/set-up-the-fiberplane-proxy#cae32ee6460b490a98aa0ecf7fd82a71) or via [CLI](https://docs.fiberplane.com/quickstart/set-up-the-fiberplane-proxy-with-the-cli#5bcaeea073a043f3a384d3f35640ca1e).

It's possible to replace docker with [podman](https://podman.io/) when using our starter kits. If you wish to do so, we recommend installing [podman-compose](https://github.com/containers/podman-compose) and replacing all `docker-compose` commands with `podman-compose`. All other aspects remain the same.

## docker-compose

- `git clone https://github.com/fiberplane/quickstart.git`
- `cd quickstart/starter-kits/elastic/`
- Edit the .env file (`vi .env`) and add in the proxy API token generated in step 3 of the [Pre-Requisites](Elasticsearch%2067e7fae5cc4041a2bd1c894085d86141.md). eg : `FP_PROXY_TOKEN=yourtokenhere`
- `docker-compose up -d`

## Verifying the installation

Verify that the containers are running (run `docker ps`).

Here is a sample output of what you should see

![Untitled](Elasticsearch%2067e7fae5cc4041a2bd1c894085d86141/Untitled.png)

## Exploring the data on Fiberplane

Now that you have everything setup, you are ready to create your first notebook. We recommend following the instructions below to do so using our starter kit template.

1. Get the template`cd ../` (elastic directory)
2. Add the template jsonnet file to Fiberplane: `fp templates create  fp-elastic-starter-template.jsonnet` >> Make a note of the template id.
3. Now you can create a notebook using the template you just created. On Fiberplane studio, click on templates >> your template >> New notebook >> Create Notebook. This is also doable using CLI, just make sure you have the template id from step 2 - `fp templates expand <templateid>`
4. Click the notebook link and you will have a notebook with few elastic queries pre-populated. Select your datasource `my elastic`

![Untitled](Elasticsearch%2067e7fae5cc4041a2bd1c894085d86141/Untitled%201.png)

1. Now execute each query by hitting CTRL + ↩︎ (Mac: ⌘ + ↩︎) or by clicking the play button next to each query cell.