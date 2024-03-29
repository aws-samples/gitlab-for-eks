---
title: "Prep Lab 2.2: Prepare GitLab classgroup and Deploy a Runner"
chapter: true
weight: 22
description: "Setup the most secure and capable method of integrating Kubernetes with GitLab."
---

# Prep Lab 2.2: Prepare GitLab classgroup and Deploy a Runner

> **Keyboard Time**: 20 mins, **Automation Wait Time**: 20 mins
>
> **Scenarios:** Instructor-Led, Self-Paced

{{< admonition type=abstract title="Target Outcomes" open=true >}}

1. Create an group for the classroom level (even if self-paced).

2. Create an inexpensive, but scalable runner fleet for the class to avoid delays due to shared runner capacity or access.

3. Do so very quickly by leveraging the [GitLab HA Scaling Runner Vending Machine for AWS EC2 ASG](https://gitlab.com/guided-explorations/aws/gitlab-runner-autoscaling-aws-asg).

4. **Not a Target**: Learning the detailed ins and outs of deploying GitLab Runners.
   {{< /admonition >}}

## Configure a New GitLab classgroup

> **Scenarios:** Instructor-Led, Self-Paced



> **IMPORTANT**
>
> - If you are operating on gitlab.com SaaS and have a paid subscription, do this within the licensed group structure.
> - If you are operating on gitlab.com SaaS and have a free user, do this at the root of the site where an Ultimate trial can be enabled with a credit card.
> - Perform this step even if you are doing this self-paced for just yourself.
> - While a runner is not needed just to deploy a GitOps mode application to a cluster, a runner is needed to build the application container in the application project.
> 

1. While in an appropriate top level group, near the top right of the page, *Click* **New subgroup** (button)

2. Name the group with your gitlab classroom name so that it will be unique, easy to remember and easy for others to identify. 

   >  From here on in the exericses this will be referred to as 'classgroup'

3. *Click* **Public**.

4. *Click* **Create Group**.

   {{< admonition type=warning title="Must Be Public" open=true >}}

   Application Build Projects that are used by the GitLab Agent for Kubernetes must be public.

   {{< /admonition >}}

5. Record: 'classgroup' = ___________________________________________________________

   {{< admonition type=warning title="IMPORTANT" open=true >}}
   Throughout the remaining exercises you will replace the text `classgroup` with this actual group name.
   {{< /admonition >}}

6. If this group is a new namespace on GitLab.com SaaS (group at the root), enable the Ultimate trial by:

   1. *Click* **Settings => Billing**
   2. *Click* **Start your free trial** (button)
   3. Complete the company information and *Click* **Continue**
   4. On the Almost there page, for *This subscription is for*, *Select* **[The Group You Just Created]**
   5. For Who will be using GitLab select **My company or team** for groups or **Just me** for individual learning.
   6. *Click* **Start your free trial**.

## Configure an HA Scaled GitLab Runner Hosted in AWS

> **Keyboard Time**: 5 mins, **Automation Wait Time**: 20 mins
>
> **Scenarios:** Instructor-Led, Self-Paced
>
> Guides Through: [GitLab HA Scaling Runner Vending Machine for AWS EC2 ASG](https://gitlab.com/guided-explorations/aws/gitlab-runner-autoscaling-aws-asg/)

{{< admonition type=tip title="Tip" open=true >}}

If you are operating on a GitLab instance and GitLab group where runners are already operational and **usable by all participants** - then this section may be unnecessary. Where it is known to be necessary is if participants accounts are free GitLab.com SaaS accounts and you do not wish to have them each register with a Credit Card to gain access to free runner minutes.

{{< /admonition >}}

{{< admonition type=warning title="Warning" open=true >}}

**IMPORTANT FOR Instructor-Led** - be sure to test labs with a GitLab account configured identically to participants (not your production GItLab account if it has extra permissions or licensing) to be sure participants will have the runner access you intend them to have.

{{< /admonition >}}

{{< admonition type=warning title="Warning" open=true >}}

**IMPORTANT FOR Instructor-Led** - While it may be tempting to use shared runners, deploying your own fleet gives you 100% control over the scale and responsiveness of runners. Since automation waiting time is a big part of this workshop it is a signficant advantage to have control over this part of the resourcing.

{{< /admonition >}}

{{< admonition type=warning title="Warning" open=true >}}

**IMPORTANT FOR Instructor-Led** - setup 1 runner instance per 5 students. This can be easily adjusted later and these are spot instances.

{{< /admonition >}}

1. In 'classgroup', *Click* **CI/CD > Runners**
2. Near the top right, *Click* **Register a group runner** (button)
3. Next to the ‘Registration token’, *Click* **[the Clipboard Icon]**
4. While logged into the AWS Account where your EKS cluster is deployed, [Click this link to deploy the runner to us-east-2](https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/create/review?templateURL=https://gl-public-templates.s3.amazonaws.com/cfn/v1.4.9-alpha14/easybutton-amazon-linux-2-docker-manual-scaling-with-schedule-spotonly.cf.yml&stackName=linux-docker-spotonly) (when us-east-1 may be near account quota limits on any resources) or [Click this link to deploy the runner to us-east-1](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://gl-public-templates.s3.amazonaws.com/cfn/v1.4.9-alpha14/easybutton-amazon-linux-2-docker-manual-scaling-with-schedule-spotonly.cf.yml&stackName=linux-docker-spotonly) (required for AWS “Event Engine” provisioned accounts)

   > If you were not logged into an AWS account you will be prompted to do so.
5. In the upper right of the AWS CloudFormation console, be sure the region selector is set to what you want - *Select* **US East (Ohio) - us-east-2** to match the region of the EKS cluster (if you used a default cluster deployment).
6. Under ‘GitLab Instance URL’ **ensure the correct instance URL is specified** It is simply your GitLab Instance URL with no additional pathing. (it must be contactable on port 443 from the AWS account you are deploying the runner to)
7. Under ‘One or more runner Registration tokens from the target instance’ *Paste* **[the token in your clipboard]**
8. For ‘‘’The number of instances that should be configured. Generally 1 for warm HA and 2 for hot HA' use a number equivalent to 1 runner for every 5 participants. For example, for 1-5 participants use “1”, for 5-10 participants use “2”
9. Near the bottom *Check* both **I acknowledge that AWS CloudFormation might create IAM resources with custom names.** and **I acknowledge that AWS CloudFormation might require the following capability: CAPABILITY_AUTO_EXPAND**
10. *Click* **Create stack** (button)
11. Wait for the stack to complete successfully.
12. In GitLab, in 'classgroup', *Click* **CI/CD > Runners** (Or refresh the page if already there) and you should now see a runner listed for the group.
13. From the left navigation *Click* **Settings > CI/CD** (this is a different location than CI/CD > Runners)
14. To the right of ‘Runners’ *Click* **Expand**
15. Under ‘Enable shared runners for this group’ ensure the toggle button is OFF (default is ON)

{{< admonition type=info title="Info" open=true >}}
The Runner ASG Desired Count and Maximum Count can be editted and updated to scale up the runner fleet if you find things are running slow. Should you choose to scale the runner fleet down, do so by editing the ASG Desire Count so that proper GitLab Runner deregistration processes are triggered.
{{< /admonition >}}

## Add Participants to classgroup

> **Scenarios:** Instructor-Led

{{< admonition type=warning title="Warning" open=true >}}

Participant account provisioning will vary depending on the type of GitLab instance being used for participants. On GitLab.com SaaS, it can be as simple as having participants register for a free account and sending you their GitLab User Name. If in a self-managed GitLab instance or a GitLab.com SaaS company namespace, participants may already have accounts associated with your organization.
This is unnecessary if you are doing the training self-paced for yourself.

{{< /admonition >}}

1. While in  'classgroup', *Click* **Group information > Members**

2. Near the upper right of the page, *Click* **Invite members** (button)

3. For each member start typing their GitLab user name or the email it is registered under and select it.

4. For ‘Select a role’ *Select* **Maintainer**

5. *Click* **Invite**

6. You can return to this Invite page as many times as needed to get everyone added.

{{< admonition type=success title="Accomplished Outcomes" open=true >}}

1. Create an group for the classroom level (even if self-paced).
2. Create an inexpensive, but scalable runner fleet for the class to avoid delays due to shared runner capacity or access.
3. Do so very quickly by leveraging the [GitLab HA Scaling Runner Vending Machine for AWS EC2 ASG](https://gitlab.com/guided-explorations/aws/gitlab-runner-autoscaling-aws-asg).
   {{< /admonition >}}