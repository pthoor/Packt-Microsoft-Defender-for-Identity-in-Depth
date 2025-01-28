# Chapter 5: Extending MDI Capabilities Through APIs

## Integrating MDI healthIssues with monitoring tools
This solution uses Logic Apps with a system-assigned managed identity, authorized via the `SecurityIdentitiesHealth.Read.All` Graph API permission, to efficiently monitor and 
manage MDI health issues. You can deploy this solution through the GitHub repository for this chapter by clicking the Deploy to Azure button. Here's how the integration works:
- Data collection and condition logic:
   - System-assigned managed identity enabled with the following permissions:
   - Storage Table Data Contributor (on resource group)
   - Monitoring Metrics Publisher (on resource group)
   - `SecurityIdentitiesHealth.Read.All` (Graph API)
   - The Logic App uses an HTTP action connector to fetch MDI health issues from the Graph API
   - It then checks Azure Table for an existing entry matching the issue ID
   - Using conditional logic, the Logic App determines whether the issue is new (requiring creation) or has changed status (requiring an update)

- Data ingestion and storage:
   - If the issue is new or updated, the Logic App sends the JSON payload to a Log Analytics workspace. This is done via another HTTP action that connects to the Data Collection Endpoint (DCE), referencing a predefined Data Collection Rule (DCR) that dictates the schema for the `MDIHealthIssues_CL` custom table.
   - Subsequently, the issue details are either updated or newly created in Azure Table Storage using the **Insert Or Replace Entity** connector.

- Visualization and alerting:
   - Administrators can set up alerts in Azure Monitor to notify them when new health issues are detected
   - Azure Workbooks can be utilized to visualize the data, providing a clear and interactive display of health issue trends and specifics

![5 6](https://github.com/user-attachments/assets/ecb69ee8-0a46-45cb-98c4-b56cb8c4409f)

### Installation

To install the solution for **Integrating MDI healthIssues with monitoring tools**, follow these steps:

1. Press the Deploy to Azure button and sign in to Azure with an account that has appropriate permissions to create new resources.

    [![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FPacktPublishing%2FMicrosoft-Defender-for-Identity-in-Depth%2Fmain%2FChapter05%2Fmain.json)

2. Verify that the deployment was successful and that you can see all of the resources in your resource group.
