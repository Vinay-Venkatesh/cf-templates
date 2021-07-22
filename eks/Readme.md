## eks-cluster.yaml 
This file contains resources to setup eks cluster.

## eks-nodegroup.yaml
This file contains resources to setup eks node group and attach it to the cluster.

## Post Cluster Setup
1. Connect to the cluster via cli - ```aws eks --region **region** update-kubeconfig --name **cluster_name**```
2. Create a yaml file with the below contents and replace the rolearn to match the instance role.

aws-auth.yaml:
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: aws-auth
  namespace: kube-system
data:
  mapRoles: |
    - rolearn: <ARN of instance role (not instance profile)>
      username: system:node:{{EC2PrivateDNSName}}
      groups:
        - system:bootstrappers
        - system:nodes
```
        
3. Run ```kubectl apply -f aws-auth.yaml``` and update the cluster config.
4. Run ```kubectl get nodes``` and verify the nodes are available and is in healthy state.
