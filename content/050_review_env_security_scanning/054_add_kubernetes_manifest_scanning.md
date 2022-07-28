---
title: "Lab 5.4: Add Kubernetes Manifest Security Scanning"
weight: 54
chapter: true
draft: false
description: "Security scanning in the application project."
#pre: '<i class="fa fa-film" aria-hidden="true"></i> '
---

# Lab 5.4: Add Kubernetes Manifest Security Scanning

> **Keyboard Time**: 15 mins, **Automation Wait Time**: 8 mins
>
> **Scenarios:** Instructor-Led, Self-Paced

{{< admonition type=gitlab title="Kubernetes Manifest SAST Security Scanning" open=true >}}

GitLab can do kubernetes manifest scanning using [Kubesec](https://github.com/controlplaneio/kubesec).

{{< /admonition >}}

{{< admonition type=abstract title="Target Outcomes" open=true >}}

1. configures the security scanning of fully rendered Kubernetes manifests for security problems.

{{< /admonition >}}

{{< admonition type=warning title="CI Runner Required" open=true >}}

While this project only uses the GitLab Agent Pull CD mode for deployment, it does need a runner if we want to do additional CI tasks in this project. In this case we want to add manifest scanning, which can only be done in the Environment Deployment project where the full manifests are constructed.

{{< /admonition >}}

1. Open 'yourpersonalgroup/world-greetings-env-1’

2. On the upper right of the Project page, *Click* **Web IDE**

3. In the left side file browser, *Click* **update-manifests.gitlab-ci.yml**

   > You will be editing YAML - be careful that tabbing is properly aligned. Only removing the comment character (“#”) should result in proper tabbing.

4. Under `include:` uncomment `- template: Security/SAST-IaC.latest.gitlab-ci.yml` which should make the section look like this.:

   ```yaml
   include: 
     - local: .gitlab/ci_templates/git-push.yaml
     - template: Security/SAST-IaC.latest.gitlab-ci.yml
   ```

5. Under `variables:` uncomment the `variables:` heading and the two variable which should make the section look like this:

   ```
   variables:
     SCAN_KUBERNETES_MANIFESTS: "true"
     KUBESEC_HELM_CHARTS_PATH: $CI_PROJECT_DIR/constructed-manifests/
   ```

6. In the left side file browser, Click packages/hello-world/base/deployment.yaml

7. At the bottom of the file edit the `securityContext:` section to look like this (be sure to keep the same indentation starting by not moving the existing keyword `securityContext` and indenting sub levels by two spaces):
   ```
         securityContext:
           capabilities:
             add:
               - SYS_ADMIN
   ```

8. *Click* **Create commit...**

9. *Select* **Commit to main branch** (this is not the default)

10. Under ‘Commit Message’, *Type* **[skip ci] Adding Manifest Security Scanning**

11. *Click* **Commit**

12. Below the Create commit… button, in the status bar, *Click* **[the pipeline #]**

13. Expand the Downstream pipeline with the great than arrow (`>`).

14. Under the new stage ‘Test‘, *Locate* the new job **kics-iac-sast** 

15. *Click* **kics-iac-sast**

16. Near the bottom of the log, *Locate* **gl-sast-report.json : found 1 files and directories**

17. On the left navigation, *Click* **Security & Compliance => Vulnerability report**

18. If there are not any vulnerabilities listed, you can examine the page for `Last updated` followed by an elapsed time and a clickable pipeline id reference.

{{< admonition type=success title="Accomplished Outcomes" open=true >}}

1. configures the security scanning of fully rendered Kubernetes manifests for security problems.

{{< /admonition >}}
