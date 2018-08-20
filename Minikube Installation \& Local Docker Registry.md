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

## Local Docker Registry

To setup a local Docker registry

