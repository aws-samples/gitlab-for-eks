---
title: "EKS Stop, Start & Scheduling"
weight: 91
chapter: true
draft: false
description: "Stopping all EKS Cluster nodes manually or by scheduling stop and start."
---

## EKS Stop, Start & Scheduling

> **Scenarios:** Instructor-Led, Self-Paced

**Important**: If you scale the bastion ASG to 0, you will need to rerun the configuration above to have kubectl, helm and docker available while logged in via SSM (ssm-user).

**Important**: The hourly cost of the EKS control plane ($0.10/hr or $73/month in us-east-1) still applies when all instances are scaled to zero.

### Manually Turning Off Cluster Nodes and Bastion Host

1. In the [EC2 AutoScaling Console for us-east-2](https://us-east-2.console.aws.amazon.com/ec2autoscaling/home?region=us-east-2#/details) click the Auto Scaling group that starts with **spot2azuseast2** and also contains **NodeGroupStack**.
2. From the 'Details' tab (default location), under 'Group details' pane, _Click_ **Edit** (button)
3. For 'Desired capacity' _Type_ **0**
4. For 'Minimum capacity' _Type_ **0**
5. _Click_ **Update**
6. Since the Bastion host is also in an ASG (for self-healing), simply repeat the above steps for the Auto Scaling group that starts with **spot2azuseast2** and also contains **BastionStack**.

### Manually Turning On Cluster Nodes and Bastion Host

Repeat the same steps in **Manually Turning Off Cluster Nodes and Bastion Host** but for the NodeGroupStack, for 'Desired capacity' and 'Minimum capacity' specify **2** and for these values for the BastionStack specify **1**.

### Scheduling Cluster and Bastion Host Availability

1. Edit the NodeGroupStack ASG and select the **Automatic scaling** tab.
2. Under the 'Scheduled actions' pane _Click_ **Create scheduled action** and specify a schedule for daily shutdown.
3. Create another 'Scheduled action' for daily startup. If you use a cron expression you can also craft a schedule only for weekdays so that the cluster does not come up on weekends.
4. For the bastion host ASG you can consider creating only a daily shutdown schedule and then simply manually start it when needed because once the cluster is integrated with GitLab the need for the cluster may be quite low.
5. Sample cron strings for ASG schedule parameter (without the quotes)
   1. In the ASG Scheduled Actions Wizard, under 'Recurrence', *Select* **Cron** to change the Wizard UI to receive a cron expression.
      - "**0 23 * * ***" = Stop at 11pm UTC (6pm ET), Every Evening (ensures nightly shutdown for cost control whether manually or automatically started) This could be done with non-cron regular scheduling.
      - "**0 11 * * MON-FRI**" = Start at 11am UTC (6am ET), Monday-Friday. (Optional automated weekday startup, otherwise leave this out to support only manual startup). Targeting only weekdays is easier with cron than regular scheduling.
   2. Other Cron examples can be devised here: https://crontab.guru/examples.html

## Configure Work Around For ssm-user in sudoers

{{% notice info %}}
SSM was recently changed so that the ssm-user is not automatically in the sudoers file. This work around fixes that problem.
{{% /notice %}}

> This should no longer be necessary since we do not need admin to run kubectl and helm and do not need to install docker for kubernetes agent registration.

1. [Click here to open the ssm send-command console in us-east-2](https://us-west-2.console.aws.amazon.com/systems-manager/run-command/send-command?region=us-east-2)
2. Immediately change regions if you are operating your EKS cluster in another region.
3. In the search parameter *Paste* `AWS-RunShellScript` and *Press* **Enter**
4. In the results list below the search prompt, *Select* **AWS-RunShellScript**
5. After Command parameters appears, under Commands, *Paste* `echo "ssm-user ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/ssm-agent-users`
6. Under Targets, *Click* **Choose instances manually**
7. After the Instances list appears, choose the one whose name is **EKSBastion**
8. At the bottom right of the screen *Click* **Run** (button)
9. Refresh the Command status until you observe Success for Overall status

{{% notice tip %}}
You have just deployed an EKS cluster ready to integrate with GitLab.
{{% /notice %}}
## Configure Built-in EKS Bastion Utilities

{{% notice warning %}}
These steps should no longer be necessary. If kubectl and/or helm are not available on the path with ssm-user, this may fix the situation.
{{% /notice %}}

1. In the EC2 Instances console, *locate* the instance named **EKSBastion**

2. *Right click* **the instance**, *select* => **Connect**  => **Session Manager** => **Connect** (button)

3. *Paste* `sudo su`

   > This is how you will always connect to the bastion host in the future.

4. Test that the cluster administration commands are available by using these comments:

   1. `helm version` should produce a string with a bunch of versions. (Warnings about an insecure .kube/config are ok)
   2. `kubectl get nodes --all-namespaces` should produce a list of two nodes.
   3. If either test command fails, paste the following commands:

      ```
      mkdir -p /home/ssm-user/.kube/
      sudo cp /home/ec2-user/.kube/config /home/ssm-user/.kube/config
      sudo chown -R ssm-user:ssm-user /home/ssm-user/.kube/
      sudo ln -s /usr/local/bin/kubectl /opt/aws/bin
      sudo ln -s /usr/local/bin/helm /opt/aws/bin
      ```