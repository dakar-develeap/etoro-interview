# Etoro home exercise description
## Nginx ingress controller
I've installed nginx ingress controller using helm from - https://docs.microsoft.com/en-us/azure/aks/ingress-basic

## Application helm chart
1. I've initialized a helm chart using helm create command and configured the relevant yaml files.
2. I didn't change anything from the template files except for the ingress resource, the Chart.yaml and the values.yaml
3. Chart.yaml - since we don't manage the application versions I've changes the 'appVersion' attribue to 'latest'
4. ingress.yaml - I've changed the rule to be without a host(since we don't have a DNS record), to include all '/*' routes.
5. values.yaml - here i've updated the application docker image, the service to listen http, the requests/limits with small values since this is a simple app, the autoscaling(of the hpa) to be enabled and to be activated when pods reach 80% CPU utilizations.

## Jenkins
I've created my own custom jenkins docker image using the latest image of jenkins. Within the dockerfile I've installed relevant tools for deployment to AKS. I've installed docker, azurecli, kubectl, helm
Also, I've added a docker-compose file to launch jenkins custom image with the relevant ports to open, and volumes for the docker socket and for the jenkins_home dir.

## Jenkins pipeline
Within jenkinsfile i've added github 'push' trigger to allow jenkins receiving the payload from github hook and running the relevant pipelines. I've used 2 stages since it's a simple pipeline:
1) Checkout - to git clone the sources
2) Deploy using azurecli to login registry and to configure kubeconfig in order to connect the AKS cluser, and helm to upgrade the chart. I've used global environment variables for the AKS details which you gave me.
** Each stage is conditioned with a changeset on the helm chart directory, so the pipeline will run only when a change occurs within chart only.
