> Tried without success. Kubernetes cluster is installed successfully. Ansible playbook works. Missing *ingress-controller*. Tried *prometheus-operator (carlosedp)* and it does not work. I changed to pure kubernetes solution hoping to have prometheus monitoring solution working perfectly, but it is not the case.

[source](https://github.com/raspbernetes/k8s-cluster-installation/tree/master/ansible)

To run the playbook:

```
ANSIBLE_CONFIG=ansible.cfg ansible-playbook ./playbooks/all.yml
```

# Requirements

- The library `netaddr` of python is required in the host executing ansible in order to be able to validate ip addresses in playbooks (ex: `keepalived_vip | ipaddr`) [`pip3 install netaddr`]

# Changes

- In `family_vars/debian.yml` it's been added the dependency `systemd-timesyncd` because it is required in role `common/tasks/common.yml`

# Additional Steps

### Install Helm in master node (https://helm.sh/docs/intro/install/)

```
curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
sudo apt-get install apt-transport-https --yes
echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```

### Try to install traefik usin helm

```
helm repo add traefik https://helm.traefik.io/traefik
helm repo update

# to see installation details
helm install traefik traefik/traefik --namespace traefik-system --create-namespace --dry-run --debug

# to install it
helm install traefik traefik/traefik --namespace traefik-system --create-namespace
```

> La instalación de Traefik teniendo `flannel` como CNI me ha fallado. Con `calico` no falla.

### Install MetalLB

```
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.9.5/manifests/namespace.yaml
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.9.5/manifests/metallb.yaml
# On first install only
kubectl create secret generic -n metallb-system memberlist --from-literal=secretkey="$(openssl rand -base64 128)"
```

Create the configuration file:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: default
      protocol: layer2
      addresses:
      - 192.168.86.80-192.168.86.100
```

Apply configuration:

```
kubectl apply -f metallb-config.yaml
```

# TODO

- Tratar de instalar Traefik en el cluster y que sea accesible el dashboard
  - Mirar los tipos de Service de Kubernetes (LoadBalancer, ExternalIP)
  - Mirar lo que significa externalIP en un Service de Kubernetes

- Leer https://opensource.com/article/20/7/homelab-metallb
- Ejemplo de alguien que lo ha expuesto... pero solo tiene un nodo (https://medium.com/@evnsio/managing-my-home-with-kubernetes-traefik-and-raspberry-pis-d0330effea9a)
- Ejemplo usando K3s (https://blog.kubernauts.io/k3s-with-metallb-on-multipass-vms-ac2b37298589). Aunque yo diría que ahora K3s ya viene con MetalLB

- Documentacion Traefik (https://doc.traefik.io/traefik/user-guides/crd-acme/)
- Repo Helm Traefik Chart (https://github.com/traefik/traefik-helm-chart)

- Hacer la instalación de Helm con Ansible (y en todos los nodos master)
- Utilizar en la instalación de Kubernetes el mismo fichero de inventario
- Interesante creación de VMs con Ubuntu y cloud-init (https://multipass.run/) (compatible con Windows)