# Qualys KB V2
**Author: Sreedhar Ande**

This Function App calls the Qualys Vulnerability Management (VM) - KnowledgeBase (KB) API (https://www.qualys.com/docs/qualys-api-vmpc-user-guide.pdf) to pull vulnerability data from the Qualys KB.

**V2.0**
1. User can select date for first ingestion - From date to pull vulnerability data from the Qualys KB

2. User can change Azure function trigerring schedule

3. Store Qualys API Password and Log Analytics Workspace Primary/Secondary Key in Azure KeyVault

## Configuration Steps to Deploy Function App
1. Click on Deploy to Azure (For both Commercial & Azure GOV)  
[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsreedharande%2FQualysKBV2%2Fmain%2Fazuredeploy_QualysKB_API_FunctionApp.json)
  

2. Select the preferred **Subscription**, **Resource Group** and **Location**  
   **Note**  
   Best practice : Create new Resource Group while deploying - all the resources of your custom Data connector will reside in the newly created Resource 
   Group
   
3. Enter parameters value in the ARM template while deployment


## Post Deployment Steps
1. Qualys API Password and Log Analytics Workspace Key will be placed as "Secrets" in the Azure KeyVault `<<Function App Name>><<uniqueid>>` with only Azure Function access policy. If you want to see/update these secrets,

	```
		a. Go to Azure KeyVault `<<Function App Name>><<uniqueid>>`
		b. Click on "Access Policies" under Settings
		c. Click on "Add Access Policy"
			i. Configure from template : Secret Management
			ii. Key Permissions : GET, LIST, SET
			iii. Select Prinicpal : <<Your Account>>
			iv. Add
		d. Click "Save"

	```

2. The `TimerTrigger` makes it incredibly easy to have your functions executed on a schedule. This sample demonstrates a simple use case of calling your function based on your schedule provided while deploying. If you want to change
   the schedule 
   ```
   a.	Click on Function App "Configuration" under Settings 
   b.	Click on "Schedule" under "Application Settings"
   c.	Update your own schedule using cron expression.
   ```
   **Note: For a `TimerTrigger` to work, you provide a schedule in the form of a [cron expression](https://en.wikipedia.org/wiki/Cron#CRON_expression)(See the link for full details). A cron expression is a string with 6 separate expressions which represent a given schedule via patterns. The pattern we use to represent every 10 minutes is `0 */10 * * * *`. This, in plain text, means: "When seconds is equal to 0, minutes is divisible by 10, for any hour, day of the month, month, day of the week, or year".**


