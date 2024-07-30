```
helm init
```
# Create the helmchart
```
helm create webapp1
```

# Install the first one
```
Syntax - helm install releaseName* appfolder* --values values.yml
helm install mywebapp-release webapp1/ --values mywebapp/values.yaml
```

# Upgrade after first install. It will rerun and you will see revision# incremented
```
helm upgrade mywebapp-release webapp1/ --values mywebapp/values.yaml
```

# Troubleshoot or check before actuall install
```
helm template mywebapp-release webapp1/ --values mywebapp/values.yaml
```

# Accessing it
```
minikube tunnel
```

# Create dev/prod
```
k create namespace dev
k create namespace prod
helm install mywebapp-release-dev webapp1/ --values webapp1/values.yaml -f webapp1/values-dev.yaml -n dev
helm install mywebapp-release-prod webapp1/ --values webapp1/values.yaml -f webapp1/values-prod.yaml -n prod
helm ls --all-namespaces
```

```
Example-
helm install dev-release1 webapp1 --values webapp1/values.yaml -f webapp1/environments/dev/values.yml
```

# Set placeholders for values
## Templates and Values.yml are important files
```
<b>Template you add all kubernetes manifest file. Like any other manifest</b> Next instead of assigning values..we want dynamic at runtime of helm
So change value to {{ .Values.Abc.cde}}.  e.g - {{ .Values.Service.port }}

Next in Values.yml. You provide actual values like
Values.yml
Service
  port: 8080
  type: Nodeport
```
```
metadata:
  name: {{ .Values.appName }}
  namespace: {{ .Values.namespace }}
  labels:
    app: {{ .Values.appName }}
```

# Set values in values.yml
```
appName: devhelmapp
configname: devconfig
namespace: dev
```
```
Add repo
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install my-wordpress bitnami/wordpress --version 10.1.4
```
## Common Helm Commands

| **Category**      | **Command**                                              | **Description**                                                                                           |
|-------------------|----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| **Installation**  | `helm install <release-name> <chart>`                    | Installs a Helm chart with the specified release name.                                                    |
|                   | `helm upgrade <release-name> <chart>`                    | Upgrades an existing release to a new version of the chart.                                               |
|                   | `helm uninstall <release-name>`                          | Uninstalls a release from the Kubernetes cluster.                                                         |
|                   | `helm repo add <repo-name> <repo-url>`                   | Adds a new Helm chart repository.                                                                         |
|                   | `helm repo update`                                       | Updates the information of available charts locally from chart repositories.                              |
| **Linting**       | `helm lint <chart>`                                      | Runs a series of checks to ensure that a chart follows best practices.                                     |
| **Template**      | `helm template <release-name> <chart>`                   | Renders the templates locally and displays the generated YAML.                                             |
| **Troubleshooting**| `helm status <release-name>`                            | Displays the status of the specified release.                                                             |
|                   | `helm history <release-name>`                            | Shows the history of the specified release, including previous versions and their statuses.               |
|                   | `helm get all <release-name>`                            | Retrieves all information about the specified release, including values, hooks, and manifests.            |
|                   | `helm get values <release-name>`                         | Shows the values used to create the specified release.                                                    |
|                   | `helm list`                                              | Lists all releases in the current namespace.                                                              |
| **Debugging**     | `helm install <release-name> <chart> --debug --dry-run`  | Simulates an install and provides detailed debug information without actually installing.                 |
| **Repo Management**| `helm repo list`                                        | Lists all configured Helm chart repositories.                                                             |
|                   | `helm search repo <keyword>`                             | Searches for charts in the configured repositories that match the keyword.                                |
| **Release Management**| `helm rollback <release-name> <revision>`           | Rolls back a release to a specific revision.                                                              |
|                   | `helm diff upgrade <release-name> <chart>`               | Shows a diff explaining what a `helm upgrade` would change. Requires the `helm-diff` plugin.              |
| **Plugins**       | `helm plugin list`                                       | Lists installed Helm plugins.                                                                             |
| **Packaging**     | `helm package <chart>`                                   | Packages a Helm chart directory into a chart archive.                                                     |
| **Chart Management**| `helm show values <chart>`                            | Shows the default values for a chart.                                                                     |
|                   | `helm show chart <chart>`                                | Shows the details of the chart, such as name, version, and dependencies.                                  |


<p>
Why you need Helm?
When deploying an application on Kubernetes, it is required to define and manage several Kubernetes resources such as pods, services, deployments, and replica-sets. Each of these require to write a group of manifest files in YAML format. In the context of a complex application deployment it becomes a difficult task to maintain several manifest files for each of these resources. Helm simplifies this process and creates a single package that can be advertised to your cluster. <br/>

Helm Charts are simply Kubernetes YAML manifests combined into a single package that can be advertised to your Kubernetes clusters. Helm packages are called charts. Charts are easy to create, version, share, and publish.<br/>
Once packaged, installing a Helm Chart into your cluster is as easy as running a single helm install, which really simplifies deployment of containerized applications. <br/>

<b>Helm allows us to package Kubernetes releases into a convenient zip (.tgz) file.</b> A Helm chart can contain any number of Kubernetes objects, all of which are deployed as part of the chart. A Helm chart will usually contain at least a Deployment and a Service, but it can also contain an Ingress, Persistent Volume Claims, or any other Kubernetes object.<br/>

The chart is a bundle of information necessary to create an instance of a Kubernetes application.
<br/>
A release is a running instance of a chart, combined with a specific config.<br/>
</p>

#### How Does Helm Work?
Helm and Kubernetes work like a client/server application. The Helm client pushes resources to the Kubernetes cluster. The server-side depends on the version: Helm 2 uses Tiller while Helm 3 got rid of Tiller and entirely relies on the Kubernetes API.

Charts
Kubernetes package
Helm uses a packaging format called charts. A chart is a collection of files that describe a related set of Kubernetes resources. A single chart might be used to deploy something simple, like a pod, or something complex, like a full web app stack with HTTP servers, databases, caches, and so on.

<p>
<li>
chart.yaml: This is where you’ll put the information related to your chart. That includes the chart version, name, and description so you can find it if you publish it on an open repository. Also in this file you’ll be able to set external dependencies using the dependencies key.
</li>
<li>
values.yaml: Like we saw before, this is the file that contains defaults for variables.
</li>
<li>
<b>templates (dir): This is the place where you’ll put all your manifest files. Everything in here will be passed on and created in Kubernetes.</b>
</li>
</li>
<li>
charts: If your chart depends on another chart you own, or if you don’t want to rely on Helm’s default library (the default registry where Helm pull charts from), you can bring this same structure inside this directory. Chart dependencies are installed from the bottom to the top, which means if chart A depends on chart B, and B depends on C, the installation order will be C ->B ->A.
</li>
</p>

1. chart.yaml => Meta information about the chat such as name, version, etc.

2. values.yaml => Contains default value. You can override these default values while deploying.

3. charts => Contains all the chart dependencies.

4. templates => All the YAML files with a placeholder for values.

When to Use Helm Chart:
1. Assume you are deploying a complex application. For that, you are using multiple YAML files for ConfigMaps, Secrets, Services, and Pods. If you want to replicate the same setup on another Kubernetes cluster then you can use the Helm charts instead of managing multiple YAML files separately.

2. If you are using a standard(what all developers doing) way to install the application, then you can use Helm charts.
