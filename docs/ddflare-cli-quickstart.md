# Quickstart

## Update DynDNS FQDN
Update myhost.ddns.net at [NoIP](https://noip.com) with a
DDNS Key Username `68816xj` and a
DDNS Key Password `v4UMHzugodpE`:

=== "binary"
    ```console
    ddflare set -s noip -t 68816xj:v4UMHzugodpE  myhost.ddns.net
    ```
=== "docker"
    ```console
    docker run -ti --rm ghcr.io/ddflare/ddflare:0.7.0 set -s noip -t 68816xj:v4UMHzugodpE  myhost.ddns.net
    ```

## Get the current public IP address

Retrieving the current public IP address is as easy as running:

=== "binary"
    ```bash
    ddflare get
    ```
=== "docker"
    ```bash
    docker run -ti --rm ghcr.io/ddflare/ddflare:0.7.0 get
    ```
ddflare queries the `ipify.org` service under the hood, which detects the public IP address used to reach the service.

