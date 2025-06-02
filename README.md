# Platform Operations Engineer - Coding Challenge

This guide assumes you have a working Kubernetes cluster. If you don't have Kubernetes already, Rancher Desktop is easy to set up, and what was used for this demo. Instructions for that can be found at [rancher](https://rancherdesktop.io/) otherwise another alternative I would recommend is [minikube](https://minikube.sigs.k8s.io/docs/start/?arch=/macos/arm64/stable/binary+download).

## First Steps:

Clone down the repository to your local machine:

```shell
git clone git@github.com:aaronpederson/harper-code-challenge.git
cd harper-code-challenge
```

Deploy the application:

```shell
kubectl apply -k harperdb
```

## Monitoring Installation:
```shell
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm upgrade --install --create-namespace --namespace monitoring prometheus prometheus-community/kube-prometheus-stack
```

1. kube-prometheus-stack has been installed. Check its status by running:
```shell
kubectl --namespace monitoring get pods -l "release=prometheus"
```

2. Get Grafana 'admin' user password by running:
```shell
kubectl --namespace monitoring get secrets prometheus-grafana -o jsonpath="{.data.admin-password}" | base64 -d ; echo
```

3. Access Grafana local instance:
```shell
  export POD_NAME=$(kubectl --namespace monitoring get pod -l "app.kubernetes.io/name=grafana,app.kubernetes.io/instance=prometheus" -oname)
  kubectl --namespace monitoring port-forward $POD_NAME 3000
```

Once operational, navigate to [`http://127.0.0.1:3000`](http://127.0.0.1:3000) in your browser and log in to Grafana with the credentials from the output above. In the "Dashboards" section on the sidebar, you can find the available dashboards for viewing metrics.

## DB Operations:
Once the cluster in a working state you have several DB operation jobs that can be initiated based on user need.

```shell
kubectl apply -f ./harperdb/resources/Job/{{ job_name }}.yaml
```
In the event of a failed job you will need to delete it before running it again"

```shell
kubectl delete -f ./harperdb/resources/Job/{{ job_name }}.yaml
```
