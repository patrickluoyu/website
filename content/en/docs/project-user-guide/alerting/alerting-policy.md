---
title: "Alerting Policies (Workload Level)"
keywords: 'KubeSphere, Kubernetes, Workload, Alerting, Policy, Notification'
description: 'Learn how to set alerting policies for workloads.'
linkTitle: "Alerting Policies (Workload Level)"
weight: 10710
---

KubeSphere provides alerting policies for nodes and workloads. This tutorial demonstrates how to create alerting policies for workloads in a project. See [Alerting Policy (Node Level)](../../../cluster-administration/cluster-wide-alerting-and-notification/alerting-policy/) to learn how to configure alerting policies for nodes.

## Prerequisites

- You have enabled [KubeSphere Alerting](../../../pluggable-components/alerting/).
- To receive alert notifications, you must configure a [notification channel](../../../cluster-administration/platform-settings/notification-management/configure-email/) beforehand.
- You need to create a workspace, a project and an account (`project-regular`). The account must be invited to the project with the role of `operator`. For more information, see [Create Workspaces, Projects, Accounts and Roles](../../../quick-start/create-workspace-and-project/).
- You have workloads in this project. If they are not ready, see [Deploy and Access Bookinfo](../../../quick-start/deploy-bookinfo-to-k8s/) to create a sample app.

## Create an Alerting Policy

1. Log in to the console as `project-regular` and go to your project. Navigate to **Alerting Policies** under **Monitoring & Alerting**, then click **Create**.

2. In the dialog that appears, provide the basic information as follows. Click **Next** to continue.

   - **Name**. A concise and clear name as its unique identifier, such as `alert-demo`.
   - **Alias**. Help you distinguish alerting policies better.
   - **Description**. A brief introduction to the alerting policy.
   - **Duration (Minutes)**. An alert will be firing when the conditions defined for an alerting policy are met at any given point in the time range.
   - **Severity**. Allowed values include **Warning**, **Error** and **Critical**, providing an indication of how serious an alert is.

3. On the **Alerting Rule** tab, you can use the rule template or create a custom rule. To use the template, fill in the following fields.

   - **Resource Type**. Select the resource type you want to monitor, such as **Deployment**, **StatefulSet** and **DaemonSet**.
   - **Monitoring Target**. Depending on the resource type you select, the target can be different. You cannot see any target if you do not have any workload in the project.
   - **Alerting Rules**. Define a rule for the alerting policy. These rules are based on Prometheus expressions and an alert will be triggered when conditions are met. You can monitor objects such as CPU and memory.

   ![rule-template](/images/docs/project-user-guide/alerting/alerting-policies/rule-template.png)

   {{< notice note >}}

   You can create a custom rule with PromQL by entering an expression in the **Monitoring Metrics** field (autocompletion supported). For more information, see [Querying Prometheus](https://prometheus.io/docs/prometheus/latest/querying/basics/). 

   {{</ notice >}} 

   Click **Next** to continue.

4. On the **Notification Settings** tab, enter the alert summary and message to be included in your notification, then click **Create**.

5. An alerting policy will be **Inactive** when just created. If conditions in the rule expression are met, it will reach **Pending** first, then turn to **Firing** if conditions keep to be met in the given time range.

## Edit an Alerting Policy

To edit an alerting policy after it is created, on the **Alerting Policies** page, click <img src="/images/docs/project-user-guide/alerting/alerting-policies/edit-alerting-policy.png" height="20px"> on the right.

1. Click **Edit** from the drop-down menu and edit the alerting policy following the same steps as you create it. Click **Update** on the **Notification Settings** page to save it.

   ![alert-policy-created](/images/docs/project-user-guide/alerting/alerting-policies/alert-policy-created.png)

2. Click **Delete** from the drop-down menu to delete an alerting policy.

## View an Alerting Policy

Click an alerting policy on the **Alerting Policies** page to see its detail information, including alerting rules and alerting messages. You can also see the rule expression which is based on the template you use when creating the alerting policy.

Under **Monitoring**, the **Alert Monitoring** chart shows the actual usage or amount of resources over time. **Notification Settings** displays the customized message you set in notifications.

![alerting-policy-detail](/images/docs/project-user-guide/alerting/alerting-policies/alerting-policy-detail.png)