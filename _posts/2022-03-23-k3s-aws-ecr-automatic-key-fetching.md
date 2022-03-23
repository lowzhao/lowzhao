---
published: true
layout: post
author: eugene
categories: Kubernetes
tags:
  - featured
  - k3s
  - helm
  - helm_controller
  - kubernetes
  - aws
  - aws_ecr
---
## K3s AWS ECR Key refershing

### Background
The AWS ECR(Elastic Container Registry) allow users to host container repository(private and public repository), but the step to install the containers are quite confusing and complicated.

Therfore I decided to search for the simpler solution. Here comes the helm chart that automatically refreshes the key for docker installation. 

Helm Chart: [https://artifacthub.io/packages/helm/aws-multi-ecr-credentials/aws-multi-ecr-credentials](https://artifacthub.io/packages/helm/aws-multi-ecr-credentials/aws-multi-ecr-credentials)

My current cluster installed with [K3s](https://k3s.io/) which is the smallest memory footprint cluster that I have tested on AWS EC2. The way to join new worker nodes into the cluster is exteremly simple, therefore I go with K3s.

### Execution

With K3s comes the benefit of Helm Controller. The Rancher's [Helm Controller](https://github.com/k3s-io/helm-controller) made everything easier by allowing us to install helm chart as a config similar to Kubernetes components(kudos to Kubernetes CRD). 

You may modify the `NAMESPACE` to your desired namespace.

Example `helm_chart.yaml` file.
```yaml
apiVersion: helm.cattle.io/v1
kind: HelmChart
metadata:
  name: aws-multi-ecr-credentials-NAMESPACE
  namespace: kube-system
spec:
  chart: https://raw.githubusercontent.com/laviua/aws-multi-ecr-credentials/master/docs/aws-multi-ecr-credentials-1.4.3.tgz
  targetNamespace: NAMESPACE
  valuesContent: |-
    aws:
      account: AWS_ACCOUNT_ID
      region: "ap-east-1"
      accessKeyId: AWS_ACCESS_KEY_ID
      secretAccessKey: AWS_SECRET_ACCESS_KEY
    targetNamespace: NAMESPACE
    cron: "0 */4 * * *"
```

By `kubectl apply -f helm_chart.yaml` k3s will automatically install the Helm chart similar to how you would do `helm install` and `helm upgrade`.

### Extra

After the setup complete there would have new pods in the namespace `aws-multi-ecr-credentials-NAMESPACE-ns` responsible to dealing with the credentials and create kubernets secret(name `aws-registry-AWS_ACCOUNT_ID`) in the `targetNamespace` you want.

K3s will creates new pods with the prefix `helm-install` in the namespace `kube-system` you may check the logs of the pods to debug the installation.

If you want to delete this helm release(terminology of Helm) you can do `kubectl delete -f helm_chart.yaml`


Also for more information on how to use Helm Controller read this:
https://rancher.com/docs/k3s/latest/en/helm/
