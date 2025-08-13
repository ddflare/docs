# Deploy on a Kubernetes cluster

ddflare can be run as a `Deployment` on a Kubernetes cluster.

In this case it is reccomended to store the service credentials in a Kubernetes `Secret`:
```bash
kubectl -n ddflare create secret generic my-domain --from-literal=token=<SERVICE_CREDENTIALS>
```
and pass it to the ddflare deployment via an environment variable.

## DDNS service Deployment
Prerequisite is to register for a No-IP account and get a FQDN and the authentication credentials as explained in the
[Update section](../ddflare-cli-update/#update-domain-name-via-ddns-services).

| Required data    | Sample value                  |
| ---------------- | ----------------------------- |
| _DDNS provider_  | [No-IP](https://www.noip.com/)|
| _FQDN to update_ | `myhost.ddns.net`             |
| _user_           | `68816xj`                     |
| _password_       | `v4UMHzugodpE`                |

Create the secret with:
```bash
kubectl -n ddflare create secret generic myhost.ddns.net \
    --from-literal=token=68816xj:v4UMHzugodpE
```

Apply the ddflare deployment yaml:

```yaml title="myhost-ddns-net.yaml"
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myhost.ddns.net
  namespace: ddflare
spec:
  selector:
    matchLabels:
      ddomain: myhost.ddns.net
  replicas: 1
  template:
    metadata:
      labels:
        ddomain: myhost.ddns.net
    spec:
      containers:
        - name: ddflare
          image: ghcr.io/ddflare/ddflare:0.7.0
          args: ["set", "-s", "noip", "-l", "myhost.ddns.net"]
          env:
            - name: DDFLARE_API_TOKEN
              valueFrom:
                secretKeyRef:
                  name: myhost.ddns.net
                  key: token
```
with:
```bash
kubectl apply -f myhost-ddns-net.yaml
```

## Cloudflare Deployment
Prerequisite is to have registered a domain with Couldflare and have created a _type A record_ and a _Cloudflare API token_
as explained in the [Update section](../ddflare-cli-update/#update-domain-name-via-cloudflare).

| Required data    | Sample value |
| ---------------- | ------------ |
| _FQDN to update_ | `myhost.example.com`|
| _API token_      | `gB1fOLbl2d5XynhfIOJvzX8Y4rZnU5RLPW1hg7cM`|

Create the secret with:
```bash
kubectl -n ddflare create secret generic myhost.example.com \
    --from-literal=token=gB1fOLbl2d5XynhfIOJvzX8Y4rZnU5RLPW1hg7cM
```


Apply the ddflare deployment yaml:
```yaml title="myhost-example-com.yaml"
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myhost.example.com
  namespace: ddflare
spec:
  selector:
    matchLabels:
      ddomain: myhost.example.com
  replicas: 1
  template:
    metadata:
      labels:
        ddomain: myhost.example.com
    spec:
      containers:
        - name: ddflare
          image: ghcr.io/ddflare/ddflare:0.7.0
          args: ["set", "-l", "myhost.example.com"]
          env:
            - name: DDFLARE_API_TOKEN
              valueFrom:
                secretKeyRef:
                  name: myhost.example.com
                  key: token
```
with:
```bash
kubectl apply -f myhost-example-com.yaml
```
