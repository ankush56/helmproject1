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

# Upgrade after templating
```
helm upgrade mywebapp-release webapp1/ --values mywebapp/values.yaml
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
templates (dir): This is the place where you’ll put all your manifest files. Everything in here will be passed on and created in Kubernetes.
</li>
<li>
charts: If your chart depends on another chart you own, or if you don’t want to rely on Helm’s default library (the default registry where Helm pull charts from), you can bring this same structure inside this directory. Chart dependencies are installed from the bottom to the top, which means if chart A depends on chart B, and B depends on C, the installation order will be C ->B ->A.
</li>
</p>