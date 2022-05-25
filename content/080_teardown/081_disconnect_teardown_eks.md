---
title: "Disconnect and Teardown EKS"
weight: 81
chapter: true
draft: false
description: "Teardown the EKS Cluster and GitLab Projects."
---

# Disconnect and Teardown EKS

> **Scenarios:** Instructor-Led, Self-Paced

## Uninstall Ingress

> This will remove the AWS Load Balancer.

1. In 'classgroup/cluster-management' *Start* the **Web IDE**.
2. *Edit* the file **applications/ingress/helmfile.yaml**
3. Update the text <mark>installed: true</mark> to <mark>installed: false</mark>
4. *Edit* the file **applications/cert-manager/helmfile.yaml**
5. Update the text <mark>installed: true</mark> to <mark>installed: false</mark>
6. *Click* **Commit...**
7. *Select* **Commit to master branch**
8. *Click* **Commit**
9. Wait for the GitLab deployment to complete successfully.
10. To validate removal of the AWS load balancer created by the ingress chart, in the 'AWS EC2 Console', in the left hand navigation, under 'Load balancing', *Click* **Load Balancers** 
11. **None** of the Load Balancers should have their 'Tag' details matching **Key = kubernetes.io/cluster/spot2azuseast2** with **Value = owned**

## Teardown EKS Quick Start Stack

> This will remove the EKS cluster and all that it contains.

1. Open the AWS CloudFormation console and on the upper right, in the region selector *Select* **us-east-2**
2. *Set* 'View nested' (toggle) to **off**
3. *Select* <mark>the stack that created the EKS cluster</mark> (default name for these docs = **spot2azuseast2**)
4. In the upper right, *Click* **Delete** (button)

Optionally Remove the Advanced Configuration Stack

> The advanced configuration stack only hold parameters and does not consume any resources, if you might use it again to redeploy a stack with the same configuration, it is more efficient to not tear it down.

1. Open the AWS CloudFormation console and on the upper right, in the region selector *Select* **us-east-2**
2. *Set* 'View nested' (toggle) to **off**
3. *Select* <mark>the stack that created the Advanced Configuration</mark> (default name for these docs = **spot-t2-medium-v120-paramset**)
4. In the upper right, *Click* **Delete** (button)

## GitLab CleanUp

1. If you do not have other clusters associated with your cluster-management project and will not reuse it:
2. In 'classgroup/cluster-management' Click Infrastructure => Kubernetes clusters
3. Use the trash can icons to delete all agent registrations.
4. To delete the project in 'classgroup/cluster-management' *Click* **Settings => General => Advanced => Delete project**
5. Type <mark>the indicated full project path</mark> and *Click* **Yes, delete project**
6. To delete the project in 'classgroup/Auto DevOps-security-scanning' *Click* **Settings => General => Advanced => Delete project**
7. Type <mark>the indicated full project path</mark> and *Click* **Yes, delete project**
