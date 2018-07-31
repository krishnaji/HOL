### Task 1: Provision Azure API Management

In this task, you will create a new API Management Resource.

1.  Connect to the Azure portal at: <https://portal.azure.com>, select +Create a resource, **Integration** and then select **API management**
   
2.  On the **API management** blade, specify the following configuration and select **Create**:

    a.  **Name**: Enter a unique value, such as contosoinsuranceSUFFIX (ensure the green checkmark appears)

    b.  **Subscription**: Select the subscription you are using for this hands-on lab.

    c.  **Resource Group**: Select the resource group you are using for this hands-on lab.

    d.  **Organization Name**: Contoso Insurance

    e.  **Administrator Email**: Enter the email address associated with the Azure account you are using for this hands-on lab.

    f.  **Pricing Tier**: Develper (No SLA)
      > **Note**: It will take several minutes to provision the APIM resource. You can safely move on to the next task.

### Task 2 : Add a Service Bus
1.  Create a new service Bus at https://ms.portal.azure.com/#create/Microsoft.ServiceBus

2. **Create** and Congigure Service Bus Namespace

    a. **Name**: Unique name <service-name.servicebus.windows.net>

    b. **Pricing Tier**: Select Pricing tier

    c. **Subscription**: Select the subscription you are using for this hands-on lab.

    d. **Resource Group**: Select the resource group you are using for this hands-on lab.

    e. **Location**: Select the location you are using for resources in this hands-on lab

3. **Create** Service Bus Queue 

    a. Add **Name**
    
    b. Select **Max Queue Size** as 1GB

    c. leave everythig else as default and **Create**

### Task 3: Create request-triggered Logic App 
1.  Create a new Logic App at https://ms.portal.azure.com/#create/Microsoft.EmptyWorkflow

2. Specify the configuration

    a. **Name**: Logic App Name

    c. **Subscription**: Select the subscription you are using for this hands-on lab.

    d. **Resource Group**: Select the resource group you are using for this hands-on lab.

    e. **Location**: Select the location you are using for resources in this hands-on lab

    f. **Log Analytics** : Off

    g. **Create**

3. Open the Logic App once it has been provisioned

    a. In the Logic App Designer, scroll through the page until you located the Start with a common trigger section. Select the **HTTP Request-Response** template

    b. Open **When a http request is received** and update the **Update the JSON Body schema** with following
        ` {
    "properties": {
        "CustomerId": {
            "type": "string"
        },
        "ItemId": {
            "type": "string"
        },
        "Quantity": {
            "type": "string"
        }
    },
    "type": "object"
}
`

    c. Click **+** to insert new step , Add **Action** , **Select Service Bus** and finally select **Send Message**

    d. Specify a **Connection Name** , Select *Namespace* and *Service Bus Policy* , **Create**

    e. Select **Queue Name** , Content as **"@{base64(triggerBody())}"**


### Task 4: Import the Logic App to API Management(APIM)

In this task, you will import your Logic app to the APIM's api collection.

1.  Return to the **API Management** service and  select the **APIs** blade.

2.  Select **Logc app** to begin importing your Policy Docs api into APIM.

3.  Browse **Logic App (Please select Function App)** and choose your the logic app you created.

4.  On the **Logic App** blade, specify the following configuration and select **Create**:

    a.  **Display Name**: Subimit Order

    b. **API URL suffix**

    b.  **Products**: Unlimited

5. Test the newly created API: Add following **Request body** and **Send** 
     ` {
          "CustomerId": "1",
          "ItemId": "2",
          "Quantity": "3"
      } `

6. Verify if the Logic App Succeeded and message Added to servicebus Queue

### Task 5: Exercise Logic App  that

        a. Triggers on new messages in Service Bus Queue ( Hint: User Events in Service Bus)
        b. Get messages from Queue ( Hint: Use Servie Bus Step)
        c. Process each message and complte the message in Queue(Hint: Use For Each , Parse JSON , Servie Bus- Complete Messages)
        d. Send email (Hint: Use Outlook connector)
