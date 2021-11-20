# Kind Playground

> **Remark**
>
> As I had troubles with DNS resolution within a container I followed the suggestion in [this stackoverflow post](https://serverfault.com/questions/960362/host-dns-not-working-with-docker-container/963155#comment1415366_963155)

Create cluster

```bash
kind create cluster --config kind-config.yaml
```

add ingress

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

Now the Ingress is all setup. Wait until is ready to process requests running:

```bash
kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=90s
```

Creates simple http-echo services and an Ingress object to route to these services.

```bash
kubectl apply -f https://kind.sigs.k8s.io/examples/ingress/usage.yaml
```

Now verify that the ingress works

```bash
# should output "foo"
curl localhost/foo
# should output "bar"
curl localhost/bar
```
