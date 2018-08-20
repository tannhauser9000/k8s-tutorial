# Minikube Installation \& Local Docker Registry

In this document, we summarize the procedures for ***Mac*** users to initialize a local K8s cluster and prepare a local Docker registry for local experiment.

## Minikube

[Minikube](https://kubernetes.io/docs/setup/minikube/) is a tool to enable easy access to creation and operation of a local Kubernetes ***cluster***.

### Dependency

- Virtual machine Hypervisor
    - [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
        - `brew cask install virtualbox`
        - If [homebrew](https://brew.sh) is not available
            - Visit VirtualBox [official site](https://www.virtualbox.org/wiki/Downloads) for latest installer
    - Other VM Hypervisor

### Installation

[Latest release](https://github.com/kubernetes/minikube/releases) can be found on Kubernetes official GitHub repo.

The current latest version is `v0.28.2` and can be installed by the following command:

`curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.28.2/minikube-darwin-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/`

### Start Local Cluster

After getting all necessary components above, we can start the local Kubernetes cluster now:

`./create_local`

### Minikube Usage

In this section, we will have a brief illustration about the commands of `minikube`:

#### Information Retrieval & Debugging

- `dashboard`: Open or print the URL of K8s dashboard for the local cluster
  - Open dashboard: `minikube dashboard`
  - Print dashboard URL: `minikube dashboard --url`
- `docker-env`: Set up or unset Docker related environment variables
  - Set up: `eval $(minikube docker-env)`
  - Unset: `eval $(minikube docker-env -u)`
- `get-k8s-versions`: List available K8s versions for the minikube
- `ip`: Get Minikube cluster IP
- `logs`: Minikube cluster debugging log
- `service`: Print or open K8s service URL
  - List services: `minikube service list`
  - Open K8s dashboard: `minikube service kubernetes-dashboard -n kube-system`
  - Print URL for K8s dashboard: `minikube service kubernetes-dashboard -n kube-system --url`
- `ssh`: Connect to Minikube node via `ssh`
- `ssh-key`: Get path to ssh key for `minikube ssh`
- `update-check`: Check the current and latest version of Minikube
- `update-context`: Update context of Minikube cluster to `kubeconfig` (`~/.kube/config` by default)

#### Cluster Operating

Due to some customization of this tutorial, we strongly suggest use the provieded script in ***Start Local Cluster*** section to manipulate your Minikube clusters.

- `start`: Start the Minikube cluster
- `stop`: Stop the Minikube cluster (will cause the restart of `storage-provisioner` and `kubernetes-dashboard`)
- `delete`: Destroy the current Minikube cluster

## Local Docker Registry

To setup a local Docker registry, please use the following command:

`kubectl create -f https://raw.githubusercontent.com/tannhauser9000/k8s-tutorial/master/minikube_creation/docker-registry/local-registry.yml`

After creating the local Docker registry, you can use the following command to list the local docker registry:

```
export _LOCAL_DOCKER="http://$(minikube ip):5000";
curl -s ${_LOCAL_DOCKER}/v2/_catalog
```

