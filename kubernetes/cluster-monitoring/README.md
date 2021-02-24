[source](https://github.com/carlosedp/cluster-monitoring)

I had an [error](https://github.com/prometheus-operator/kube-prometheus/issues/619) executing `make docker` from `WLS2`. It has been fixed by adding `sleep .2; ` in `scripts/build.sh` script:

```
$JSONNET_BIN -J vendor -m manifests "${1-example.jsonnet}" | xargs -I{} sh -c 'cat {} | $(go env GOPATH)/bin/gojsontoyaml > {}.yaml; rm -f {}' -- {}
                                                                               ^
                                                                               |
                                                                            sleep .2; 
```

To install `cluster-monitoring` in kubernetes cluster:

```
make docker

kubectl apply -f manifests/setup

kubectl apply -f manifests
```
