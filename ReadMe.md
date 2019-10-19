# Prometheus, Pushgateway, Alertmanager and other tools

Installing Prometheus, Pushgateway, AlertManager etc. via Helm:  
`helm upgrade --install prometheus stable/prometheus --namespace monitoring -f prometheus/values.yaml`

In the prometheus folder you can find the values.yaml, which you can edit for adding new prometheus scrape targets or editing the ingresses and so on.

Take a look at the deployments:  
`kubectl -n monitoring get all`  


# Grafana

Installing Grafana via Helm:   
`helm upgrade --install grafana stable/grafana --namespace monitoring -f grafana/values.yaml`

In the grafana folder you can find the values.yaml, which you can edit for adding new grafana datasources, editing the ingresses and so on.

Take a look at the deployments:  
`kubectl -n monitoring get all`  


# Kube Cost

Official Documentation: https://github.com/kubecost/cost-model

## Setting up as own deployment

We deploy Kube Cost in a different namespace. The steps below are from the documentation: https://github.com/kubecost/cost-model/blob/master/kubernetes/deployment.yaml#L30
 

1. Setting environment variable: https://github.com/kubecost/cost-model/blob/master/kubernetes/deployment.yaml#L30
2. `kubectl create namespace cost-model`
3. `kubectl apply -f kubernetes/ --namespace cost-model`


# Metrics from Kube Cost and Dashboards in Grafana

There are some dashboards in this repository for the virtualization of the Kubernetes Cluster and its resources. They can be found within the grafana/dashboards folder.  

We are using 5 dashboards at the moment:
- Analysis by Cluster
- Analysis by Namespace
- Analysis by Pod
- Node Utilization metrics
- Kubecost cluster metrics 

The first 3 dashboards in the list have variables defined such as CPU, RAM or Storage costs. They can be changed within the dashboards. These data (which are coming from the usage of K8s in Google Cloud) have to be evaluated for the K8s in Azure. The Node Utilization metrics dashboard just gives an overview about the current nodes and how the workload looks like. 

The Kubecost cluster metrics dashboard can be compared to the Analysis by Cluster dashboard. The difference is that there are no predefined variables. All the data is coming from the kube cost deployment in the cube-cost namespace. The cube-cost deployment and its dashboard have also to be evaluated in the future.

If one of those dashboards (Kubecost  or Analysis by Cluster) is better, the other dashboard (or in case of kubecost the whole deployment) can be removed. 
Kubecost provides some queries and metrics which can be used to create custom dashboards or to extend the provided one: https://github.com/kubecost/cost-model/blob/master/PROMETHEUS.md