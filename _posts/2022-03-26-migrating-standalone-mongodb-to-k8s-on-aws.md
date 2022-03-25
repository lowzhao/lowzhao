---
published: false
---

## Migrating Standalone MongoDb Database to Kubernetes on AWS

While configuring the connection between the applications in K8s Pods, I found out that there would be lots of custom work if I want to use an external database for the pods. Therefore I seek for a better solution.

I know that there is few [Helm Charts (by Bitnami)](https://artifacthub.io/packages/helm/bitnami/mongodb) that will help me setup the MongoDb database on frontend cluster, but I am looking for a solution to migrate the data from one server to the cluster. 

Morever the Helm Chart have **31 templates!** That is too much for any workload, therefore I wish to check the rendered result before moving towards deploying the charts onto the cluster.

The Bitnami template contains a sets of customizable values one of which caught my eye:

```persistence.existingClaim:  Provide an existing PersistentVolumeClaim (only when architecture=standalone)```

I know that I will need to commit my current database data into the [`PersistentVolumeClaim`](https://kubernetes.io/docs/reference/kubernetes-api/config-and-storage-resources/persistent-volume-claim-v1/) and setup the database with credential. So when the MongoDb pods are running, existing data can be in the database. 

# Multinode solution
The database pod can be running in any of the nodes, the [PresistentVolume](https://kubernetes.io/docs/reference/kubernetes-api/config-and-storage-resources/persistent-volume-v1/) feature(or I should say "requirement") that allow node have access to the presistent volume can be helpful.

My solution is to use the [AWS EBS Multi-Attach](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volumes-multi.html) to attempt to attach the current database volume to all the nodes on the cluster.
