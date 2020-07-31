# HTTP Headers

## Headers to Reject for Security Reasons

The following headers should be rejected to avoid DoS attacks:

- `challenge-bypass-token` (CVE unknown)
- `via` (CVE unknown)
- `x_proxy_id` (CVE unknown)

An easy way to do this with NGINX is to reject requests that have these
headers set by adding the following to `nginx.conf` under `http > server`:

```NGINX
http {
  server {
    # Rejection flag
    set $reject 0;

    if ($http_challenge_bypass_token) {
      set $reject 1;
    }

    # These two don't seem to work on Google Cloud:
    if ($http_via) {
      set $reject 1;
    }
    if ($http_x_proxy_id) {
      set $reject 1;
    }

    # Reject all invalid requests
    if ($reject = 1) {
      return 400;
    }
  }
}
```
