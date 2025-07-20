# ddflare CLI

ddflare CLI is a DynDNS client built upon the ddflare library.
It allows to update available DynDNS records at
[DynDNS update prococol v3](https://help.dyn.com/remote-access-api/)
compliant DynDNS providers.
ddflare CLI is also compatibile with the [Cloudflare API](https://developers.cloudflare.com/api/)
and allows to update existing DNS records registered at Cloudflare.

## Get ddflare

ddflare CLI is released as statically compiled binaries for different OS/architetures that you can grab
from the [release page](https://github.com/ddflare/ddflare/releases/latest).

```console title="example: download and install ddflare x86_64 binary"
wget https://github.com/ddflare/ddflare/releases/download/v0.7.0/ddflare-linux-amd64

sudo install ddflare-linux-amd64 /usr/local/bin/ddflare
```

ddflare CLI is also available as
[container images on the github registry](https://github.com/ddflare/ddflare/pkgs/container/ddflare).

```console title="example: run ddflare in Docker"
docker run -ti --rm ghcr.io/ddflare/ddflare:0.7.0
```

## Commands

ddflare has two main commands:

* `set` - updates the type A record of the target FQDN
to the current public IP address of the the client (or updates to the IP address passed
as argument).
* `get` - retrieves the current public IP address of the client
(or resolves the FQDN passed as argument).

Run `ddflare help` to display all the available commands and options.

