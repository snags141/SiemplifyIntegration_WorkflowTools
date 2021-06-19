
# SiemplifyIntegration_WorkflowTools
Gain more control over your workflows, with tools such as approval stages that can only be completed by a specified user or group. This can be actioned against a specific alert by moving it into a different case when the workflow requires, or by requiring an approval action against the case as a whole.

## 1 - Why does this integration exist
The purpose of this integration was initially to implement some kind of workflow approval system. In some circumstances, you might want to require approval from one or two specific users or maybe a role, such as tier 3, before a playbook can continue execution.
As of writing this, there is no built-in functionality to achieve these goals, and so Workflow Tools was born.

## 2 - Actions
Typically you'll want to use the "Approval Request" action, followed by "Approval Response" in a playbook. See section 4 for a playbook example.
### 2.1 - Approval Request
Assigns the case to the primary approval manager (As configured in the Integration settings) and optionally posts a message to slack if you provide a Slack Webhook URL in the integration settings.
#### 2.1.1 - Parameters
|Parameter| Type | Description| 
|--|--|--|
|Move Alert If Grouped| Boolean | If set to true and the alert is grouped in a case with one or more other alerts, we'll move the alert to its own case. This can be extremely useful when you want to require approval for only that one alert without affecting the rest of the case. You can then also close the newly created case to prevent other alerts being correlated, while the alert is pending approval. See section 4 for example use cases showing this.|
### 2.2 - Approval Request
Assigns the case to the primary approval manager (As configured in the Integration settings) and optionally posts a message to slack if you provide a Slack Webhook URL in the integration settings.
#### 2.2.1 - Parameters
|Parameter| Type | Description| 
|--|--|--|
|Action Approved| Boolean | A simple yes or no as to whether you'd like to approve the action and allow the workflow to continue |
| Response Comments | String | An optional string with comments that will be added to the case, and optionally sent via the Slack Webhook|
### 2.3 - Assign Case to Current User
This is a simple action that is designed to be run in **manual** mode. Make sure you specify this when using it in a playbook. When placed somewhere in a playbook, it only presents one button: "Execute". When this is clicked, the case is automatically assigned to the user that clicked the button. This is extremely useful when placed at the start of a playbook, as it always makes sure that the user working on the playbook first has the case assigned to themselves.
#### 2.2.1 - Parameters
No parameters, just make sure you have it in manual mode when used in a playbook.

## 3 - Integration Settings
### 3.1 - Parameters
|Parameter| Type | Description| 
|--|--|--|
| Approval Manager | String | The primary approval manager - This can be a username or role name that is allowed to execute the "Approval Response" action. If a role is given, please make sure it starts with an @ for example: @RoleName|
| Secondary Approval Manager | String | An optional approval manager. The same as above but basically a backup/alternative if your primary is not available. May be a good idea to set this to @Administrator, otherwise you can leave it blank. |
| Siemplify API Key | Password String | This is an API key you need to generate in the settings page. The key is used to interact with the local API for things such as: Moving an alert to another case, checking that a user is a member of a role if one is supplied as an approval manager, etc |
| Siemplify Instance Hostname| String | The externally accessible hostname for your Siemplify instance. This is used to generate clickable links in the Slack messages. It might be a hostname like hxxps://soar.mycompany.com or an IP address such as hxxps://192.168.1.10 |
| Siemplify API Root | String | This is the root address to your Siemplify instance's API. I've put it here for future-proofing/advanced architectures. No need to change this unless you know what you're doing and why you're doing it. |
| Slack Webhook URL | String | If you use slack and want to send messages to a channel for auditing/notifications, enter a URL from your Webhook custom integration here. Otherwise, leave it blank. | 

## 4 - Use Case Example


## 3 - Caveats - Things to be aware of
I purposely put the designation of approval managers in the overall integration settings/parameters, rather than the action settings/parameters, to avoid cases where the playbook action (namely "Approval Response") would fail, resulting in any user on the platform with access to that case simply changing the values and clicking "re-run".
When would the action fail?
There are two cases you may see the "Approval Response" action fail with an error. Firstly: Coding error (Unheard of, since I'm a python wizard) and secondly: When a user who **is not** an approval manager attempts to approve or deny the action.
