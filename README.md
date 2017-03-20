# efk-stack
ElasticSearch Cluster (v5.2.2), Fluentd (v0.12) &amp; Kibana (v5.2.2) deployment files for Kubernetes

# Instructions
1. Build the docker images for Elastisearch & Fluentd using the configurations present in `docker-images` directory.
2. Replace the container image (Search for: `<docker-image>`) in files `elasticsearch.yml` and `fluentd.yml` with the images built in Step 1.
3. Run the following commands in the same order.

```

# Create the EFK Namespace
kubectl apply -f efk-namespace.yml

# Create EFK Storage Class - We have used AWS EBS provisioned volumes. Modify according to your needs.
kubectl apply -f efk-sc.yml

# Create Elasticseach Cluster - StatefulSet
kubectl apply -f elasticsearch.yml

# Note: Wait till all the pods of the Elasticsearch cluster are up and running.
# Create Fluentd - DaemonSet
kubectl apply -f fluentd.yml

# Create Kibana - Deployment. Default username for kibana is 'elastic' and default password is 'changeme'
kubectl apply -f kibana.yml

# Note: Kibana dashboard can be accessed by proxying Cluster IP of the kibana service or by using the load balancer url created by your cloud service provider.

```
