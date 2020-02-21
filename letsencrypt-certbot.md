# Let's Encrypt / Certbot

Restart web server after each command.

## Add a new certificate

Using nginx webroot:

```console
sudo certbot --nginx -d example.com
```

[Through CloudFlare CDN](https://bjornjohansen.no/wildcard-certificate-letsencrypt-cloudflare):

```console
sudo certbot certonly -d example.com --dns-cloudflare --dns-cloudflare-credentials /root/.secrets/cloudflare.ini
```

## Renew certificates

```console
sudo certbot renew
```

## Delete a certificate

```console
sudo certbot delete --cert-name example.com
```
