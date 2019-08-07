# Let's Encrypt / Certbot

Restart web server after each command.

## Add a new certificate

```console
sudo certbot --nginx -d example.com
```

## Renew certificates

```console
sudo certbot renew
```

## Delete a certificate

```console
sudo certbot delete --cert-name example.com
```
