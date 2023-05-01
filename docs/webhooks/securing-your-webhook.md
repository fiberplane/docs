---
title: Securing your webhook
excerpt: Ensure your server only handles requests sent by Fiberplane
category: 644f7777d0901a002b72fa8a
slug: webhooks-security
---

After you created your webhook, you received a shared secret which is a random 32 character hexadecimal string.
If you did not save it, you can update the webhook and request a regeneration of the shared secret.

The request body will be hashed signed using that shared secret using the algorithm **HMAC SHA-512**. The signature
will be sent with every request in the `X-Fiberplane-Signature` header. The value of the header is `v1=[signature]`.

## Python example

We can extend our server from the previous chapter to verify the signature:

```python
from flask import Flask, request
import bytes
import os
import hmac
import hashlib

shared_secret = bytes.fromhex(os.environ['FIBERPLANE_WEBHOOK_SHARED_SECRET'])
app = Flask(__name__)

def verify_payload(data, signature):
    digest = hmac.new(shared_secret, bytes(data, 'utf8'), hashlib.sha512).hexdigest()
    return hmac.compare_digest(digest, signature)

@app.route("/delivery", methods=["POST"])
def handle_delivery():
    if not verify_payload(request.data, request.headers.get("X-Fiberplane-Signature")[3:]):
        return "Mismatching signature", 401

    payload = request.get_json(force=True)
    
    print(f"Received delivery from Fiberplane: {payload}")
    return "OK", 200
```

Now we will check every incoming request for a matching signature found in the `X-Fiberplane-Signature` header.
If the verification fails, we will respond with status code 401.
