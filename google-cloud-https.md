# Google Cloud HTTPS

This howto summarizes, in a few code snippets, how to set up an HTTPS
endpoint for a Kubernetes ingress on Google Cloud.

In the following, we'll use `$name` to denote a custom name that you have
picked for your endpoint.

## 1. Create a Google-managed SSL certificate

We'll use the name `$name` and the domain `$domain`.

```sh
gcloud beta compute ssl-certificates create \
  $name --domains $domain --global
```

Example:

```sh
gcloud beta compute ssl-certificates create \
  foo --domains foo.bar.com --global
```

## 2. Obtain the URL map used by the Kubernetes ingress

Assuming the ingress is called `$ingress`, run:

```sh
gcloud compute url-maps list \
  --filter="name=$ingress" \
  --format json \
  | jq -r '.[0].name'
```

Remember this name.

## 3. Create an HTTPS proxy

```sh
gcloud compute target-https-proxies create \
  $name-https \
  --url-map=<NAME_FROM_BEFORE> \
  --ssl-certificates=$name
```

## 4. Create a public static IP address

```sh
gcloud compute addresses create $name --global
```

## 5. Create a forwarding rule

```sh
gcloud compute forwarding-rules create \
  $name-https \
  --global \
  --ip-protocol TCP \
  --ports 443 \
  --target-https-proxy $name-https \
  --address $name
```

## 6. Add a DNS record for the IP address

First, obtain the static IP address:

```sh
# Obtain the IP address (via the "address" field in the output):
gcloud compute addresses describe $name --global
```

Then, add a DNS `A` record for this IP address pointing to your `$domain`.

Example using `now`:

```
now dns add bar.com foo A <IP_ADDRESS>
```

where `bar.com` is the top-level domain and `foo` (can be empty) is the
subdomain.
