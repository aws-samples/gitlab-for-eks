---
title: "Lab 6.1: Integrated Security Code Warrior Security Training (Optional)"
weight: 61
chapter: true
draft: false
description: "Security training integrated into GitLab findings"
---

# Lab 6.1: Enable Security Code Warrior Integration and Take Training

> **Keyboard Time**: 20 mins, **Automation Wait Time**: 0 mins
>
> **Scenarios:** Instructor-Led, Self-Paced

{{< admonition type=gitlab title="GitLab Partner Security Integrations" open=true >}}

The Security Code Warrior integration allows developers to immediately access highly relevant and brief training to understand the nature of a specific vulnerabilities and why it should be remediated. It helps with the Why and the How of vulnerability management.

{{< /admonition >}}

{{< admonition type=warning title="Prerequisite Labs" open=true >}}

This lab assumes you have completed Labs 3.1 and 3.2 - please do those now if you have not done them yet.

{{< /admonition >}}

{{< admonition type=abstract title="Target Outcomes" open=true >}}

1. Configure Security Code Warrior training integration.

2. Locate and use the link to training on a finding.

3. Watch the relevant training video.

4. Optionally participate in the Security Code Warrior game to practice your remediation skills.

   {{< /admonition >}}

1. Open 'yourpersonalgroup/simply-simple-notes’

2. On the left sidebar, *Click* **Security & Compliance => Configuration**.

3. On the tab bar, *Click* **Vulnerability Management**

4. To the right of ‘Security Code Warrior’ *Click* **[the toggle button]**

   > The integration is now enabled. It is not necessary to enable it BEFORE security scanning occurs.

5. On the left sidebar, *Click* **Security & Compliance => Vulnerability report**

6. In the filter bar (with drop down boxes), under ‘Tool’, **Select** **DAST**

7. In the bottom right drop down box, *Select* **Show 100 items**

8. *Browse or Search for*, **Cookie Without Secure Flag**

9. *Click* **Cookie Without Secure Flag** 

10. Near the bottom of the page, *locate the heading* **Training**.

11. *Click* **View training**

    > A new window opens on the Security Code Warrior site

12. At the ‘Log in to track your progress page’, *Click* **Continue as guest**

    > A world map with a simulated attack displays.

13. In the upper left of the screen notice the text “Security Misconfiguration” followed by “Disabled Security Features”

14. In the ‘Language’ drop down, *Select* **Python API** (you can use the search box to narrow the choices)

15. In the lower left corner, *Click* **Enter game mode**

    > You are now placed in a virtual environment to identify a vulnerability similar to the one in your code.

16. *Click* **Hint** (green button)

17. At the ‘Open A Hint?’ popup, *Click* **Give me a hint!**

18. To hear the two minute video, *Click* **[the play control]**

    > The video discusses disabled security features including cookies not marked ‘private’

19. Close the browser tab for Code Security Warrior.

{{< admonition type=success title="Accomplished Outcomes" open=true >}}

1. Configure Security Code Warrior training integration.

2. Locate and use the link to training on a finding.

3. Watch the relevant training video.

4. Optionally participate in the Security Code Warrior game to practice your remediation skills.

   {{< /admonition >}}
