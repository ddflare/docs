# ddflare Library

Library documentation is published at https://pkg.go.dev/github.com/ddflare/ddflare .

## Quickstart

Updating a DNS type A record with the actual Public IP address is achieved with 4 steps:

1. create a new _DNSManager_  (`NewDNSManager()`)
2. initialize the DNSManager with the authentication credentials (`.Init()`)
3. retrieve the current public IP address (`GetPublicIP()`)
4. update the DNS record via the DNSManager (`.UpdateFQDN`)

```go title="example: update www.myddns.host at NoIP"
import "github.com/ddflare/ddflare"

func main() {
  // fqdn to be updated
  fqdn := "www.myddns.host"
  // auth Token from the DDNS service if available or contatenation of user:password
  authToken := "user:password"

  // create a new DNSManager targeting the desired service (ddflare.Cloudflare,
  // ddflare.NoIP or ddflare.DDNS). For a DDNS provider using the DynDNS API v3
  // but not in the list, pick ddflare.DDNS and set a custom API endpoint with
  // dm.SetAPIEndpoint("$HTTP_ENDPOINT")
  dm, err := ddflare.NewDNSManager(ddflare.NoIP)
  if err != nil {
    log.Fatal(err)
  }

  // init the DNSManager with the API credentials
  if err = dm.Init(authToken); err != nil {
    log.Fatal(err)
  }

  // retrieve the current Public IP address
  pubIP, err := ddflare.GetPublicIP()
  if err != nil {
    log.Fatal(err)
  }
  // set the Public IP address just retrieved as the addres of `fqdn`
  if err = dm.UpdateFQDN(fqdn, pubIP); err != nil {
    log.Fatal("update failed")
  }

  log.Info("update successful")
}
```

