# Monitoring & Alerts

[Prometheus](https://github.com/prometheus) is an open-source system monitoring and alerting toolkit originally built at [SoundCloud](https://soundcloud.com).

![](https://gblobscdn.gitbook.com/assets%2F-MERG_iQW5oN4ukgXP8K%2F-MGrw8HGOm3l_f8x_z_k%2F-MGrwL-4kKqpAxJeuFs6%2Fimage.png?alt=media\&token=5abd97b1-ed7e-4431-8b19-05990306b7c6)

​

​[prometheus-operator](https://github.com/egovernments/eGov-infraOps/tree/master/helm/charts/backbone-services/prometheus-operator) chart includes multiple components and is suitable for a variety of use-cases.

The default installation is intended to suit monitoring a kubernetes cluster the chart is deployed onto. It closely matches the kube-prometheus project.

* service monitors to scrape internal kubernetes components
  * kube-apiserver
  * kube-scheduler
  * kube-controller-manager
  * etcd
  * kube-dns/coredns
  * kube-proxy

With the installation, the chart also includes dashboards and alerts.

**Deployment steps**

1. Add environment variable to the respective env config file

![](https://gblobscdn.gitbook.com/assets%2F-MERG_iQW5oN4ukgXP8K%2F-MGrw8HGOm3l_f8x_z_k%2F-MGrwTLM-FUSNONevMC\_%2Fimage.png?alt=media\&token=74b8862b-6559-4eeb-9c62-d6e6b135f259)

Update the configs branch (like for qa.yaml added qa branch)

1. Add [monitoring-dashboards](https://github.com/egovernments/configs/tree/master/monitoring-dashboards) folder to respective configs branch.
2. Enable the nginx-ingress monitoring and redeploy the nginx-ingress.

![](https://gblobscdn.gitbook.com/assets%2F-MERG_iQW5oN4ukgXP8K%2F-MGrw8HGOm3l_f8x_z_k%2F-MGrwh6EjT8WevJDyZlZ%2Fimage.png?alt=media\&token=ae3f08ef-c717-4542-8f4b-94ae24a354c8)

1.  Add alertmanager secret in respective.secrets.yaml

    If you want you can change the slack channel and other details like group_wait , group_interval and repeat_interval according to your values.

![](https://gblobscdn.gitbook.com/assets%2F-MERG_iQW5oN4ukgXP8K%2F-MGrw8HGOm3l_f8x_z_k%2F-MGrwr0B56IGjJ6WoQ_P%2Fimage.png?alt=media\&token=757e1fbe-3ccb-4365-a5b9-2b29134946bc)

​

1. Deploy the prometheus-operator using go cmd or deploy using Jenkins.

```
go run main.go deploy -e   -c 'prometheus-operator,grafana,prometheues-kafka-exporter'
```

**To create a new panel in the existing dashboard**

1. Login to dashboard and click on add panel

![](https://gblobscdn.gitbook.com/assets%2F-MERG_iQW5oN4ukgXP8K%2F-MGrw8HGOm3l_f8x_z_k%2F-MGrxHZVT0kM_QOnu_qT%2Fimage.png?alt=media\&token=51014ac7-993c-4a98-8478-6de51133f090)

1. Set all required queries and apply the changes. Export the JSON file by clicking on t the save dashboard

![](https://gblobscdn.gitbook.com/assets%2F-MERG_iQW5oN4ukgXP8K%2F-MGrw8HGOm3l_f8x_z_k%2F-MGrxSGhocrCKvWfVyjF%2Fimage.png?alt=media\&token=9a694df7-f8be-4186-a928-2b39e25e2706)

1. Update the existing \*-dashboard.json file from configs monitoring-dashboards folder with a newly exported JSON file.
