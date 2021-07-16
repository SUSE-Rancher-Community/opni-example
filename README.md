# opni-example

## Prerequisites

Here's what you need to get started:

### Core

1. [kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl)
2. [git client](https://git-scm.com/downloads/guis)

### Vagrant

1. [Vagrant](https://www.vagrantup.com/)
2. [VirtualBox](https://www.virtualbox.org/) or [qemu](https://www.qemu.org/) or [kvm](https://www.linux-kvm.org/page/Main_Page)

### Rancher Desktop

1. [Rancher Desktop](https://github.com/rancher-sandbox/rancher-desktop/releases).

## 1. Clone Repository

```bash
git clone git@github.com:SUSE-Rancher-Community/opni-example.git
```

## 2. Install Cluster

### Using Vagrant

In the directory with the `Vagrant` file run the following command:

```bash
vagrant up
```

### Using Rancher Desktop

Simply start __Rancher Desktop__

Then go to `Preferences` > `Kubernetes Settings` and make sure the cluster has 8GB of memory.

## 5. Download Opni

### Mac

These commands will download Opni:

```bash
sudo curl -s -o /usr/local/bin/opnictl -fsSL https://github.com/rancher/opni/releases/download/v0.1.2/opnictl_darwin-amd64
```

Change permissions with this command:

```bash
sudo chmod +x /usr/local/bin/opnictl
```

### Linux

These commands will download Opni:

```bash
sudo curl -s -o /usr/local/bin/opnictl -fsSL https://github.com/rancher/opni/releases/download/v0.1.2/opnictl_linux-amd64
```

Change permissions with this command:

```bash
sudo chmod +x /usr/local/bin/opnictl
```

### Windows

Download [Opni](https://github.com/rancher/opni/releases/download/v0.1.2/opnictl_windows-amd64.exe) and add it to your `PATH`.

## 6. Install Opni

This command will install Opni on your cluster:

```bash
opnictl install
```

## 7. Create Demo

This command will create the demo but it will take a few minutes to run:

```bash
opnictl create demo --deploy-gpu-services=false --deploy-helm-controller=true --deploy-nvidia-plugin=false --deploy-rancher-logging=true --timeout 15m
```

You will get an output similar but not exact to this:

```bash
 ✗ Done.                                                                                
 ✓ [Done] Waiting for job helm-install-rancher-logging-crd to complete                  
 ✓ [Done] Waiting for job helm-install-rancher-logging to complete                      
 ✓ [Done] Waiting for an install job to start for minio                                 
 ✓ [Done] Waiting for job helm-install-nats (Job.batch "helm-install-nats" not found)   
 ✓ [Done] Waiting for an install job to start for opendistro-es                         
 ✓ [Done] Waiting for job helm-install-minio to complete                                
 ✓ [Done] Waiting for job helm-install-nats to complete                                 
 ✓ [Done] Waiting for deployment drain-service to become ready                          
 ✓ [Done] Waiting for deployment nulog-inference-service-control-plane to become ready  
 ✓ [Done] Waiting for deployment preprocessing-service to become ready                  
 ✓ [Done] Waiting for deployment payload-receiver-service to become ready               
 ✓ [Done] Waiting for job helm-install-minio (Job.batch "helm-install-minio" not found) 
 ✓ [Done] Waiting for job helm-install-opendistro-es to complete                        
 ✓ [Done] Waiting for prerequisite statefulset opendistro-es-master to become ready     
 ✓ [Done] Waiting for prerequisite deployment opendistro-es-kibana to become ready      
 ✓ [Done] Waiting for prerequisite statefulset opendistro-es-data to become ready       
 ✓ [Done] Waiting for prerequisite deployment opendistro-es-client to become ready  
```

## 8. Get NodePort

This command will grab the nodePort:

```bash
kubectl -n opni-demo get -o jsonpath="{.spec.ports[0].nodePort}" services opendistro-es-kibana-svc
```

## 9. Open up Elastic

In a web browser on your local machine go to the following url:

`http://<ip address>:<nodePort>`

NOTE: `<nodePort>` is the nodePort from step 8 and the `<ip address>` is that of the cluster.

You can then log in with these credentials username `admin` and password `admin`.

## 10. Inject Anomalies

Run the following commands:

```bash
kubectl apply -f nonexistent_image_pods.yaml
kubectl apply -f nonzero_exit_code_pods.yaml
```
