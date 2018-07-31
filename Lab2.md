### Task 1: Provision a Function App

1.  In the Azure portal, select **+Create a resource**, enter "function" into the search box, select **Function App** in the results, and select **Create**. 

2.  On the Function App Create blade, enter the following:

    a.  **App name**: Enter a unique name

    b.  **Subscription**: Select the subscription you are using for this hands-on lab

    c.  **Resource group**: select the hands-on-lab resource group created in Lab1 or create new

    d.  **OS**: Select Windows

    e.  **Hosting Plan**: Select Consumption Plan

    f.  **Location**: Select the location you are using for resources in this hands-on lab

    g.  **Storage**: Choose Use existing or create new select the storage account

    h.  **Application Insights**: Select Off
   
3.  Select **Create**.

### Task 2 : Sign Up for Dark Sky https://darksky.net/dev/register
    a. Copy the sample API call 
### Task 3: Create an Azure Functions Proxy

In this task, you will create an Azure Function Proxy, which is a simple way to provide a clean API endpoint. To learn more, check out [Working with Azure Functions Proxies](https://docs.microsoft.com/en-us/azure/azure-functions/functions-proxies).

1.  Navigate to your Function App in the Azure portal.

2.  On the Function Apps blade, select **+** next to **Proxies** under the Function App.

3.  In the New proxy form, enter the following values:

    a.  **Name**: Weather

    b.  **Route template**: Enter "/"

    c.  **Allowed HTTP methods**: Select Selected Methods and check GET

    d.  **Backend URL**: Paste the Dark Sky  URL https://api.darksky.net/forecast/yourkey/37.8267,-122.4233
    

4.  Select **Create**.

5.  When it is done creating the Proxy, copy the Proxy URL, and paste it into a new browser tab or window.

6.  This will result in the weather for Los_Angeles.

### Task 4: Parameterize Azure Functions Proxy

In the previous task, you created an Azure Functions Proxy to get weather for Los_Angeles. In this task, you will update the Proxy to parameterize the URL, so you can retrieve weather for other places.

1.  Return to the Weather Proxy in your Function App in the Azure portal.

2.  Update the Route template and Backend URL fields with the following values:

    a.  **Route template**: Change to "/{latitude},{latitude}"

    b.  **Backend URL**: Change to "https://api.darksky.net/forecast/yourkey/{latitude},{longitude}"

    c.  Select **Save**
    
    
3.  Copy the new Proxy URL and paste it into a new browser tab or window, replacing the parameters in the URL with a policy holder's last name and policy number. For example:

    -  {latitude}: 37.8267

    -  {latitude}: -122.4233

    - <https://yourfunctionapp.azurewebsites.net/37.8267,-122.4233>

4.  You can try this out to check weather for other places.

