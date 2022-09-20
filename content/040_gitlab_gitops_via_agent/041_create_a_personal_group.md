---
title: "Lab 4.1: Create Personal Group"
weight: 41
chapter: true
draft: false
description: "See Auto DevOps in action. The same configuration enables GitLab CD using helm and kubectl commands."
#pre: '<i class="fa fa-film" aria-hidden="true"></i> '
---

# Lab 4.1: Create a Personal Group

> **Keyboard Time**: 1 mins, **Automation Wait Time**: 0 mins
>
> **Scenarios:** Instructor-Led, Self-Paced

{{< admonition type=warning title="Warning" open=true >}}
Skip this lab if you already have a personal group created by a previous lab.
{{< /admonition >}}

1. While in 'classgroup', near the top right of the page, *Click* **New subgroup** (button)

2. Name the group with the mask `firstname_lastname` so that it will be unique, easy to remember and easy for others to identify. (For example if your gitlab user id is @supercoolcoder and your avatar URL is https://gitlab.com/supercoolcoder, name your subgroup ‘supercoolcoder’). 
    From here on in the exericses this will be referred to as 'yourpersonalgroup’

3. *Click* **Create Group**.

4. On the left hand navigation *Click* **Settings**

5. Next to “General”, *Click* **Expand**

6. For “Visibility Level”, *Check* **Public**.

    {{< admonition type=warning title="Must Be Public" open=true >}}

Projects that are used by the GitLab Agent must be public when the agent registration is done in a project other than the one the deployment happens from and when the image being sourced is not using a stored docker login secret.
    {{< /admonition >}}

7. **Record or remember** 'yourpersonalgroup' = _________________________________________________

{{< admonition type=warning title="IMPORTANT" open=true >}}
Throughout the remaining exercises you will replace the text  <mark class="hlgreen">yourpersonalgroup</mark> with this actual group name.
{{< /admonition >}}