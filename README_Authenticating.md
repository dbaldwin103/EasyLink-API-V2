Easy-Link V2 - Authenticating
Overview

Prior to sending Rinchem any request, you will first need to prove to Chem-Star that you are an authorized user. This is done by sending your authentication details to a generic authentication endpoint that will then grant you an AccessToken and an InstanceUrl. The AccessToken and the InstanceUrl will provide a secure entry point to the Easy-Link API.

Setting up the Call

To retrieve your Access Token And Instance Url an HTTP 'POST' request needs to be made to your desired endpoint. Depending on the environment that you are developing/testing in, you will be pointing to a different endpoint. If you haven't been explicitly told to use the production endpoint, then you should be using the sandbox. 

Environment	Endpoint
Sandbox	https://test.salesforce.com/services/oauth2/token
Production	https://login.salesforce.com/services/oauth2/token
In addition to the endpoint, you will also need to provide your credentials in the form of URL Parameters. The key -> value mapping for these parameters is provided below. 

Key	Value
grant_type	password
client_id	<your_consumer_key>
client_secret	<your_consumer_secret>
username	<your_username>
password	<your_password><your_security_token>
Your Rinchem contact should have provided you with the proper username, consumer_key, and consumer_secret to use. 

You will be responsible for setting up your_password and resetting your your_security_token. Instructions to reset your security_token may be found here, https://help.salesforce.com/articleView?id=user_security_token.htm&type=5. If you have any issues with your credentials or with resetting your security token, please let your Rinchem contact know.

Putting it Together (HTTP Example)

 

POST /services/oauth2/token?
    grant_type=password&amp;
    client_id=<your_client_id>&amp;
    client_secret=<your_client_secret>&amp;
    username=<your_username>&amp;
    password=<your_password><your_security_token> HTTP/1.1
Host: <your_endpoint>
Cache-Control: no-cache
Content-Type: multipart/form-data;
Note: Everything between POST and Host should be on one line, they have been split apart for readability.

Successful Credentials Response

If the credentials are correct and the call is made successfully, Salesforce will return something along the lines of:

 

{
  "access_token": "00Dn000000093KQ!ARIAQGlBxv3YTjEomig8JcfIXwWjO8eDp_xQDo8trckJK33b.o85iU8bktoPMLfe6gby_o.7bkXoUESjn3qVswvmlzBJD4ek",
  "instance_url": "https://rinchem--CSPortalQA.cs30.my.salesforce.com",
  "id": "https://test.salesforce.com/id/00Dn000000093KQEAY/005n0000002M68dAAC",
  "token_type": "Bearer",
  "issued_at": "1494884052218",
  "signature": "ivZIqu5EdUGmmAkL964t4YEJ164X08IC97ok7yjKmok="
}
From this response, the value of access_token and instance_url should be stored, as they will be needed to make the API Call to Rinchem. 
Your access_token and instance_url will remain valid until your Salesforce session times out. Salesforce timeouts default to 2 hours, though this is subject to change and should not be relied on.

Unsuccessful Credentials Responses

In the event that you have provided an invalid username, password or security token, Salesforce will return something along the lines of:

 

{
  "error": "invalid_grant",
  "error_description": "authentication failure"
}

In the event that you have provided an invalid client_secret, Salesforce will return something along the lines of:

 

{
    "error": "invalid_client",
    "error_description": "invalid client credentials"
}

In the event that you have provided an invalid client_id, Salesforce will return something along the lines of:

 

{
    "error": "invalid_client_id",
    "error_description": "client identifier invalid"
}

In the event that your request just isn't quite set up right, Salesforce will return something along the lines of: 

 

{
    "error": "unsupported_grant_type",
    "error_description": "grant type not supported"
}
This error typically occurs due to the URL parameters being passed in incorrectly.
