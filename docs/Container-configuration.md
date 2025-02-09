## Optional container environment variables for custom configuration.

* `ACME_CA_URI` - Directory URI for the CA ACME API endpoint (defaults to ``https://acme-v01.api.letsencrypt.org/directory``).

If you set this environment variable value to `https://acme-staging.api.letsencrypt.org/directory` the container will obtain its certificates from Let's Encrypt test API endpoint that don't have the [5 certs/week/domain limit](https://letsencrypt.org/docs/rate-limits/) (but are not trusted by browsers).

For example

```bash
docker run --detach \
    --name nginx-proxy-letsencrypt \
    --volumes-from nginx-proxy \
    --volume /path/to/certs:/etc/nginx/certs:rw \
    --volume /var/run/docker.sock:/var/run/docker.sock:ro \
    --env "ACME_CA_URI=https://acme-staging.api.letsencrypt.org/directory" \
    jrcs/letsencrypt-nginx-proxy-companion
```
You can also create test certificates per container (see [Test certificates](./Let's-Encrypt-and-ACME.md#test-certificates))

* `DEBUG` - Set it to `true` to enable debugging output one the container logs, which could help you troubleshoot configuration issues.

* `REUSE_ACCOUNT_KEYS` - Set it to `false` to disable the account keys reutilization (see [ACME account keys](./Let's-Encrypt-and-ACME.md#acme-account-keys)).

* `REUSE_PRIVATE_KEYS` - Set it to `true` to make `simp_le` reuse previously generated private key for each certificate instead of creating a new one on certificate renewal. Recommended if you intend to use [HPKP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Public_Key_Pinning) (please not however that HPKP has been deprecated by Google's Chrome and that its use is therefore not recommended).

* `DHPARAM_BITS` - Change the size of the Diffie-Hellman key generated by the container from the default value of 2048 bits. For example `--env DHPARAM_BITS=1024` to support some older clients like Java 6 and 7.
