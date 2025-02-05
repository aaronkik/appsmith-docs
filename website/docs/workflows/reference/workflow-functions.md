---
description: This page provides detailed information on workflow functions available in Appsmith.
title: Workflow Functions
hide_title: true
toc_min_heading_level: 2
toc_max_heading_level: 3
---
<!-- vale off -->

<div className="tag-wrapper">
 <h1>Workflow Functions</h1>

<Tags
tags={[
{ name: "Business", link: "https://www.appsmith.com/pricing", additionalClass: "business" }
]}
/>

</div>

<!-- vale on -->

Workflow functions are in-built framework functions that enables you to interact with different entities like datasources, queries, external systems, and human-in-the-loop interactions. This page provides information about the workflow functions available in Appsmith, including their signatures, parameters, and usage examples.

## executeWorkflow()

The `executeWorkflow()` function serves as a central control unit for executing workflows and marks the starting point of the workflow execution within Appsmith. This function allows you to create a workflow logic for execution of tasks.

#### Example

    ```javascript
    export default {
        async executeWorkflow(data) {
            //call a query and read values of parameters
            const response = await qs_send_email.run({ "email": data.email });
            // Log the response for debugging
            console.log(response);
        
            return true;
        }
    }
    ```

### Signature

```javascript
executeWorkflow(data: JSON): Promise<boolean>
```
### Parameters

Below are the parameters required by the `executeWorkflow()` function to execute:

#### data <code className="parameterCodeBlock">JSON</code>

<dd>
  The parameter `data` holds the data passed from your App to trigger and process the workflow. For example, consider the following data passed to the workflow where `data` holds the body of the post request when triggered via webhook, and holds `Trigger Data` property of the [Trigger Workflow](/workflows/reference/workflow-queries#trigger-workflow) query when workflow is executed from an Appsmith app.

    ```javascript
      {
        "userId": 123,
        "action": "updateProfile",
        "data": {
          "firstName": "John",
          "lastName": "Doe",
          "email": "john.doe@example.com"
        }
      }

    ```
  In your workflow, you can access properties within the `data` object like `userId` using dot notation. To access the `userId`, use `data.userId`.
</dd>

### Return type

The `executeWorkflow()` returns a Promise that resolves to a boolean value, either `true` or `false`, indicating the success or failure of the workflow execution.

## assignRequest()

The `assignRequest()` function is an asynchronous function that is part of the `workflows` object within the global `appsmith` object in Appsmith. It allows you to create a decision point in a workflow that requires user intervention. This decision point is created as a pending request in the workflow, which can be accessed later in your app to enable users to take action using the [Get requests](/workflows/reference/workflow-queries#get-requests) workflow query. Once a pending request is created, the workflow pauses and awaits user action. 

#### Example

```javascript
async function handleUserRequest() {
  try {
    await appsmith.workflows.assignRequest({
      requestName: "Approval",
      resolutions: ["Approved", "Rejected"],
      requestToUsers: ["user1", "user2"]
    });
    console.log("Request successfully assigned and workflow paused.");
  } catch (error) {
    console.error("An error occurred while assigning the request:", error);
  }
}

```

### Signature

```javascript
assignRequest({requestName: string, resolutions: string[], requestToUsers: string[], requestToGroups: string[], message: string,  metadata:{key: string, value: any}}) : Promise<JSON>
```

### Parameters
 
Below are the parameters required by the `assignRequest()` function to execute:

#### requestName <code className="parameterCodeBlock">String</code>

    <dd>
    The name of the request, which serves as its identifier within the workflow. Give a unique name to the request, and use it to filter requests as part of [Get requests](/workflows/reference/workflow-queries#get-requests) workflow query by adding it in the `Request name` attribute.
    </dd>

#### resolutions <code className="parameterCodeBlock">String[]</code>
    <dd>
    Represents the possible actions a user can take on the request. The applicable resolution passed to the [Resolve requests](/workflows/reference/workflow-queries#resolve-requests) workflow query to apply the selected resolution. For example, `['Approve', 'Reject']`.
    </dd>

#### requestToUsers <code className="parameterCodeBlock">String[]</code> <code className="parameterCodeBlock">Optional</code>
   <dd>
   The `requestToUsers` parameter allows for targeted assignment of requests to specific Appsmith users. It specifies an array of emails for users to whom the pending requests will be assigned. The users specified here will be able to take action and resolve the pending request. These users need to be a part of the Appsmith instance for assigning the request to them. It's mandatory to supply either `requestToUsers` or `requestToGroups` atribute for request assignment. The workflow resumes upon the first action taken by any user within the assigned users.
   </dd>

#### requestToGroups <code className="parameterCodeBlock">String[]</code> <code className="parameterCodeBlock">Optional</code>

<dd>
The `requestToGroups` parameter allows for targeted assignment of requests to specific User groups in Appsmith. If specifies the user group names to which the request will be assigned for resolution. When specified, the request will be assigned to all the users belonging to the groups. It's mandatory to supply either `requestToUsers` or `requestToGroups` atribute for request assignment. The workflow resumes upon the first action taken by any user within the assigned users.
 </dd>

#### message <code className="parameterCodeBlock">String</code> <code className="parameterCodeBlock">Optional</code>
    <dd>
      A descriptive message associated with the request, providing more context for users. For example, when creating a refund request, you might include a message like "Refund request raised by User 1".
    </dd>

#### metadata <code className="parameterCodeBlock">JSON</code> <code className="parameterCodeBlock">Optional</code>
    <dd>
    Add data that may be needed to process the request or display more information to the user in your app. For example, you can include a unique identifier for the record associated with the request. Use the identifier in your app to fetch and show the details to user.
    </dd>

### Return type 

The `assignRequest()` function returns a Promise in a JSON format representing the generated response. The response includes the following data:

#### workflowInstanceId <code className="parameterCodeBlock">String</code>

<dd>
  Represents the unique identifier for the workflow instance.
</dd>

#### resolution <code className="parameterCodeBlock">String</code>

<dd>
  Indicates the resolution applied to the request based on the user action.
</dd>

#### payload <code className="parameterCodeBlock">JSON</code>

<dd>
  It holds the data from the `metadata` attribute used during the processing of the [Resolve Requests](/workflows/reference/workflow-queries#resolve-requests) workflow query.
</dd>
 
 ## See also

* [Debug Workflow](/workflows/how-to-guides/debug-workflow) - Learn to debug workflows as you build them.