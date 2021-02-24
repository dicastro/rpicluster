[source](https://github.com/rancher/k3s-ansible)

To run the playbook:

```
ANSIBLE_CONFIG=ansible.cfg ansible-playbook main.yml
```

# TODO

### kube-controller-manager and kube-scheduler

Ask in github about these two pieces in order to understand them and in order to fix the issue with prometheus-operator.

### Activate traefik dashboard

In order to activate the traefik dashboard I have had to ssh the master and edit `/var/lib/rancher/k3s/server/manifests/traefik.yaml` file, adding lines:

```yaml
spec:
  valuesContent: |-
    ...
    dashboard:
      enabled: true
      domain: "traefik.192.168.86.51.nip.io"
    ...
```

**This config is lost when the master node is rebooted**

> Edit ansible playbook to automate this config

# Articles

### Series explaining how to install an HA K3s Cluster (using etcd)

[How Rancher Labs’ K3s Makes It Easy to Run Kubernetes at the Edge](https://thenewstack.io/how-rancher-labs-k3s-makes-it-easy-to-run-kubernetes-at-the-edge/)
[Tutorial: Set up a Secure and Highly Available etcd Cluster](https://thenewstack.io/tutorial-set-up-a-secure-and-highly-available-etcd-cluster/)
[Tutorial: Install a Highly Available K3s Cluster at the Edge](https://thenewstack.io/tutorial-install-a-highly-available-k3s-cluster-at-the-edge/)