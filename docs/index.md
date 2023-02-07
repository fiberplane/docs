---
title: Fiberplane Documentation
category: 63d7e8b49721540020f5c3ea
slug: home
---

Fiberplane Studio is available for the [Web](https://fiberplane.com/).

To get started go to [https://fiberplane.com/](https://fiberplane.com/) and log
in with your Fiberplane account (currently available only for Google Workspace
users). You can also go right away to your notebook by typing
[fp.new](https://fp.new/) in your URL, however, to save and store your notebook,
you will need to log in.

![Proxy slide (1).png](proxy_slide.png)

## How Fiberplane works

Fiberplane Studio allows you to query, visualize, and understand metrics and
logs in your infrastructure.

Whenever you execute a query in the notebook:

1. The query is forwarded to the Fiberplane Proxy in your cluster;
2. The Proxy then queries the Provider data source (e.g. your Prometheus or
	 Elastic instance);
3. When the Provider returns the data, the Proxy processes, encrypts, and then
	 returns it back to the Studio.

---

[Quickstart](doc:quickstart)

Everything you need to get started with Fiberplane!

[Notebooks](doc:notebooks)

Explore what you can do in the Fiberplane Studio

[Providers](doc:providers)

Query, analyze, and visualize your infrastructure data with full-stack
Fiberplane plugins

[CLI](doc:cli)

Get started with our general purpose CLI tool to set up Proxies, Templates, and
Triggers.

[Templates](doc:templates)

Programmatically create Fiberplane Notebooks for repeatable workflows.

---

Can’t find the answer to your question? Email us at
[support@fiberplane.com](mailto:support@fiberplane.com) or send us a message on
[Discord](https://discord.gg/MJr7pYzZQ4)!

Follow us on Twitter for updates: [@fiberplane](http://twitter.com/fiberplane)
