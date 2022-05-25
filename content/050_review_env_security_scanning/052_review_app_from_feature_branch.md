---
title: "Lab 5.2: Merge Request and Review App (Optional)"
weight: 52
chapter: true
draft: false
description: "Security scanning in the application project."
#pre: '<i class="fa fa-film" aria-hidden="true"></i> '
---

# Lab 5.2: Merge Request and Review App (Optional)

> **Keyboard Time**: 10 mins, **Automation Wait Time**: 5 mins
>
> **Scenarios:** Instructor-Led, Self-Paced

In this Lab you will update the application project to have a review application and perform security scanning.

1. Open 'yourpersonalgroup/hello-world’

2. Near the top right of the page **Click** ***\*Web IDE\**** (button)

3. *Navigate to* the file **src/microwebserver.py**

4. Around line 11, *locate* the text `<BODY style="background:lightsalmon">`

5. Change the color after the word `background:` to `beige`

   Result: `<BODY style="background:beige">`

   > Other available color values [are listed here](https://www.w3.org/wiki/CSS/Properties/color/keywords)

6. *Click* **Create commit...**

7. *Select* **Create a new branch**

   {{% notice info%}}

   The action of creating a new branch is what triggers an MR and a per-feature branch review app. Commiting to the main branch would avoid the creation of a feature branch review environment - but this also prevents the developer’s Single Pane of Glass (SPOG) assessment of their changes in the Merge Request view and prevents multiple developers working on changes to the same codebase.

   {{% /notice %}}

7. Leave the default branch name.

7. *Check* **Start a new merge request** (should be defaulted to checked)

11. *Click* **Commit**

12. On the ‘New merge request’ page, for Title, *Type* **Updating color**

13. *Click* **Create merge request**

    > You will be put within the context of the new merge request

14. On the Merge Request page notice the tab bar containing Overview, Commits, Pipelines and Changes.

15. *Click* **Pipelines**

    > Merge Request pipelines are specifically associated with a Merge Request as well as the underlying branch.

16. To open the pipeline page, *Click* **[the Status badge]** or **[the pipeline #]**

    > The Review stage is where GitLab is creating a temporary copy of the running application so that Dynamic Application Security Testing (DAST) can be performed or any other type of QA requiring a running copy of the application.

17. **[Automation wait: ~5 min]** Wait for the Review stage to complete.

18. On the left navigation, *Click* **Deployments => Environments**

19. Next to ‘review/<your_mr_branch_name>’ *Click* **Open**.

20. This review environment should have the new background color.

21. In the browser tabs, *Click* **Environments**

22. Next to ‘production’ *Click* **Open**

23. The production environment should have the original background color.

24. In the browser tabs, *Click* **Environments**

25. On the left navigation, *Click* **Merge Requests**

26. *Click* **[your merge request (“Updating color”)]**

27. In the main page body, next to ‘Security scanning detected…’, *Click* **Expand**

{{% notice info%}}

All the security findings and code quality updates in a Merge Request are just for the code that has been changed from the branch you branched from. The context that these are from code that was just changed and the timing during the regular development cycle both function to increases motiviation to handle the found vulnerabilities. In this case the findings are from the entire default branch because a merge of these findings has not yet been performed.

In production development the developer would now iteratively update this branch to get their code working as intended and resolve all CI checks and test resultsthat are generated during that procses.

{{% /notice %}}