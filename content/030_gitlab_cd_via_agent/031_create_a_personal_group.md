---
title: "Lab 3.1: Create Personal Group"
weight: 31
chapter: true
draft: false
description: "See Auto DevOps in action. The same configuration enables GitLab CD using helm and kubectl commands."
#pre: '<i class="fa fa-film" aria-hidden="true"></i> '
---

# Lab 3.1: Create a Personal Group

> **Keyboard Time**: 1 mins, **Automation Wait Time**: 0 mins
>
> **Scenarios:** Instructor-Led, Self-Paced

1. While in 'classgroup', near the top right of the page, *Click* **New subgroup** (button)

2. Name the group with your gitlab user name so that it will be unique, easy to remember and easy for others to identify. (For example if your gitlab user id is @supercoolcoder and your avatar URL is https://gitlab.com/supercoolcoder, name your subgroup ‘supercoolcoder’). 
    From here on in the exericses this will be referred to as 'yourpersonalgroup'

3. *Click* **Public**.

4. *Click* **Create Group**.

    {{< admonition type=warning title="Must Be Public" open=true >}}

    Application Build Projects that are used by the GitLab Agent for Kubernetes must be public.

    {{< /admonition >}}

5. **Record or remember** 'yourpersonalgroup' = _________________________________________________

{{< admonition type=warning title="IMPORTANT" open=true >}}
Throughout the remaining exercises you will replace the text  <mark class="hlgreen">yourpersonalgroup</mark> with this actual group name.
{{< /admonition >}}