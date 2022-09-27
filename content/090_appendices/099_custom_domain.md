---
title: "Reconfigure for Custom Domain"
weight: 99
chapter: true
draft: false
description: "Use a custom domain with Auto DevOps and your cluster."
---

## Configuring DNS, SSL, Ingress and Auto DevOps for a Custom Domain

> **Scenarios:** Instructor-Led, Self-Paced

This section makes the following assumptions:

- The domain being used is managed in AWS Route 53
- That for a test setup it is OK to have the ELB to EKS Ingress traffic unencrypted.
- That you will substitute your custom domain for the sample one (**eks.devops4the.win**) used in the exercise.

### Additional Cost Estimates

For us-east-2

| Item                                | Estimated Cost | Reduce or Eliminate                                  |
| ----------------------------------- | -------------- | ---------------------------------------------------- |
| AWS Certificate Services (ACM) Cert | Free           |                                                      |
| DNS Zone Record for Custom Domain   | $0.50/Mo       | Cannot be disabled, only destroying eliminates cost |

### AWS Route 53 Setup

1. If the zone does not exist, in the AWS Route 53 console, *Click* **Create hosted zone**

2. In 'Domain name' *Type* **eks.devops4the.win** (<mark class="hlgreen">use your domain instead - it does not need to be a subdomain like this one</mark>)

3. *Select* **Public hosted zone**

4. *Click* **Create hosted zone**

5. To create a wild card record for the domain, *Click* **Create record**

   1. For 'Record name' *Type* ***** (star character)
   2. On the right of the page, to the right of the word 'Value' *Click* **Alias** (toggle button)
   3. For the newly appeared 'Route traffic to' field, *Click* the **small down arrow** on the right
   4. In the 'Drop down list' *Select* **Alias to Application and Classic Load Balancer**
   5. In 'Chose Region' *Click* the **small down arrow** on the right
   6. In the 'Drop down list' *Select* **us-east-2**
   7. *Click* **Choose load balancer**
   8. *Select* <mark>The Load Balancer Associated With Your EKS Cluster Ingress</mark>
      **IMPORTANT**: If there is more than one load balancer, you can use the EC2 Load Balancer console and check the "Tags" tab of each one until you find one with a key named **kubernetes.io/cluster/spot2azuseast2**
   9. *Click* **Create records**

6. KUBE_INGRESS_BASE_DOMAIN is used for Auto DevOps - therefore we are configuring it at the top group level for which we would like the cluster in question to be usable for Auto DevOps for all downbound groups and projects.

7. In the GitLab group 'classgroup' *Click* **Settings > CI/CD**

8. Next to 'Variables' *Click* **Expand**

9. *Click* **Add variable** once for each table row (or update the variables if they are already there)

   | Key                      | Value              | Protect | Mask |
   | ------------------------ | ------------------ | ------- | ---- |
   | KUBE_INGRESS_BASE_DOMAIN | eks.devops4the.win | No      | No   |

### Certificate Setup

1. In the AWS 'Certificate Manager' console *Click* **Request** (button)
2. On the 'Request certificate' page, *Click* **Request a public certificate** and *Click* **Next**
3. Under 'Fully qualified domain name' *Type* ***.eks.devops4the.win** (the `*` must be included)
4. Under 'Select validation method' *Select* **DNS validation - recommended**
5. *Click* **Request** (button)
6. For the row that contains *.eks.devops4the.win, Click <mark>the certificate id</mark> (e.g. "667ecb1f-c8ff-4b70-b6fd-f0ab9027a7da")
7. Under 'Domains' *Click* **Create records in Route 53**
8. On the page 'Create DNS records in Amazon Route 53' *Click* **Create records**
9. Return to the 'Certificates list' (hint: there is a navigation breadcrumb trail at the top of the page)
10. *Click* the <mark>refresh cycle icon</mark> (button) until your new certificate status changes from 'Pending validation' to 'Issued'
11. Once the Certificate is in the 'Issued' state, click the Certificate ID to see the details
12. On the 'Certificate status' page, under 'ARN' <mark>Copy the ARN</mark> or leave this page open for copying and pasting in the next section.

### Update Cluster Management Project to Install the NGINX Ingress with ACM Cert and Remove Cert Manager If Installed

update for removal

1. In 'classgroup/cluster-management' *Start* the **Web IDE**.

2. Edit the file 'helmfile.yaml' and uncomment the line <mark class="hlgreen">- path: applications/ingress/helmfile.yaml</mark>

3. Edit the file 'applications/ingress/values.yaml' and add the following at the very top: `global.ingress.tls.enabled: false`

4. At the bottom add the following

   ```
     config:
       # pass the X-Forwarded-* headers directly from the upstream
       use-forwarded-headers: "true"
     service:
       annotations:
         service.beta.kubernetes.io/aws-load-balancer-ssl-ports: 'https'
         service.beta.kubernetes.io/aws-load-balancer-backend-protocol: 'http'
         service.beta.kubernetes.io/aws-load-balancer-ssl-cert: "CERTIFICATE_ARN_GOES_HERE"
       targetPorts:
         https: http
   ```

5. In the above, paste over <mark class="hlpink">CERTIFICATE_ARN_GOES_HERE</mark> with the actual certificate ARN from the previous section.

6. *Click* **Commit...**

7. *Select* **Commit to master branch**

8. *Click* **Commit**

### Teardown and Cleanup

1. In the AWS 'Certificate Manager' console *Locate the previously created certificate*
2. *Click* <mark>the checkbox</mark> at the start of the identified row and then in the upper right, *Click* **Delete** (button).
3. In the 'Delete selected certificates' popup, *Type* **delete** and *Click* **Delete**.
4. In the AWS Route 53 console, *Click* **Hosted zones**.
5. **Click** <mark>the domain name used</mark> (eks.devops4the.win in these instructions) to edit the zone records.
6. **Locate** <mark>the CNAME record</mark> used for certificate validation (like the only CNAME record) and *Click* <mark>the checkbox</mark> at the start of the identified row.
7. In the upper right, *Click* **Delete Record** (button).
8. In the 'Delete selected record?' popup, *Click* **Delete**.
9. Repeat the record deletion procedure for the DNS wildcard record (*.eks.devops4the.win in these instructions)
10. *Click* **Hosted zones**.
11. **Locate**  <mark>the domain name used</mark> (eks.devops4the.win in these instructions) and *Click* `the Checkbox` at the start of the identified row.
12. In the upper right, *Click* **Delete** (button).
13. In the 'Delete hosted zone' popup, *Type* **delete** and *Click* **Delete**.