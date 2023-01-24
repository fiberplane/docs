# Quickstart

Fiberplane Studio is available for the [Web](https://fiberplane.com/).

To get started go to [https://fiberplane.com/](https://fiberplane.com/) and log in with your Fiberplane account (currently available only for Google Workspace users). You can also go right away to your notebook by typing [fp.new](https://fp.new/) in your URL, however, to save and store your notebook, you will need to log in.

![Proxy slide (1).png](Quickstart%20cce7b6fa53144989b6835feb900e32dc/Proxy_slide_(1).png)

## How Fiberplane works

Fiberplane Studio allows you to query, visualize, and understand metrics and logs in your infrastructure.

Whenever you execute a query in the notebook:

1. The query is forwarded to the Fiberplane Proxy in your cluster;
2. The Proxy then queries the Provider data source (e.g. your Prometheus or Elastic instance);
3. When the Provider returns the data, the Proxy processes, encrypts, and then returns it back to the Studio.

# Set up the Fiberplane Proxy

The Fiberplane Proxy is a package that runs in your infrastructure. It enables you to connect the Fiberplane Studio to data sources in your cluster(s) securely without exposing them to the Internet.

The Fiberplane Proxy is available as a [container on Docker Hub](https://hub.docker.com/r/fiberplane/proxy).

---

In order for the Proxy to receive queries from Fiberplane Notebooks, it needs to be authorized. This step will generate a **Proxy API Token** that will be needed in later steps.

You can do it in the Settings > Proxies menu or using our CLI tool.

## Generate a Proxy API Token in the Studio

![og-image_Best practices for observability (17).png](Quickstart%20cce7b6fa53144989b6835feb900e32dc/og-image_Best_practices_for_observability_(17).png)

In order for the Proxy to talk to the Fiberplane Studio successfully it needs to be successfully authorized. This step will generate a **Proxy API Token** that will be needed later.

1. Go to your Fiberplane [Settings page](https://fiberplane.com/settings).
2. Click **`+ New Proxy`** to register a proxy with a name that identifies the cluster you will install it into (for example, "Production"). This will generate and display a Proxy API Token that the proxy will use to authenticate with the Fiberplane Studio.
3. Copy the Proxy API Token generated in Step 2 for the next step.

## Generate a Proxy API Token using the CLI

Download and install the Fiberplane CLI

```bash
curl --proto '=https' --tlsv1.2 -sSf https://fp.dev/install.sh | sh
```

Or download the Fiberplane CLI manually (make sure to make it executable):

- [Linux X86](https://fp.dev/fp/latest/x86_64-unknown-linux-gnu/fp)
- [MacOS Arm64](https://fp.dev/fp/latest/aarch64-apple-darwin/fp)
- [MacOS X86](https://fp.dev/fp/latest/x86_64-apple-darwin/fp)

Authenticate your CLI with Fiberplane

```bash
fp login
```

To register a proxy run a command `fp proxies create`:

```bash
$ fp proxies create my-proxy-name
				Name:  my-proxy-name
          ID:  <generated_proxy_id>
      Status:  disconnected
 Datasources:  (none)
        Token  <YOUR_PROXY_API_TOKEN> <-- SAVE THIS FOR LATER!
```

You can leave out the proxy name to have one randomly generated, but we recommend naming it according to the environment it will be deployed to (for example, `production`). Note that proxy names must follow the [Fiberplane name format](Quickstart%20cce7b6fa53144989b6835feb900e32dc/Configuration%20help%20FAQ%2018941c30bc32404785f767ab1892c0ce.md).

# Next steps

You can now:

- add Providers;
- deploy the Proxy to Kubernetes cluster, Docker container or run it locally for testing.

## Add Providers

[Setting up Providers](Quickstart%20cce7b6fa53144989b6835feb900e32dc/Setting%20up%20Providers%20d28e28323f40453abee907f37dbfd2fb.md)

## Deploy

[Deploy to Kubernetes](Quickstart%20cce7b6fa53144989b6835feb900e32dc/Deploy%20to%20Kubernetes%2026d88884937c4c389afc99b191694da1.md)

[Deploy to Docker](Quickstart%20cce7b6fa53144989b6835feb900e32dc/Deploy%20to%20Docker%207f4609bf86cb43cb87527cd7b87b5834.md)

[Run Local (Docker)](Quickstart%20cce7b6fa53144989b6835feb900e32dc/Run%20Local%20(Docker)%20c92f31063d3d4dcb87479a41770a84c8.md)

---

[Configuration help / FAQ](Quickstart%20cce7b6fa53144989b6835feb900e32dc/Configuration%20help%20FAQ%2018941c30bc32404785f767ab1892c0ce.md)