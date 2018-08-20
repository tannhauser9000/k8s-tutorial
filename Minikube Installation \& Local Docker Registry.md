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

```
wget https://raw.githubusercontent.com/user9000/k8s-tutorial/master/minikube_creation/mini-kube
chmod +x mini-kube
```

**Remark**: Before using this script, please ensure that the `minikube` binary is in your system `${PATH}`.

And the illustration of this script is as follows:

```
Usage: ./mini-kube <op> [flags]
  op:
    start: Start a local Minikube cluster
    stop: Stop a local Minikube cluster
    delete: Delete the local Minikube cluster
    list: List Minikube clusters
  flags:
    -p: Minikube cluster profile name, default: "minikube"

```

As a result, you can simply use the following commands to manipulate you local Minikube cluster:

- Initialze and start: `./mini-kube start`
- Suspend: `./mini-kube stop`
- Cleanup: `./mini-kube delete`
- List clusters: `./mini-kube list`

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

Due to some customization of this tutorial, use the provieded script in ***Start Local Cluster*** section to manipulate your Minikube clusters is **STRONGLY SUGGESTED**.

- `start`: Start the Minikube cluster
- `stop`: Stop the Minikube cluster (will cause the restart of `storage-provisioner` and `kubernetes-dashboard`)
- `delete`: Destroy the current Minikube cluster

### Cluster Switching between Multiple Minikube Clusters

You can use `kubectl config get-contexts` to list available K8s contexts, and use the following command to switch to the desired cluster:

```
kubectl config use-context <your cluster profile name>
```

#### More Detail about K8s Config

In K8s config, we have the following three kinds of entries:

- `user`: Authentication data, must have if RBAC (Role-based Access Control) is enabled.
- `cluster`: K8s cluster data, including address of the K8s clusters and the corresponding certificates.
- `context`: This type of entries combines the `cluster` and the `user` authentication data. Besides, it is also possible for a `context` to have its own default `namespace`.

However, the three entries for `minikube` will all be identical to the cluster profile name, which can be provided via `-p` parameter no matter in `minikube` or in our control script `mini-kube`.

For example, the following is the result of my `kubectl config get-contexts`, where `cluster_1` is created by `minikube` using `-p cluster_1` parameter:

```
CURRENT   NAME                          CLUSTER     AUTHINFO            NAMESPACE
*         cluster_1                     cluster_1   cluster_1           
          c1_user_default               c1          user_c1             default
          c1_user_johnwick              c1          user_c1             johnwick
          c1_user_parrot                c1          user_c1             parrot
          m1-user-default               m1          user-m1             default
```

Thus, to switch between the K8s configs generated by `minikube`, we can merely use the `kubectl config use-context` command. 

**Remark**: As metioned in ***Start Local Cluster*** section, the default cluster profile name is `minikube`.

## Local Docker Registry

To setup a local Docker registry, please use the following command:

`kubectl create -f https://raw.githubusercontent.com/user9000/k8s-tutorial/master/minikube_creation/docker-registry/local-registry.yml`

After creating the local Docker registry, you can access the RESTful API of the local docker registry use the following command:

```
export _CLUSTER_NAME=<your cluster profile name>
export _LOCAL_DOCKER="http://$(minikube ip -p ${_CLUSTER_NAME}):5000";
curl -s ${_LOCAL_DOCKER}/v2/_catalog
```

