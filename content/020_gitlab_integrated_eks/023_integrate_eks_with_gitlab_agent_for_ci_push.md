---
title: "Prep Lab 2.3: Using GitLab K8s Agent to Integrate The Cluster with GitLab"
chapter: true
weight: 23
description: "Setup the most secure and capable method of integrating Kubernetes with GitLab."
---

# Prep Lab 2.3: Use GitLab K8s Agent to Integrate The Cluster with GitLab

{{< admonition type=abstract title="Target Outcomes" open=true >}}

1. Integrate a Kubernetes Cluster with GitLab using the GitLab Agent.
2. Create the integration at a group level so it works for all subgroups and projects.
3. **[Dev Environments Only]** Enable applications deployed to the EKS cluster using Auto DevOps will have full dynamic [review environments](https://docs.gitlab.com/ee/ci/review_apps/) support, which requires:
   1. Can resolve External, Dynamic DNS names (via the dynamic DNS service nip.io combined with the Ingress).
   2. With SSL urls (via cert-manager).{{< /admonition >}}

## Configure and Install GitLab Kubernetes Agent for CI Push

> **Keyboard Time**: 20 mins, **Automation Wait Time**: 20 mins
>
> **Scenarios:** Instructor-Led, Self-Paced
>
> Guides Through: [Installing the agent for Kubernetes](https://docs.gitlab.com/ee/user/clusters/agent/install) 

{{< admonition type=tip title="Tip" open=true >}}
This specific agent configuration combined with the values and placement of the CI/CD variables starting with “KUBE_” accomplish CI Push integration for the entire heirarchy of projects under 'classgroup'. This mimics the way that GitLabs legacy certificate connection method was able to integrate and entire heirarchy of projects - so this specific configuration may be useful in your actual development processes. A cluster integrated in this way will not appear in the group list of Kubernetes Clusters in the GitLab UI.
{{< /admonition >}}

IMPORTANT: This Guide assumes you have a runner available to configure the agent.

{{< admonition type=abstract title="Target Outcomes" open=true >}}
1. Agent registration and installation.
2. Configuration of the class group and all subgroups to use **Traditional Runner CD Push** to a Kubernetes cluster.
{{< /admonition >}}

This guide uses the **GitLab CI/CD workflow** and the **Single project** approach for agent install and cluster management.

1. While in ‘classgroup’:

   > Guides Through: [Create a new project based on the Cluster Management Template](https://docs.gitlab.com/ee/user/clusters/management_project_template.html#create-a-new-project-based-on-the-cluster-management-template)

   1. In 'classgroup', near the top to the left of Search GitLab,  *Click* **+** (a small button) and then *Click* **New project/repository**
   2. *Click* **Create from template**
   3. On the 'Create from template' page, *Locate* **GitLab Cluster Mangement** and to the right of this name, *Click* **Use template** (button)
   4. On the next page, for 'Project name' *Type* **Cluster Management**
   5. Near the bottom of the page *Click* **Create project** (button)

   IMPORTANT: From here on these instructions will refer to this as 'classgroup/cluster-management'.

2. Register the agent by

   > Guides Through: [Create an agent configuration file](https://docs.gitlab.com/ee/user/clusters/agent/install/#create-an-agent-configuration-file) and [Register the agent with GitLab](https://docs.gitlab.com/ee/user/clusters/agent/install/#create-an-agent-configuration-file)

   1. Open ‘classgroup/cluster-management’ (would be the default state from the previous step above)

   2. In the top of the left navigation bar, *Click* **Cluster Management** (The title of the project in the nav bar)

   3. Near the top right of the page *Click* **Web IDE** (button)

   4. After opening in the Web IDE, add a directory to 'classgroup/cluster-management' *Locate* the word **Edit**

   5. Next to ‘Edit’, *Click* the <mark class="hlgreen">folder with a plus sign</mark> icon
      in the path specify `.gitlab/agents/spot2azuseast2-agent1` and *Click* **Create directory**.

   6. In the Web IDE file selector, *Click* the new leaf directory level '**spot2azuseast2-agent1**'

   7. *Click* <mark class="hlgreen">the three dots to the right of the directory name</mark> and *Click* **New file**

   8. In the popup, for 'Name' *Type* **config.yaml** (do not abbreviate to `.yml`) and *Click* **Create file**

   9. IMPORTANT: Verify that your full file path is `.gitlab/agents/spot2azuseast2-agent1/config.yaml` - the agent registration will not show the option to register `spot2azuseast2` if the path is not exactly like this.

   10. Place these contents in the file - substitute your actual class group name for the text `_replace_with_path_to_classgroup_` for instance for a group https://gitlab.com/awesomeclassgroup you would specify only `awesomeclassgroup`.

      >  Dispite the `id:` yaml parameter name this is the text group path, not the group id.

      ```
      ci_access:
        groups:
        - id: _replace_with_path_to_classgroup_
        
      observability:
        logging:
          level: debug
      ```

      **Important**: The above configuration represents Traditional Runner CD Push access to every project below the root level group <mark class="hlgreen">classgroup</mark> the scope of projects can be made tighter with more group levels by continuing to specify a GitLab group hiearchy like <mark class="hlgreen">classgroup/mysubgroup</mark> Also use the <mark class="hlgreen">projects:</mark> key word instead of <mark class="hlgreen">groups:</mark> if scoping right to a project. Documentation: [Authorize the agent](https://docs.gitlab.com/ee/user/clusters/agent/ci_cd_tunnel.html#authorize-the-agent).

   11. *Click* **Create commit...**

   12. **IMPORTANT:** *Select* **Commit to master branch** (non-default selection)

   13. *Click* **Commit**

       > Ignore pipeline failures.

   14. To exit the editor, in the left navigation bar at the top, *Click* **Cluster Management** (the project name)

   15. From the left sidebar, *Select* **Infrastructure > Kubernetes clusters**.

   16. Click **Connect a cluster** (button). (DO NOT Click the down arrow next to the button)

   17. In the field **Select an agent or enter a new name to create new** Click the dropdown list arrow, select <mark class="hlgreen">spot2azuseast2-agent1</mark> and *Click* **Register** (button)

   18. GitLab generates a registration token for this agent. Securely store this secret token. You need it to install the agent in your cluster and to [update the agent](https://docs.gitlab.com/ee/user/clusters/agent/install/#update-the-agent-version) to another version.

   19. **IMPORTANT:** Copy the command under **Recommended installation method**. You need it when you use the one-liner installation method to install the agent in your cluster. (or you can leave this popup open to copy the command in the next section)

   20. Do not close the dialog nor browser window until you have successfully run the command in the next steps.

3. Install the agent by:

   > Guides Through: [Install the agent in the cluster](https://docs.gitlab.com/ee/user/clusters/agent/install/#install-the-agent-in-the-cluster)

   1. [Click here to open the EC2 Instances Console in us-east-2](https://us-east-2.console.aws.amazon.com/ec2/v2/home?region=us-east-2#Instances:instanceState=running)

   2. In the EC2 Instances console, *locate* the instance named **EKSBastion**
   
   3. *Right click* **the instance**, *select* => **Connect**  => **Session Manager** => **Connect** (button)
   
      {{< admonition type=tip title="Remember The Above Sequence" open=true >}}
      kubectl and helm are now available on your path and the bastion instance already has administrative permissions to the cluster. Remember the above sequence for gaining access to CLI based cluster admin.
      {{< /admonition >}}
   
   4. After the command prompt appears, *Paste* the **'Recommended installation method'** command from the previous page.
      **Note:** Success is indicated by about 9 lines of logging with no errors.

   5. Select the browser tab you previous left open ('classgroup/cluster-management' > Cluster Mangement > Kubernetes), If the popup 'Connect a Kuberntes cluster' is still displaying *Click* **Close**

   6. Refresh the page.

   7. Under 'Agents' your agent named <mark class="hlgreen">spot2azuseast2-agent1</mark> (or whatever your actual name is) should have 'Connected' in the 'Connection status' column. If it does not show connected yet, keep refreshing until it does.

      > KUBE_CONTEXT and KUBE_NAMESPACE are used for both agent registration and agent usage in Auto DevOps - therefore we are configuring it at the top group level for which we would like the agent to be usable for Auto DevOps for all downbound groups and projects.

   8. Navigate away from the Cluster Management project, to 'classgroup' (There should be a clickable breadcrumb trail on teh cluster page to go directly)
   
   9. *Click* **Settings > CI/CD** (Be sure you do this from the ‘classgroup’, not a project).
   
      > IMPORTANT: This menu is nest under “Setings” it is NOT the direct menu choice “CI/CD”
   
   10. Next to 'Variables' *Click* **Expand**
   
   11. *Click* **Add variable** once for each table row and specify the variables settings as indicated in the table. Be sure to substitute your actual classgrouppath in KUBE_CONTEXT.
   
       Use the variable references in KUBE_NAMESPACE exactly (literally) as documented in the table.
   
       | Key                            | Value                                                        | Protect | Mask |
       | ------------------------------ | ------------------------------------------------------------ | ------- | ---- |
       | KUBE_CONTEXT                   | <mark class="hlgreen">classgroup</mark>/cluster-management:spot2azuseast2-agent1 | No      | No   |
       | KUBE_NAMESPACE                 | $CI_PROJECT_NAME-$CI_PROJECT_ID                              | No      | No   |
       | AUTO_DEPLOY_IMAGE_VERSION      | v2.25.0                                                      | No      | No   |
       | DAST_AUTO_DEPLOY_IMAGE_VERSION | v2.25.0                                                      | No      | No   |

> These variables references in KUBE_NAMESPACE ensure that all branches in all projects in the downbound group hiearchy remain unique and therefore isolated on Kubernetes.

{{< admonition type=warning title="Warning" open=true >}}
AUTO_DEPLOY_IMAGE_VERSION and DAST_AUTO_DEPLOY_IMAGE_VERSION version pegging is necessary at this time to prevent errors with deployment. This may be able to be removed when this issue is resolved: [Error when ingressClassName is supported by API but not by the ingress controller](https://gitlab.com/gitlab-org/cluster-integration/auto-deploy-image/-/issues/206)
{{< /admonition >}}

{{< admonition type=success title="Accomplished Outcomes" open=true >}}
1. Agent registration and installation.
2. Configuration of the class group and all subgroups to use **Traditional Runner CD Push** to a Kubernetes cluster.
{{< /admonition >}}

## Configuring Ingress with Built-in nip.io SSL for Auto DevOps

> **Keyboard Time**: 20 mins, **Automation Wait Time**: 30 min
>
> **Scenarios:** Instructor-Led, Self-Paced

### Update Cluster Management Project to Install the NGINX Ingress and Cert Manager

{{< admonition type=warning title="Not For Production" open=true >}}
Production application setups would generally not use this specific Ingress install, nor cert-manager or nip.io - these are all used for the convenience of quick demo and training setups.
{{< /admonition >}}

1. In 'classgroup/cluster-management' *Start* the **Web IDE**.

2. In the Web IDE file navigation, *Click* **.gitlab-ci.yml**

3. Under the job ‘**detect-helm2-releases**’, *Locate* the line starting with **image:**

4. At the end of the line change the portion after the colon (“:”) to **v1.6.0**

   > Final result: image: "registry.gitlab.com/gitlab-org/cluster-integration/cluster-applications:v1.6.0”

5. Under the job ‘**apply**’, *Locate* the line starting with **image:**

6. At the end of the line change the portion after the colon (“:”) to **v1.6.0**

   > Final result: image: "registry.gitlab.com/gitlab-org/cluster-integration/cluster-applications:v1.6.0"

7. In the Web IDE file navigation, *Click* **helmfile.yaml**

   > In the following commands ONLY delete the hash character (<mark class="hlpink">#</mark>) character to preserve the needed yaml indentations

8. To enable the ingress controller *Uncomment* the line <mark class="hlgreen">- path: applications/ingress/helmfile.yaml</mark>

9. To enable cert manager with nip.io SSL *Uncomment* the line <mark class="hlgreen">- path: applications/cert-manager/helmfile.yaml</mark>

10. To configure cert-manager, Edit <mark class="hlgreen">applications/cert-manager/helmfile.yaml</mark> (file is in the subdirectory path ‘applications/cert-manager’)

11. **IMPORTANT**: At the bottom of the file find <mark class="hlgreen">email: example@example.com</mark> and change <mark class="hlgreen">example@example.com</mark> to a valid email address.

12. To configure ingress, Edit <mark class="hlgreen">applications/ingress/values.yaml</mark>

13. At the bottom add the following (ENSURE that the indenting of <mark class="hlgreen">config:</mark> aligns with <mark class="hlgreen">podAnnotations:</mark> above it)

    ```yaml
      config:
        # pass the X-Forwarded-* headers directly from the upstream
        use-forwarded-headers: "true"
    ```

    The result should roughly be (assuming the template has not changed since this writing):
    ```yaml
    controller:
      stats:
        enabled: true
      podAnnotations:
        prometheus.io/scrape: "true"
        prometheus.io/port: "10254"
      config:
        # pass the X-Forwarded-* headers directly from the upstream
        use-forwarded-headers: "true" 
    ```

14. *Click* **Create commit...**

15. **IMPORTANT**: *Select* **Commit to master branch** (non-default selection)

16. *Click* **Commit**

17. Wait for the deployment to complete successfully by watching the pipeline that was just created by your commit.

    **Note:** You can also use the bastion host to run `kubectl get pods --all-namespaces` and look for some pods starting with <mark>certmanager</mark> and some starting with <mark>ingress</mark> in the namespace <mark>gitlab-managed-apps</mark>.

18. To see the new AWS load balancer created by the ingress chart, in the 'AWS EC2 Console', in the left hand navigation, under 'Load balancing', *Click* **Load Balancers** 

19. If there is more than one load balancer, examine their 'Tag' details until you find one with the **Key = kubernetes.io/cluster/spot2azuseast2** with **Value = owned**

20. Once you have located the appropriate load balancer, in the details tabs *Click* **Description**

21. Find 'DNS Name' and *Copy* <mark>\<the Load Balancer DNS Name\></mark>

22. In a web browser open https://dnschecker.org

23. Under 'DNS CHECK', over top 'example.com', *Paste* <mark>\<the Load Balancer DNS Name\></mark>

24. *Click* **Search**

25. If you setup EKS for 2 availability zones, you should see 2 IP addresses being returned by most or all locations and green checkmarks after the IPs.

26. If there are red X's instead of IPs, wait and keep clicking "Search" until you see IP addresses.

27. Copy one of the <mark>\<the Load Balancer IP\></mark>'s

28. On https://dnschecker.org, *Paste* the  <mark>\<the Load Balancer IP\></mark> over the <mark>\<the Load Balancer DNS Name\></mark> and append <mark>.nip.io</mark> (take note of both dots)

    > The complete string should be <mark>\<the Load Balancer IP\>.nip.io</mark>

29. *Click* **Search**

    > If nip.io is fast, you will see it resolves to the same IP address. Auto DevOps URLs and SSL will not work correctly until this is resolving, but configuration can continue and it will likely be done by the time all configuration is done.

30. If there are red X's instead of IPs, wait and keep clicking "Search" until you see IP addresses.

31. Copy the DNS name <mark>\<the Load Balancer IP\>.nip.io</mark>

32. In the GitLab group 'classgroup' *Click* **Settings > CI/CD**

    > KUBE_INGRESS_BASE_DOMAIN is used for Auto DevOps - therefore we are configuring it at the top group level for which we would like the cluster in question to be usable for Auto DevOps for all downbound groups and projects.

33. Next to 'Variables' *Click* **Expand**

34. *Click* **Add variable** once for each table row (or update the variables if they are already there)

    | Key                      | Value                                        | Protect | Mask |
    | ------------------------ | -------------------------------------------- | ------- | ---- |
    | KUBE_INGRESS_BASE_DOMAIN | <mark>\<the Load Balancer IP\>.nip.io</mark> | No      | No   |

{{< admonition type=success title="Accomplished Outcomes" open=true >}}

1. Integrate a Kubernetes Cluster with GitLab using the GitLab Agent.
2. Create the integration at a group level so it works for all subgroups and projects.
3. **[Dev Environments Only]** Enable applications deployed to the EKS cluster using Auto DevOps will have full dynamic [review environments](https://docs.gitlab.com/ee/ci/review_apps/) support, which requires:
   1. Can resolve External, Dynamic DNS names (via the dynamic DNS service nip.io combined with the Ingress).
   2. With SSL urls (via cert-manager).{{< /admonition >}}