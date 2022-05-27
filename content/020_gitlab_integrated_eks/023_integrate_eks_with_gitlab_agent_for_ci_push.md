---
title: "Prep Lab 2.3: Using GitLab K8s Agent to Integrate The Cluster with GitLab"
chapter: true
weight: 23
description: "Setup the most secure and capable method of integrating Kubernetes with GitLab."
---

# Prep Lab 2.3: Use GitLab K8s Agent to Integrate The Cluster with GitLab

{{% notice info %}}
When this section is complete you will have integrated the EKS cluster with GitLab using the GitLab Kubernetes Agent.

While this section isn’t strictly needed for GitOps scenarios, it is how we deploy the ingress as a managed app. Pure GitOps scenarios in production would not require a runner nor this agent configuration to deploy the ingress.{{% /notice %}}

## Configure and Install GitLab Kubernetes Agent for CI Push

> **Keyboard Time**: 20 mins, **Automation Wait Time**: 20 mins
>
> **Scenarios:** Instructor-Led, Self-Paced
>
> Guides Through: [Installing the agent for Kubernetes](https://docs.gitlab.com/ee/user/clusters/agent/install) 

{{% notice tip %}}
This specific agent configuration combined with the values and placement of the CI/CD variables starting with “KUBE_” accomplish CI Push integration for the entire heirarchy of projects under 'classgroup'. This emulates the way that the legacy certificate connection method was able to integrate and entire heirarchy of projects - so this specific configuration may be useful in your actual development processes.
{{% /notice %}}

IMPORTANT: This Guide assumes you have a runner available to configure the agent.

This guide uses the **GitLab CI/CD workflow** and the **Single project** approach for agent install and cluster management.

1. At the top of the group heirarchy you wish to connect to the cluster:

   > Guides Through: [Create a new project based on the Cluster Management Template](https://docs.gitlab.com/ee/user/clusters/management_project_template.html#create-a-new-project-based-on-the-cluster-management-template)

   1. In 'classgroup', near the top to the left of Search GitLab,  *Click* **+** (a small button) and then *Click* **New project/repository**
   2. *Click* **Create from template**
   3. On the 'Create from template' page, *Locate* **GitLab Cluster Mangement** and to the right of this name, *Click* **Use template** (button)
   4. On the next page, for 'Project name' *Type* **Cluster Management**
   5. Near the bottom of the page *Click* **Create project** (button)

   IMPORTANT: From here on these instructions will refer to this as 'classgroup/cluster-management'.

2. Register the agent by

   > Guides Through: [Create an agent configuration file](https://docs.gitlab.com/ee/user/clusters/agent/install/#create-an-agent-configuration-file) and [Register the agent with GitLab](https://docs.gitlab.com/ee/user/clusters/agent/install/#create-an-agent-configuration-file)

   1. Within the new project. In the top of the left navigation bar, Click Cluster Management (The title of the project in the nav bar)

   2. Near the top right of the page *Click* **Web IDE** (button)

   3. After opening in the Web IDE, add a directory to 'classgroup/cluster-management' using the <mark class="hlgreen">folder with a plus sign</mark> icon
      in the path specify `.gitlab/agents/spot2azuseast2-agent1` and *Click* **Create directory**.

   4. One In the Web IDE, hover the new leaf directory 'spot2azuseast2-agent1' and *Click* <mark class="hlgreen">the three dots to the right of the directory name</mark> and *Click* **New file**

   5. In the popup, for 'Name' *Type* **config.yaml** (do not abbreviate to `.yml`) and *Click* **Create file**

   6. Place these contents in the file (substitute your actual class group for the text `_replace_with_classgroup_`)

      ```
      ci_access:
        groups:
        - id: _replace_with_classgroup_
        
      observability:
        logging:
          level: debug
      ```

      **Important**: The above configuration represents access to every project below the root level group <mark class="hlgreen">classgroup</mark> the scope of projects can be made tighter with more group levels by continuing to specify a GitLab group hiearchy like <mark class="hlgreen">classgroup/mysubgroup</mark> Also use the <mark class="hlgreen">projects:</mark> key word instead of <mark class="hlgreen">groups:</mark> if scoping right to a project. Documentation: [Authorize the agent](https://docs.gitlab.com/ee/user/clusters/agent/ci_cd_tunnel.html#authorize-the-agent).

   7. *Click* **Create commit...**

   8. *Select* **Commit to master branch**

   9. *Click* **Commit**

   10. > Ignore pipeline failures.

   11. To exit the editor, in the left navigation bar, *Click* **Cluster Management** (the project name)

   12. From the left sidebar, *Select* **Infrastructure > Kubernetes clusters**.

   13. Click **Connect a cluster (agent)** (button). (DO NOT Click the down arrow next to the button)

   14. From the **Select an agent** dropdown list, select <mark class="hlgreen">spot2azuseast2-agent1</mark> and *Click* **Register** (button)

   15. GitLab generates a registration token for this agent. Securely store this secret token. You need it to install the agent in your cluster and to [update the agent](https://docs.gitlab.com/ee/user/clusters/agent/install/#update-the-agent-version) to another version.

   16. Copy the command under **Recommended installation method**. You need it when you use the one-liner installation method to install the agent in your cluster. (or you can leave this popup open to copy the command in the next section)

3. Install the agent by:

   > Guides Through: [Install the agent in the cluster](https://docs.gitlab.com/ee/user/clusters/agent/install/#install-the-agent-in-the-cluster)

   1. If you are not still logged in, login to the EKS Bastion host using SSM as you did in the instructions above.

      > You will need to have logged off and on again after running the docker install command before the below will work.

   2. Paste the 'Recommended installation method' command.
      **Note:** Success is indicated by about 5 lines ending in 'created' and no errors.

   3. Select the browser tab you previous left open ('classgroup/cluster-management' > Cluster Mangement > Kubernetes), If the popup 'Connect a cluster through an agent' is still displaying *Click* **Close**

   4. Refresh the page.

   5. Under 'Agents' your agent named <mark class="hlgreen">spot2azuseast2-agent1</mark> (or whatever your actual name is) should have 'Connected' in the 'Connection status' column.

      > KUBE_CONTEXT and KUBE_NAMESPACE are used for both agent registration and agent usage in Auto DevOps - therefore we are configuring it at the top group level for which we would like the agent to be usable for Auto DevOps for all downbound groups and projects.

   6. In 'classgroup' *Click* **Settings > CI/CD** (Be sure you do this from the top level group, not a project)

   7. Next to 'Variables' *Click* **Expand**

   8. *Click* **Add variable** once for each table row

      | Key                            | Value                                               | Protect | Mask |
      | ------------------------------ | --------------------------------------------------- | ------- | ---- |
      | KUBE_CONTEXT                   | classgroup/cluster-management:spot2azuseast2-agent1 | No      | No   |
      | KUBE_NAMESPACE                 | $CI_PROJECT_NAME-$CI_PROJECT_ID                     | No      | No   |
      | AUTO_DEPLOY_IMAGE_VERSION      | v2.25.0                                             | No      | No   |
      | DAST_AUTO_DEPLOY_IMAGE_VERSION | v2.25.0                                             | No      | No   |

> These variables references in KUBE_NAMESPACE ensure that all branches in all projects in the downbound group hiearchy remain unique and therefore isolated on Kubernetes.

{{% notice warning %}}
AUTO_DEPLOY_IMAGE_VERSION and DAST_AUTO_DEPLOY_IMAGE_VERSION versoin pegging is necessary at this time to prevent errors with deployment. This may be able to be removed when this issue is resolved: [Error when ingressClassName is supported by API but not by the ingress controller](https://gitlab.com/gitlab-org/cluster-integration/auto-deploy-image/-/issues/206)
{{% /notice %}}

{{% notice tip %}}
The EKS cluster is now integrated using the GitLab Kubernetes Agent is now available and could be used as-is to manage cluster integration that is purely GitOps oriented (no CI / CD pushes into the cluster). In fact, the agent is now used in the next section to pull some managed apps onto the cluster that are needed for Auto DevOps operation.
{{% /notice %}}

## Configuring Ingress with Built-in nip.io SSL for Auto DevOps

> **Keyboard Time**: 20 mins, **Automation Wait Time**: 30 min
>
> **Scenarios:** Instructor-Led, Self-Paced

### Update Cluster Management Project to Install the NGINX Ingress and Cert Manager

{{% notice info %}}
When this section is complete applications deployed to the EKS cluster using Auto DevOps will have full dynamic [review environments](https://docs.gitlab.com/ee/ci/review_apps/) support with SSL urls.
{{% /notice %}}

1. In 'classgroup/cluster-management' *Start* the **Web IDE**.

2. *Edit* the file 'helmfile.yaml'

   > In the following commands ONLY delete the hash character (<mark class="hlpink">#</mark>) character to preserve the needed yaml indentations

3. To enable cert manager with nip.io SSL *Uncomment* the line <mark class="hlgreen">- path: applications/cert-manager/helmfile.yaml</mark>

4. To enable the ingress controller *Uncomment* the line <mark class="hlgreen">- path: applications/ingress/helmfile.yaml</mark>

5. To configure cert-manager, Edit <mark class="hlgreen">applications/cert-manager/helmfile.yaml</mark>

6. **IMPORTANT**: At the bottom of the file find <mark class="hlgreen">email: example@example.com</mark> and change <mark class="hlgreen">example@example.com</mark> to a valid email address.

7. To configure ingress, Edit <mark class="hlgreen">applications/ingress/values.yaml</mark>

8. At the bottom add the following (ENSURE that the indenting of <mark class="hlgreen">config:</mark> aligns with <mark class="hlgreen">podAnnotations:</mark> above it)

   ```
     config:
       # pass the X-Forwarded-* headers directly from the upstream
       use-forwarded-headers: "true"
   ```

9. *Click* **Create commit...**

10. *Select* **Commit to master branch**

11. *Click* **Commit**

12. Wait for the deployment to complete successfully by watching the pipeline that was just created by your commit.

    **Note:** You can also use the bastion host to run `kubectl get pods --all-namespaces` and look for some pods starting with `certmanager` and some starting with `ingress`

13. To see the new AWS load balancer created by the ingress chart, in the 'AWS EC2 Console', in the left hand navigation, under 'Load balancing', *Click* **Load Balancers** 

14. If there is more than one load balancer, examine their 'Tag' details until you find one with the **Key = kubernetes.io/cluster/spot2azuseast2** with **Value = owned**

15. Once you have located the appropriate load balancer, in the details tabs *Click* **Description**

16. Find 'DNS Name' and *Copy* <mark>\<the Load Balancer DNS Name\></mark>

17. In a web browser open https://dnschecker.org

18. Under 'DNS Check', over top 'example.com', *Paste* <mark>\<the Load Balancer DNS Name\></mark>

19. *Click* **Search**

20. If you setup EKS for 2 availability zones, you should see 2 IP addresses being returned by most or all locations.

21. If there are red X's instead of IPs, wait and keep clicking "Search" until you see IP addresses.

22. Copy one of the <mark>\<the Load Balancer IP\></mark>'s

23. On https://dnschecker.org, *Paste* the  <mark>\<the Load Balancer IP\></mark> over the <mark>\<the Load Balancer DNS Name\></mark> and append <mark>.nip.io</mark> (take note of both dots)

24. *Click* **Search**

    > If nip.io is fast, you will see it resolves to the same IP address. Auto DevOps URLs and SSL will not work correctly until this is resolving, but configuration can continue and it will likely be done by the time all configuration is done.

25. If there are red X's instead of IPs, wait and keep clicking "Search" until you see IP addresses.

26. Copy the DNS name <mark>\<the Load Balancer DNS Name\>.nip.io</mark>

27. In the GitLab group 'classgroup' *Click* **Settings > CI/CD**

    > KUBE_INGRESS_BASE_DOMAIN is used for Auto DevOps - therefore we are configuring it at the top group level for which we would like the cluster in question to be usable for Auto DevOps for all downbound groups and projects.

28. Next to 'Variables' *Click* **Expand**

29. *Click* **Add variable** once for each table row (or update the variables if they are already there)

    | Key                      | Value                                              | Protect | Mask |
    | ------------------------ | -------------------------------------------------- | ------- | ---- |
    | KUBE_INGRESS_BASE_DOMAIN | <mark>\<the Load Balancer DNS Name\>.nip.io</mark> | No      | No   |

{{% notice tip %}}
Applications deployed to the EKS cluster using Auto DevOps will now have full dynamic [review environments](https://docs.gitlab.com/ee/ci/review_apps/) support with SSL urls.
{{% /notice %}}