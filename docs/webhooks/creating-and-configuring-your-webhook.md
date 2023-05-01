---
title: Creating & configuring your webhook
excerpt: Learn how to configure your webhook and how to receive your first delivery
category: 644f7777d0901a002b72fa8a
slug: webhooks-setup
---

Now that we understand [what webhooks are and how they work](doc:webhooks), we can create our own
example webhook subscriber which will receive deliveries.

Creating a webhook is a two-step process, first having to set up the subscriber which receives
Fiberplane's payloads and then configuring Fiberplane to send the payloads to it.

## Writing a subscriber

A subscriber is a HTTP server which will receive deliveries from Fiberplane. The subscriber needs to:

- Be able to receive HTTP `POST` requests using `HTTP/1.1`
- Be able to read the `application/json` body.
  **Note**: Fiberplane will send deliveries with a custom `Content-Type`: `application/vnd.fiberplane.webhook+json`
- Be not more than five redirects away

For the example of this tutorial, we will use this simple Python 3.0 server:

```python
from flask import Flask, request

app = Flask(__name__)

@app.route("/delivery", methods=["POST"])
def handle_delivery():
    payload = request.get_json(force=True)
    
    print(f"Received delivery from Fiberplane: {payload}")
    return "OK", 200
```

Run the above example with

```shell
$ python3 -m flask run --host=0.0.0.0 --port=62113
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on all addresses (0.0.0.0)
 * Running on http://127.0.0.1:62113
 * Running on http://172.18.119.222:62113
```

Now the server will listen for network wide incoming connections on port `62113`.
However, we will not be able to reach it as we're most likely behind a NAT.

We can work around this by using a HTTP tunnel such as [`ngronk`](https://ngrok.com/),
[Cloudflare Tunnels](https://www.cloudflare.com/products/tunnel/), [Tailscale Funnel](https://tailscale.com/kb/1223/tailscale-funnel/)
or [`tunnel.pyjam.as`](https://tunnel.pyjam.as/). The last one only requires Wireguard and
is a simple one-liner to get set up and running:

```shell
$ curl https://tunnel.pyjam.as/62113 > tunnel.conf && wg-quick up ./tunnel.conf
You can now access http://0.0.0.0:62113 on https://mwk8xc.tunnel.pyjam.as/ ✨
```

## Setting up the webhook

You can create a webhook using Fiberplane Studio or using the `fp` CLI.

To set up a webhook in Fiberplane Studio, go to your workspace settings and click `Webhooks`.
From there, click on `+ Add webhook` and fill in the fields described below.

To set up a webhook with the `fp` CLI, simply type `fp webhooks create` and fill in the questions
it asks you. The fields are described below.

## Endpoint URL

The endpoint URL is the URL to which Fiberplane will send deliveries using a HTTP `POST` request.

Since we're developing locally for the purpose of this tutorial, we'll set it to the URL which
`tunnel.pyjam.as` gave us above, followed by `/delivery`. Example: `https://mwk8xc.tunnel.pyjam.as/delivery`

If you are configuring a custom endpoint which uses HTTPS, please ensure you are running TLS 1.2 or higher. The
certificate must be trusted by the Mozilla Trust Store and cannot be self-signed.

## Endpoint URL Limitations

- Your endpoint needs to respond within 30 seconds
- Your endpoint URL must resolve to a global reachable IP address (not `127.0.0.1`)
- A maximum of 5 redirects will be followed
- The endpoint may be either HTTP or HTTPS
- If HTTPS, self-signed certificates are not allowed. The certificate must be trusted by the Mozilla Trust Store

## Events

You can select a list of categories for which your endpoint will receive deliveries. You cannot
select individual events for which you want to receive payloads, for example you can't subscribe
to only the `frontmatter.update` event but only to the whole `frontmatter` category, which also includes
the `frontmatter.delete` event.

For a complete list of available webhook events and their payloads,
see [Webhook events and payloads](doc:webhook-events).
