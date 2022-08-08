---
title: "Prep Lab 2.1: Provision a Kubernetes Cluster Using The AWS EKS Quick Start"
weight: 21
chapter: true
draft: false
description: "Provision a value-added EKS spot cluster by using the official AWS Quick Start."
---

# Prep Lab 2.1: Provision a Kubernetes Cluster Using The AWS EKS Quick Start

> **Keyboard Time**: 5 mins, **Automation Wait Time**: 60 mins
>
> **Scenarios:** Instructor-Led, Self-Paced

{{< admonition type=success title="What is an AWS Quick Start?" open=true >}}

“Quick Starts are automated reference deployments built by Amazon Web Services (AWS) solutions architects and AWS Partners. Quick Starts help you deploy popular technologies on AWS according to AWS best practices. You can reduce hundreds of manual procedures to just a few steps so that you can build and start using your environment within minutes.”

Quote from: [AWS Quick Starts](https://aws.amazon.com/quickstart/?solutions-all.sort-by=item.additionalFields.sortDate&solutions-all.sort-order=desc&awsf.filter-content-type=*all&awsf.filter-tech-category=*all&awsf.filter-industry=*all)

Since this Quick Start is for AWS technology and functions as a dependency of many other partner Quick Starts - it reflects AWS internal best practices for EKS deployment and has value over console or eksctl deployment methods. For these labs we take advantage of the single parameter inclusion of the Kubernetes Cluster Autoscaler and spot instances and especially the very valuable EKS Bastion for cluster administration.
{{< /admonition >}}

{{< admonition type=danger title="Classroom Setup and Labs Assume EKS Quick Start" open=true >}}

The exercises, as written, assume that the cluster was prepared with the EKS Quick Start. If you decide to use other methods, you will need to also provision a cluster administration workstation with helm, kubectl, kube configuration and AWS authentication to perform EKS administration commands as needed. Additionally, you will need to plan for Kubernetes node scaling that matches your class size. If you prepare the cluster by another means, be sure to run through all exercises with your cluster before considering it ready for a classroom. Scale testing with a group should also be performed if using alternative cluster node autoscaling.
{{< /admonition >}}

{{< admonition type=abstract title="Target Outcomes" open=true >}}

1. Create an EKS Cluster that is very cost optimized for training scenarios.

2. Do so very quickly by leveraging AWS official Managed Infrastructure as Code known as “AWS Quick Starts”.

3. **Not a Target**: Learning the detailed ins and outs of deploying and configuring EKS.
   {{< /admonition >}}

## Deploy Official AWS EKS QuickStart with Spot Nodes

> **Keyboard Time**: 5 mins, **Automation Wait Time**: 60 mins
>
> **Scenarios:** Instructor-Led, Self-Paced
>
> Guides Through: [AWS EKS on the AWS Cloud](https://aws-quickstart.github.io/quickstart-amazon-eks/)

{{< admonition type=warning title="IMPORTANT" open=true >}}

In order to take advantage of spot support and specifying the Kubernetes version (required by GitLab integration), we must first deploy a small ‘Advanced Configuration’ template from the EKS Quick Start that is then read by the main EKS Quick Start template when deploying.

{{< /admonition >}}

1. Login to your target AWS account.

2. In the EC2 Console for us-east-2, on the left navigation under 'Network & Security', *Click* **Key Pairs**

   {{< admonition type=warning title="Warning" open=true >}}
   While this exercise only ever uses SSM to access the Bastion host, the CloudFormation form always requires a Key Pair. You will not end up using this key pair, but it must be available for the stack to succeed.
   {{< /admonition >}}

3. In the upper right of the page, *Click* **Create key pair**

4. In 'Name', *Type* **spot2azuseast2**

5. The following links will set the parameters in the table for you. You may customize the parameters before submitting the template. **IMPORTANT** Cluster add-on settings for Hashicorp vault and others are not used unless these items are installed during the next template deployment - they can be ignored.

   - To Deploy to us-east-2 (in case us-east-1 is near account quota limits): [Click this link to deploy the Advanced Configuration Template with the below parameters preconfigured](https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/quickcreate?templateUrl=https://s3.amazonaws.com/aws-quickstart/quickstart-amazon-eks/templates/amazon-eks-advanced-configuration.template.yaml&stackName=spot-t2-medium-v120-paramset&param_ConfigSetName=spot-t2-medium-v120-paramset&param_KubernetesVersion=1.21&param_NodeInstanceType2=t3.medium&param_NodeInstanceType3=t3.large&param_OnDemandPercentage=0)
   - To Deploy to us-east-1 (required for AWS “Event Engine” provisioned accounts): [Click this link to deploy the Advanced Configuration Template with the below parameters preconfigured](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https://s3.amazonaws.com/aws-quickstart/quickstart-amazon-eks/templates/amazon-eks-advanced-configuration.template.yaml&stackName=spot-t2-medium-v120-paramset&param_ConfigSetName=spot-t2-medium-v120-paramset&param_KubernetesVersion=1.21&param_NodeInstanceType2=t3.medium&param_NodeInstanceType3=t3.large&param_OnDemandPercentage=0)  

   | <span style="white-space:nowrap;">CF&emsp;GUI&emsp;Name&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;</span> | CF Parameter Name  | <span style="white-space:nowrap;">Value&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;</span> | Notes                                                        |
   | ------------------------------------------------------------ | ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
   | Stack name                                                   | N/A                | spot-t2-medium-v120-paramset                                 |                                                              |
   | Config set name                                              | ConfigSetName      | spot-t2-medium-v120-paramset                                 |                                                              |
   | Kubernetes version                                           | KubernetesVersion  | 1.21                                                         | GitLab integrated K8s clusters must use specific versions    |
   | Instance type 2                                              | NodeInstanceType2  | t3.medium                                                    | Instance type cannot match what is used for NodeInstanceType<br /> in the EKS Quick Start deployment (next template below) as that value<br /> is used for the first spot type when spot is configured and all NodeInstanceTypes in a spot configuration must be unique from each other. |
   | Instance type 2                                              | NodeInstanceType3  | t3.large                                                     | Instance type cannot match what is used for NodeInstanceType in the EKS Quick Start deployment (next template below) as that value is used for the first spot type when spot is configured and all NodeInstanceTypes in a spot configuration must be unique from each other. |
   | On-demand percentage                                         | OnDemandPercentage | 0                                                            |                                                              |

   **Important**: EKS Advanced Configuration 'Config sets' can be used to configure multiple deployments of the EKS Quick Start.

6. Verify the above values - **including any name substitutions you have elected to make.** 

7. At 'the bottom of the page', *Click* **Create stack**.

8. Wait for the deployment to complete successfully.

   {{< admonition type=warning title="Warning" open=true >}}

   **IMPORTANT FOR Instructor-Led** - setup 1 EKS node per 5 students. This can be easily adjusted later and these are spot instances.

   {{< /admonition >}}

9. The following links will set the parameters in the table for you. You may customize the parameters before submitting the template.

   - To Deploy to us-east-2 (in case us-east-1 is near account quota limits): [Click this link to deploy the Advanced Configuration Template with the below parameters preconfigured](https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/quickcreate?templateUrl=https://s3.amazonaws.com/aws-quickstart/quickstart-amazon-eks/templates/amazon-eks-entrypoint-new-vpc.template.yaml&stackName=spot2azuseast2&param_AvailabilityZones=us-east-2b,us-east-2c&param_RemoteAccessCIDR=198.51.100.0/24&param_KeyPairName=spot2azuseast2&param_ConfigSetName=spot-t2-medium-v120-paramset&param_NodeInstanceType=t2.medium&param_EKSClusterName=spot2azuseast2&param_NumberOfAZs=2&param_NumberOfNodes=2&param_MaxNumberOfNodes=3&param_NodeGroupType=Unmanaged&param_NodeInstanceFamily=Standard&param_ClusterAutoScaler=Enabled)
   - To Deploy to us-east-1 (required for AWS “Event Engine” provisioned accounts): [Click this link to deploy the Advanced Configuration Template with the below parameters preconfigured](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https://s3.amazonaws.com/aws-quickstart/quickstart-amazon-eks/templates/amazon-eks-entrypoint-new-vpc.template.yaml&stackName=spot2azuseast1&param_AvailabilityZones=us-east-1b,us-east-1c&param_RemoteAccessCIDR=198.51.100.0/24&param_KeyPairName=spot2azuseast2&param_ConfigSetName=spot-t2-medium-v120-paramset&param_NodeInstanceType=t2.medium&param_EKSClusterName=spot2azuseast1&param_NumberOfAZs=2&param_NumberOfNodes=2&param_MaxNumberOfNodes=3&param_NodeGroupType=Unmanaged&param_NodeInstanceFamily=Standard&param_ClusterAutoScaler=Enabled)

   | CF GUI Name                  | CF Parameter Name  | Value                           |                                                              |
   | ---------------------------- | ------------------ | ------------------------------- | ------------------------------------------------------------ |
   | Stack name                   | N/A                | spot2azuseast2                  |                                                              |
   | Availability Zones           | AvailabilityZones  | us-east-2b,us-east-2c (example) |                                                              |
   | Allowed external access CIDR | RemoteAccessCIDR   | 198.51.100.0/24                 | TEST-NET-2 IP address - only SSM is used for bastion         |
   | SSH key name                 | KeyPairName        | spot2azuseast2                  |                                                              |
   | Config set name              | ConfigSetName      | spot-t2-medium-v120-paramset    | Must match Config set name in above 'Advanced Configuration Template' |
   | Instance type                | NodeInstanceType   | t2.medium                       |                                                              |
   | Number of Availability Zones | NumberOfAZs        | 2                               |                                                              |
   | EKS cluster name             | EKSClusterName     | spot2azuseast2                  |                                                              |
   | Number of nodes              | NumberOfNodes      | 2                               |                                                              |
   | Maximum number of nodes      | MaxNumberOfNodes   | 3                               | **IMPORTANT Instructor-Led:** Adjust for class size, about 1 node per 5 students. |
   | Node group type              | NodeGroupType      | Unmanaged                       |                                                              |
   | Node instance family         | NodeInstanceFamily | Standard                        | Auto DevOps will not work on ARM clusters                    |
   | Cluster Autoscaler           | ClusterAutoScaler  | Enabled                         |                                                              |

10. Verify the above values - **including any name substitutions you have elected to make.** 

11. At 'the bottom of the page' *Check* **I acknowledge that AWS CloudFormation might create IAM resources with custom names.**

12. *Check* **I acknowledge that AWS CloudFormation might require the following capability: CAPABILITY_AUTO_EXPAND**

13. *Click* **Create stack**.

{{< admonition type=info title="Prep Lab 2.2 Can Be Done in Parallel" open=true >}}
You can complete Prep Lab 2.2 while this CloudFormation is processing, but this CF must complete successfully before doing Prep Lab 2.3.
{{< /admonition >}}

{{< admonition type=abstract title="Accomplished Outcomes" open=true >}}

1. Create an EKS Cluster that is very cost optimized for training scenarios.
2. Do so very quickly by leveraging AWS official Managed Infrastructure as Code known as “AWS Quick Starts”.
   {{< /admonition >}}
