---
title: "Orchestrate - Workflow"
chapter: true
---

## Workflows to orchestrate your ETL

A workflow is a container for a set of related jobs, crawlers, and triggers in AWS Glue. Using a workflow, you can design a complex multi-job extract, transform, and load (ETL) activity that AWS Glue can execute and track as single entity.

In AWS Glue, you can use workflows to create and visualize complex extract, transform, and load (ETL) activities involving multiple crawlers, jobs, and triggers. Each workflow manages the execution and monitoring of all its components. As a workflow runs each component, it records execution progress and status, providing you with an overview of the larger task and the details of each step. The AWS Glue console provides a visual representation of a workflow as a graph.

Event triggers within workflows can be fired by both jobs or crawlers, and can start both jobs and crawlers. Thus you can create large chains of interdependent jobs and crawlers.

### Static and Dynamic Workflow Views

For each workflow, there is the notion of static view and dynamic view. The static view indicates the design of the workflow. The dynamic view is a run time view that includes the latest run information for each of the jobs and crawlers. Run information includes success status and error details.

When a workflow is running, the console displays the dynamic view, graphically indicating the jobs that have completed and that are yet to be run. You can also retrieve a dynamic view of a running workflow using the AWS Glue API.

### Workflow Restrictions

Keep the following workflow restrictions in mind:
    
* A trigger can be associated with only one workflow.
* Only one starting trigger (on-demand or schedule) is permitted.
* If a job or crawler in a workflow is started by a trigger that is outside the workflow, any triggers inside the workflow that depend on job or crawler completion (succeeded or otherwise) do not fire.
* Similarly, if a job or crawler in a workflow has triggers that depend on job or crawler completion (succeeded or otherwise) both within the workflow and outside the workflow, if the job or crawler is started from within a workflow, only the triggers inside the workflow fire upon job or crawler completion.

### Creating and Building Out a Workflow Using the Console

**Step 1: Create the workflow** 

1. Sign in to the AWS Management Console and open the AWS Glue console at https://console.aws.amazon.com/glue/.
2. In the navigation pane, under ETL, choose Workflows.
3. Choose Add workflow and complete the Add a new ETL workflow form.

![sparkjob](/image/glueworkflow.png)

4. Choose Add workflow.
    The new workflow appears in the list on the Workflows page.

![sparkjob](/image/glueworkflow1.png)

![sparkjob](/image/glueworkflow2.png)

**Step 2: Add a start trigger**

1. On the **Workflows** page, select your new workflow. In the tabs at the bottom, choose **Graph**.
2. Choose Add trigger, and in the Add trigger dialog box, do one of the following:

![sparkjob](/image/glueworkflow3.png)

*  Choose **Add new**, and complete the Add trigger form, selecting **Schedule** or On demand for **Trigger Type**. Then choose **Add**.

![sparkjob](/image/glueworkflow4.png)

*   The trigger appears on the graph, along with a placeholder node (labeled **Add node**). At this point, the trigger is not yet saved.

![sparkjob](/image/glueworkflow5.png)

* Choose the placeholder node (**Add node**) and go to Crawlers section. Select Crawler which you have created in **Glue Data Catalog section**

![sparkjob](/image/glueworkflow6.png)
![sparkjob](/image/glueworkflow7.png)

**Step 3: Add New Triggers**

Click on Crawler which you have added in previous section; it will show option to **Add trigger** or else go to Action and select "Add trigger"

![sparkjob](/image/glueworkflow8.png)

Provide **Trigger Name** and select **Start after ALL watched event**

![sparkjob](/image/glueworkflow9.png)

![sparkjob](/image/glueworkflow10.png)

Slect **Add Node** and go to Job section and select job which you have created in Glue ETL section and click on **Add**

![sparkjob](/image/glueworkflow11.png)

![sparkjob](/image/glueworkflow12.png)


## Running a Workflow

If the start trigger for a workflow is an on-demand trigger, you can start the workflow from the AWS Glue console.

### To run a workflow

1. Open the AWS Glue console at https://console.aws.amazon.com/glue/.
2. In the navigation pane, under ETL, choose Workflows.
3. Select a workflow. On the Actions menu, choose Run

![sparkjob](/image/glueworkflow13.png)



