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

## 4. Download Opni

```bash
sudo curl -s -o /usr/local/bin/opnictl -fsSL https://github.com/rancher/opni/releases/download/v0.1.2/opnictl_linux-amd64
sudo chmod +x /usr/local/bin/opnictl
```

## 5. Install Opni

```bash
opnictl install
```
