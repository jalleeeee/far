---
title: Send a weekly report on project status from the Milestones sample app template (contains video)
description: Learn how to extend the Milestones sample app for Microsoft Teams to send a weekly summary of the status of work items for a project.

ms.topic: conceptual
ms.custom: 
ms.date: 06/25/2021
author: joel-lindstrom
ms.subservice: teams
ms.author: saperlmu
ms.reviewer: mkaur
contributors:
  - msftsamperl
  - joel-lindstrom
  - mduelae
---

# Send weekly report on project status from the Milestones sample app

The Milestones sample app template allows users to create milestones for a team and track their progress using work items and work item status. The app can be extended further to send weekly emails to project managers or person creating the project with a list of all the work items and their status.

In this article, we'll learn how to create a Power Automate flow to send a project weekly report to the person who created the project with the work items listed along with their status.

Watch this video to learn how to send weekly report on project status from Milestones app: https://www.microsoft.com/videoplayer/embed/RWLiBU

## Prerequisites

To complete this exercise, we'd need the ability to log in into Teams that will be available as part of select Microsoft 365 subscriptions, and will also need to have the Milestones sample app for Teams installed. This app can be installed using the [installation guide](../../INSTALLATION.md).

## Create the Power Automate flow

1. Sign in to [Power Automate](https://flow.microsoft.com).

1. Select the environment that has the same name as the Team in which the app is installed.

1. Select **+Create** to create a new flow.

1. Select **Start from blank – Automated cloud flow**.

1. Select **Skip** to go to the next screen.

1. Update the flow title, and select **Recurrence** as the flow trigger.

    ![Set flow title and recurrence trigger.](https://github.com/microsoft/teams-powerapps-app-templates/blob/main/Milestones/Documentation/media/extend-milestones-weekly-report/flow-recurrence.png "Set flow title and recurrence trigger")

1. Update the **Recurring** trigger values as shown below.

    ![Flow recurrence step.](https://github.com/microsoft/teams-powerapps-app-templates/blob/main/Milestones/Documentation/media/extend-milestones-weekly-report/recurrence-step.png "R")

1. Add the following steps to the flow.

1. List rows steps – renamed to List Projects.

    - Table Name – "Projects"

    - Expand Query – "createdby"

    ![List projects step.](https://github.com/microsoft/teams-powerapps-app-templates/blob/main/Milestones/Documentation/media/extend-milestones-weekly-report/list-projects.png "List projects step")

1. List rows – renamed to List Work Items.

    - Table Name – "Project Work Items"

    - Filter Rows – "\_msft_project_id_value eq @{items('Apply_to_each_2')?['msft_projectid']}"

    - Expand Query – "msft_WorkItemStatus_id(\$select=msft_name)"

> [NOTE]
> As soon as we select the Project ID in the Filter Rows field, the flow would put the List Work Items step into an Apply to Each container in which we would add more steps that would be needed.

1. Create HTML table – renamed to Create HTML table – Work Items.

    - From – the value from the previous step –  "@{outputs('List_Work_Items')?['body/value']}"

    - Columns – "Custom"

    - Add the columns to the table:

        | Header | Value |
        | - | - |
        | WorkItemName | Name from the List Work Items step |
        | WorkItemStatus | Work Item Status from the List Work Items step |

    ![Create an HTML table.](https://github.com/microsoft/teams-powerapps-app-templates/blob/main/Milestones/Documentation/media/extend-milestones-weekly-report/create-html-table.png "Create an HTML table")

1. Send an email (v2).

    - To: "Created by Primary Email from the List Projects step."

    - Subject: "Project Work Item Status for [Name](Name from the List Projects step)."

    - Body: Output from the Create HTML table step.

    ![Send email step.](https://github.com/microsoft/teams-powerapps-app-templates/blob/main/Milestones/Documentation/media/extend-milestones-weekly-report/send-email.png "Send email step")

1. Save the flow.

1. Select the Test button on the top right and select manually.

## Data set up before testing the Power Automate Flow

1. Before testing the flow, we need to make sure we have some data with projects and work items within each project in the Milestones app.

1. Open Teams and run the Milestones app.

1. Create a new project.

1. Create Work Items under that project.

## Test the Power Automate Flow

1. Go back to https://powerautomate.microsoft.com and open the flow.

1. Select Test on the top right.

    ![Test the flow.](https://github.com/microsoft/teams-powerapps-app-templates/blob/main/Milestones/Documentation/media/extend-milestones-weekly-report/test-flow.png "Test the flow")

1. Run the flow.

1. Check inbox for emails with weekly status – We should see one or more emails in the inbox for each Project.

    ![Email status report.](https://github.com/microsoft/teams-powerapps-app-templates/blob/main/Milestones/Documentation/media/extend-milestones-weekly-report/status-report.png "Email status report")

### See also

- [Deploy Milestones as broad distribution app](milestones-broad-distribution.md)
- [Customize Milestones sample app template](customize-milestones.md)
- [Understand Milestones sample app template architecture](milestones-architecture.md)
- [Customize sample app templates](https://learn.microsoft.com/en-us/power-apps/teams/customize-sample-apps)
- [Frequently Asked Questions (FAQs) for sample app templates](https://learn.microsoft.com/en-us/power-apps/teams/sample-apps-faqs)
