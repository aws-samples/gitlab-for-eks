---
title: "Lab 5.4: Add Kubernetes Manifest Security Scanning (Optional)"
weight: 54
chapter: true
draft: false
description: "Security scanning in the application project."
#pre: '<i class="fa fa-film" aria-hidden="true"></i> '
---

# Lab 5.4: Add Kubernetes Manifest Security Scanning (Optional)

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

2. In the left navigation, *Click* **CI/CD => Editor** 

3. On the upper right of the Project page, *Click* **Web IDE**

4. In the left side file browser, *Click* **update-manifests.gitlab-ci.yml**

   > You will be editing YAML - be careful that tabbing is properly aligned. Only removing the comment character (“#”) should result in proper tabbing.

5. Under `include:` uncomment `- template: Security/SAST-IaC.latest.gitlab-ci.yml` which should make the section look like this.:

   ```yaml
   include: 
     - local: .gitlab/ci_templates/git-push.yaml
     - template: Security/SAST-IaC.latest.gitlab-ci.yml
   ```

6. Under `variables:` uncomment the `variables:` heading and the two variable which should make the section look like this:

   ```
   variables:
     SCAN_KUBERNETES_MANIFESTS: "true"
     KUBESEC_HELM_CHARTS_PATH: $CI_PROJECT_DIR/constructed-manifests/
   ```

7. In the left side file browser, Click packages/hello-world/base/deployment.yaml

8. At the bottom of the file edit the `securityContext:` section to look like this (be sure to keep the same indentation starting by not moving the existing keyword `securityContext` and indenting sub levels by two spaces):
   ```
         securityContext:
           capabilities:
             add:
               - SYS_ADMIN
   ```

9. *Click* **Create commit...**

10. *Select* **Commit to main branch** (this is not the default)

11. Under ‘Commit Message’, *Type* **[skip ci] Adding Manfest Security Scanning**

12. *Click* **Commit**

13. Below the Create commit… button, in the status bar, *Click* **[the pipeline #]**

14. Expand the Downstream pipeline with the great than arrow (`>`).

15. Under the new stage ‘Test‘, *Locate* the new job **kics-iac-sast** 

16. *Click* **kics-iac-sast**

17. Near the bottom of the log, *Locate* **gl-sast-report.json : found 1 files and directories**

18. On the left navigation, *Click* **Security & Compliance => Vulnerability report**

19. If there are not any vulnerabilities listed, you can examine the page for `Last updated` followed by an elapsed time and a clickable pipeline id reference.

{{< admonition type=success title="Accomplished Outcomes" open=true >}}

1. configures the security scanning of fully rendered Kubernetes manifests for security problems.

{{< /admonition >}}
