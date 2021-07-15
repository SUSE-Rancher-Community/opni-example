# opni-example

## Prerequisites

Here's what you need to get started:

For k3s in vagrant:

1. [Vagrant](https://www.vagrantup.com/)
2. [VirtualBox](https://www.virtualbox.org/) or [qemu](https://www.qemu.org/) or [kvm](https://www.linux-kvm.org/page/Main_Page)
3. [kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl)
4. [git client](https://git-scm.com/downloads/guis)

## 1. Clone Repository

```bash
git clone git@github.com:SUSE-Rancher-Community/opni-example.git
```

## 2. Install Cluster

In the directory with the `Vagrant` file run the following command:

```bash
vagrant up
```

## 3. SSH

SSH in to the VM with the following command:

```bash
vagrant ssh
```

## 4. Update Kube Config

```bash
mkdir -p $HOME/.kube
# Copy the kube config from /etc/rancher/k3s/k3s.yaml to $HOME/.kube/config
sudo cp -i /etc/rancher/k3s/k3s.yaml $HOME/.kube/config
# Change ownership of the config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
# Update KUBECONFIG environment variable
export KUBECONFIG=$HOME/.kube/config
```

## 5. Download Opni

These commands will download Opni and change the permissions:

```bash
sudo curl -s -o /usr/local/bin/opnictl -fsSL https://github.com/rancher/opni/releases/download/v0.1.2/opnictl_linux-amd64
sudo chmod +x /usr/local/bin/opnictl
```

## 6. Install Opni

This command will install Opni on your cluster:

```bash
opnictl install
```

## 7. Create Demo

This command will create the demo but it will take a few minutes to run:

```bash
opnictl create demo --deploy-gpu-services=false --deploy-helm-controller=true --deploy-nvidia-plugin=false --deploy-rancher-logging=true --timeout 10m
```

## 8. Get NodePort

This command will grab the nodePort:

```bash
kubectl -n opni-demo get -o jsonpath="{.spec.ports[0].nodePort}" services opendistro-es-kibana-svc
```

## 9. Open up Elastic

In a web browser on your local machine go to the following url:

`http://192.168.33.11:<nodePort>`

NOTE: `<nodePort>` is the nodePort from step 8.

## 10. Inject Anomalies
