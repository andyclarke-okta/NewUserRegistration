# **New User Registration** 


## <span style="text-decoration:underline;">Overview</span>

In CIAM use cases, many business units, locales and brands may require distinct user management operations. Okta Workflows can help implement custom processing of the registration context. 

In this template, the context of a new user registration is processed by Workflows which shall customize the branding, customize the birthright enablement and link to external systems for duplication verification and Preference Management.
The External Check for existing users is implemented with an API stub. In an actual use case a specific HTTP Connector shall be configure along with the needed API Request JSON to match your system requirements. Similarly, the Preference Management 
is implemented with a stub, but can be substituted with an Okta Workflow built in Connector such as DataGrail or OneTrust, as well as , any system that supports API comminication.


## Before you get Started / Prerequisites

Before you get started, here are the things you’ll need:



*   Access to an Okta tenant with Okta Workflows enabled for your org 
*   Email service, in this sample O365 is used. Configure O365 Workflow Connector.
*   Create Okta group named "AcmeUsers"
*   The External API calls in this sample are using a mock API. Create HTTP Connector for mock API Auth = none


## Setup Steps



1. Select  parent flow titled “Create_User_wExtCheck”
    1. Make sure a connection is selected for the instances of the “HTTP Raw Request” cards. This card is used to link to external duplicate user check.
	2. In the Action event card titled “On Demain API Endpoint”  at the far left of the flow, click the “&lt;/>” link at the bottom right of the card.
		1. Check the radio button "Expose as Webhook”
		2. Copy the “Invoke URL” from the top of the popup up to but NOT including “?clientToken=&lt;xxxxxxxx". Save this in a notepad
		3. Copy the “Client Token”. Save this in a notepad.
		4. These values shall be used by the calling party to initiate the operation.
2. Select flow titled "[O365] Send Email HTML".
    1. Selector Connector for "Office 365 Mail" card
3. Select child flow titled “[child] process USA customer”
    1. Make sure a connection is selected for the instances of the “HTTP Raw Request” cards. This card is used to link to external preference management system.
4. Select child flow titled “[child] process CANADA customer”
    1. Make sure a connection is selected for the instances of the “HTTP Raw Request” cards. This card is used to link to external preference management system.	
3. Ensure that all flows are turned on.


## Testing this Flow

The easiest way to test a flow is to send new user registration payload Workflows using POSTMAN



1. Open your POSTMAN console and bring up a new tab
    1. Choose POST action
    2. In the URL section insert the value you copied from the “Invoke URL” config.
    3. In the Headers section add the following key value pairs
        * Accept: application/json
        * Content-Type: application/json
        * x-api-client-token: &lt;value copied from the “Client Token” config>.
    4. In the Body Section add the following payload
        * Choose ‘raw’ data option, insert curly braces for JSON, add the following key value pairs.

```json
{
"email":"test.user1@mailinator.com",
"password":"Password@1",
"firstName":"test",
"lastName":"user1",
"countryCode":"US",
"hasOptedOutTracking":"false",
"hasOptedOutSolicit":"false"
}
```
Note: The POSTMAN collection is also available from the repo

2. Hit the “Send” for the POSTMAN request.
3. View Flow History and verify it completed successfully.
4. Go to the Okta tenant Admin UI-> People
    5. Verify the test user exists with correct Group memberships and profile attributes 


## Limitations & Known Issues



*   None