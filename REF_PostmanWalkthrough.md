# Easy-Link V2 - Postman Walkthrough

Postman is a standalone desktop application (Mac, Windows, or Linux) that advertises itself as "the Only Complete API Development Environment." Primary amongst it's plethora of features, it provides a straightforward interface for the manual setup of HTTP callouts. We'll be taking advantage of this to step through the API process.  

If you find yourself stuck at any point during this tutorial, please don't hesitate to reach out to your Rinchem contact.

### 1. Installing Postman

The first step is going to be installing Postman. The download can be found at [https://www.getpostman.com/apps](https://www.getpostman.com/apps). After you click start the download for your specified operating system, their website will walk you through the rest of the application setup.

After Postman is installed and set up, when you launch the application, you should see something similar to this.

![52155776071](/Resources/1521557760717.png)

### 2. Authenticating

Now that Postman is installed and set up, we can start by connecting to the Chem-Star/Salesforce platform. 

For this first step, we will be making a *POST* call to our test endpoint (https://test.salesforce.com/services/oauth2/token). We will be providing our credentials as *Params*. If you have any questions about what these credentials are, please ask your Rinchem contact.

Prior to submission you should have something that looks like this.

![52156357053](/Resources/1521563570533.png)

At this point, you can hit **Send** and if you're credentials are correct, you should receive a json response back. Something that looks like.

![52156400600](/Resources/1521564006002.png)

If successful we have now proved to Chem-star/Salesforce that we are a legitimate user, and we can now use this response to access the Easy-Link API and its functionality. 

### 3. Setting Up The Easy-Link Call

From the response we are going to use the *instance_url* and the *access_token* to build a new *HTTP* request to send data to Rinchem. Once again we are going to be building a *POST* request. Our endpoint URL is going to be the *instance_url* along with the suffix for the functionality that we want to use. In this particular example we will be sending an outbound order and thus will use the suffix */services/apexrest/v1/outbound*. We will also need to set an *Authorization* **Header** that will house our *access_token*. Overall, our set up should currently look something like this.

![52156467529](/Resources/1521564675295.png)

### 4. Building The Payload

Now that we have the general format set up, we now need to add the actual data that we want to send. This will be done under the **Body** tab. For this example, I'll simply be using the minimum permissible payload, however for a full list of available fields, please see the  [***REF_AllFields.md***](REF_AllFields.md) and/or [***REF_PayloadExamples.md***](REF_PayloadExamples.md) files. 

Our request should now look something like.

![52156629878](/Resources/1521566298783.png)

Then, if we hit send and everything is set up correctly, we'll end up with a response like this.

![52156635746](/Resources/1521566357467.png)

In this case, the *record_name* will be the same as the case that was created by your request. If we log in to the community portal, we will be able to see the new Case, along with the Outbound Order and Line Items that are associated with it.

![52156700847](/Resources/1521567008473.png)

### 5. Celebrate

Congrats you've successfully called the Easy-Link API!
