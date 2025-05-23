# harper-code-challenge

Platform Operations Engineer - Coding Challenge

## First Steps

Clone down the repository to your local machine:

```shell
git clone git@github.com:aaronpederson/harper-code-challenge.git
cd harper-code-challenge
```

Deploy the application:

```shell
kubectl apply -k deploy/harperdb
```

## Monitoring Installation:
```shell
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install prometheus prometheus-community/kube-prometheus-stack --namespace monitoring --create-namespace
```

This process may take some time, but will eventually return the following:

```shell
kube-prometheus-stack has been installed. Check its status by running:
  kubectl --namespace monitoring get pods -l "release=prometheus"

Get Grafana 'admin' user password by running:

  kubectl --namespace monitoring get secrets prometheus-grafana -o jsonpath="{.data.admin-password}" | base64 -d ; echo

Access Grafana local instance:

  export POD_NAME=$(kubectl --namespace monitoring get pod -l "app.kubernetes.io/name=grafana,app.kubernetes.io/instance=prometheus" -oname)
  kubectl --namespace monitoring port-forward $POD_NAME 3000

Visit https://github.com/prometheus-operator/kube-prometheus for instructions on how to create & configure Alertmanager and Prometheus instances using the Operator.
```
Once operational; navigate in a browser to `127.0.0.1:3000` where you will be prompted to input a user|password found in the previous output. This will bring you to a Grafana homepage where things such as metrics can be found and monitored prior to any DB operations listed below.

## DB Operations
Once the cluster in a working state you have several DB operation jobs that can be initiated based on user need.  

```shell
kubectl apply -f ./harperdb/resources/Job/{{ job_name }}.yaml
```
In the event of a failed job you will need to delete it before running it again"

```shell 
kubectl delete -f ./harperdb/resources/Job/{{ job_name }}.yaml
```
