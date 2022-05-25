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

{{% notice tip%}}

This lab configures the security scanning of fully rendered Kubernetes manifests for security problems.

{{% /notice %}}

1. Open 'yourpersonalgroup/world-greetings-env-1’

2. In the left navigation, *Click* **Repository => Files** 

3. On the upper right of the Project page, *Click* **Web IDE**

4. In the left side file browser, *Click* **.gitlab-ci.yml**

   > You will be editing YAML - be careful that tabbing is properly aligned. Only removing the comment character (“#”) should result in proper tabbing.

5. Under `include:` uncomment `- template: Security/SAST-IaC.latest.gitlab-ci.yml*` which should make the section look like this.:

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

7. *Click* **Create commit...**

8. *Select* **Commit to main branch**

9. Under ‘Commit Message’, *Type* **[skip ci] Adding Manfest Security Scanning**

10. *Click* **Commit**

11. On the left navigation, *Click* **CI/CD => Pipelines**

12. On the latest running pipeline, *Click* **[the Status badge]** or **[the pipeline #]**

13. Under the new stage ‘Test‘, *Locate* the new job **kics-iac-sast** 

14. Near the bottom of the log, *Locate* **gl-sast-report.json : found 1 files and directories**

15. On the left navigation, *Click* **Security & Compliance => Vulnerability report**

16. Examine the various vulnerabilities.

17. To open a vulnerability object, *Click* **[the Description text on one of them]**

18. *Click* **Create issue**

    > Notice all the information is transferred to the issue. The issue can be used to collaborate with others or schedule the finding for another time.

19. *Click* **Create issue**

