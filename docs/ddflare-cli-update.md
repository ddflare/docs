## Update domain name via DDNS services

In order to update a domain name with a DynDNS client you should register to a DDNS service first,
configure the desired FQDN there and retrieve the _authentication token_ or the _update key_ 
required to authenticate to the service.

!!!Tip
    Some DynDNS providers do not provide _authentication tokens_ or _update keys_: in that case
    the _authentication token_ can be generated using the user and password of the service account
    concatenated by a colon: `user:password`.

To update a domain name (FQDN, type A record) to the current public IP address of the host run:

```console
ddflare set -s <DDNS_URL> -t <AUTH_TOKEN> <FQDN>
```
where `<DDNS_URL>` is the http endpoint of the DDNS service,
`<FQDN>` is the domain to be updated and `<AUTH_TOKEN>` is the _Update Key_ or the _API authentication token_.

!!!Example
    | Required data    | Sample value                  |
    | ---------------- | ----------------------------- |
    | _DDNS provider_  | [No-IP](https://www.noip.com/)|
    | _FQDN to update_ | `myhost.ddns.net`             |
    | _user_           | `68816xj`                     |
    | _password_       | `v4UMHzugodpE`                |

    ```console title="update myhost.ddns.net at NoIP"
    ddflare set -s noip -t 68816xj:v4UMHzugodpE  myhost.ddns.net
    ```

## Update domain name via [Cloudflare](https://www.cloudflare.com/)

!!!Note
    To update a FQDN via Cloudflare you need to buy a domain at Cloudflare (or transfer it to).
    <br>
    You also need to create a _Cloudflare API token_ with `Zone.DNS Edit` permission following the
    [Cloudflare docs](https://developers.cloudflare.com/fundamentals/api/get-started/create-token/).

    ddflare can <ins>update</ins> existing type A DNS records but cannot create new ones yet, so the
    record should be created in advance.
    <br>
    To create a type A record see Cloudflare's
    [Manage DNS Records](https://developers.cloudflare.com/dns/manage-dns-records/how-to/create-dns-records/)
    docs.

    !!!Tip
        When creating a type A DNS record pay attention to the value of the `TTL` field:
        it tracks the number of seconds DNS clients and DNS resolvers are allowed to
        cache the resolved IP address.
        You may want to keep the TTL low (the allowed minimum is 60 secs) if you plan to use the record
        to track the (dynamic) IP address of a host in a DDNS scenario.

```bash
ddflare set -t <CLOUDFLARE_TOKEN> <FQDN>
```
where `<FQDN>` is the domain to be updated and `<CLOUDFLARE_TOKEN>` is the Cloudflare API token.

!!!Example
    | Required data    | Sample value |
    | ---------------- | ------------ |
    | _FQDN to update_ | `myhost.example.com`|
    | _API token_      | `gB1fOLbl2d5XynhfIOJvzX8Y4rZnU5RLPW1hg7cM`|

    ```console title="update myhost.example.com at Cloudflare"
    ddflare set -t gB1fOLbl2d5XynhfIOJvzX8Y4rZnU5RLPW1hg7cM  myhost.example.com
    ```
