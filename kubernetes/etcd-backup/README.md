# ETCD backup cronjob for Kubernetes Clusters

- An existing image can be pulled down from `jvkhan/etcd-backup:latest`

- This cron pod is meant to be run on master nodes, hence it uses the corresponding toleration and nodeSelector.

- It assumes a secret with the aws crendetials has already been created.

- The same job can be deployed to multiple clusters with a distinct value for the *CLUSTER* env variable. This will become the folder name that all backups get uploaded to.

- Pods can be listed with:

```
# List the cronjob
kubectl get cronjobs -n kube-system

# Create a job manually
kubectl create job --from=cronjob/etcd-backup etcd-backup-test -n kube-system

# List the pods & look at logs
kubectl get pods -n kube-system | grep etcd
```

- A successful run will look like this:

```
$ kubectl logs etcd-backup-1580774400-zw5t6 -n kube-system

aws-cli/1.14.10 Python/2.7.15 Linux/4.4.158-1.el7.elrepo.x86_64 botocore/1.8.14
member b891238f8ee7bd16 is healthy: got healthy result from https://10.175.101.214:2379
member c750876c9e9077d5 is healthy: got healthy result from https://10.175.101.209:2379
member cd8d6ba6711bc205 is healthy: got healthy result from https://10.175.101.210:2379
cluster is healthy
Saving backup snapshot 2020-02-04 00:00:05
Snapshot saved at snapshotdb_cluster01_2020-02-04
upload: ./snapshotdb_cluster01_2020-02-04 to s3://k8s-etcd-backups/cluster01/snapshotdb_cluster01_2020-02-04

```
