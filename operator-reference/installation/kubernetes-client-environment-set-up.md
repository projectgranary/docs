# Kubernetes Client Environment Set Up

## Environment Set Up

In order to use the Segment Table Creation feature in Granary the tools kubectl and helm have to be set up on your local machine. The setup will be exemplary shown for Linux using the package managment tool snap. 

### snap

#### Description

Snap Apps \(short: snaps\) is a package format which can be used for conflict free package managment. 

**Installation:** 

```bash
$ sudo apt-get install snapd
```

### kubectl

#### **Description:** 

kubectl is the Kubernetes command-line tool, which allows to run commands against Kubernetes clusters. It can be used to deploy applications, inspect and manage cluster resources as well as viewing logs**.**

**Installation:** 

The installation will be exemplary shown for Linux using the package management tool snap, further installation options can be found [here](https://kubernetes.io/docs/tasks/tools/install-kubectl/). 

```
$ sudo snap install kubectl
```

**Configuration:**

In order to configure kubectl to access a Kubernetes cluster the config file has to be placed in the folder: ~/.kube. The config file is created automatically when setting up a Kubernetes cluster. Therefore the file can be provided to you by the Kubernetes Cluster Admin. 

1. Check that config file is available in the ~/.kube folder:

```text
$ cd ~/.kube
~/.kube$ ls
cache  config  http-cache
```

2. Check that kubectl is properly configured by getting the cluster state:

```text
$ kubectl cluster-info --namespace <namespace>
Kubernetes master is running at https:/<cluster-address>
```

#### Useful statements

1. List all running pods on Kubernetes cluster

```text
$ kubectl get pods
```

2. View pod's logs

```text
$ kubectl logs <pod name>
```

### helm

#### **Description:** 

Helm helps to manage Kubernetes applications — Helm Charts help you define, install, and upgrade  Kubernetes applications. Further information can be found [here](https://helm.sh/docs/intro/). 

**Installation:** 

The helm client has to be installed in the same version \(i.e. 2.16\) as the helm server which is running on Kubernetes. The current version has to be provided to you by Kubernetes Cluster Admin. In case of the helm version of the server is 2.16 helm is installed using this statement:

```text
$ sudo snap install helm --channel=2.16/stable –classic
```

#### Configuration

1. Retrieve the settings from the Helm Server running on Kubernetes to ensure the helm client is working correctly.

```text
$ helm init --client-only
```

2. Helm repositories references have to be added to helm. The repository details \(name and url\) can be provided by the Kubernetes Cluster Admin.

```text
$ helm repo add <repository name> <url>
```

3. The repositories have to be updated:

```text
$ helm repo update
```

4. Define the Kubernetes namespace which should be used to avoid adding the flag --namespace &lt;namespace&gt; to each helm statement

```text
$ export TILLER_NAMESPACE=<namespace>
```

5. Check the correct configuration by listing the charts available in the helm repository. 

```text
$ helm ls
```

#### Useful commands

The list below provide a few useful commands to get started with helm. These commands are working for the helm version 2.xx, for helm version 3.xx the statements have to be adjusted.

1. Help flag can be used for to find out which commands are available in helm. The help flag can be also used for each helm command to get further information

```text
$ helm --help
```

2a. All available charts in all repository can be listed via this command:

```text
$ helm search 
```

2b. All available charts in a specific repository can be listed via this command:

```text
$ helm search helm search <repository name>/
```

3. Install releases by using charts as templates. 

&lt;release name&gt; : Name of the installed chart

&lt;helm repo&gt; : Repository containing the helm chart used as template for the release

&lt;helm chart&gt; : Chart that is used as template for the release

&lt;configuration file&gt; : Yaml file which contains the configurations with which the release should be installed

```text
$ helm install --name <release name> <helm repo>/<helm chart> -f <configuration file>.yaml --namespace <kubernetes cluster namespace>
```

4. Releases in the can be found by using following command:

```text
$ helm ls
```

5. Values of a release's configuration file can be retrieved by using following command:

```text
$ helm get values <release name>
```

