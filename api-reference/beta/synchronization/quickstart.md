# Synchronization API Quickstart

Resources

	API 2.0 Spec
	Walk-through: Synchronize Custom Target Attributes
	Walk-through: Synchronize Directory Extension Attributes

## Authorization

Provisioning API is part of Microsoft Graph, and uses the same OAuth 2.0 authorization as MS Graph does. Before making any requests to the API, you would need to obtain an access token.

### Using Graph Explorer

One way to bypass the hassle with authentication is to use Graph Explorer web application (make sure you start "private" browser session with Graph Explorer):

- PPE: https://graphexplorer2-ppe.azurewebsites.net/#, 
- PROD: https://graph.microsoft.io/en-us/graph-explorer
	 
This is convenient when you want to run a few simple requests. Graph Explorer will handle authentication for you, so you don't need to worry about access tokens. 

- Sign-in with the administrative account for the tenant you will be working with
- Paste requests into Graph Explorer and click 'GO'
- Note however, Graph Explorer has limitation of not supporting HTTP PUT method.

### Using arbitrary REST client (Postman, Fiddler, etc)

To request access token, you will need to have the following:
- Tenant Identifier. Unique identifier of the tenant you will be working with
- Administrative user credentials for the same tenant
- Client Application Id (application which is performing  API calls). This application must be registered in Azure Active Directory, have Directory.ReadWrite.All permissions for Microsoft Graph, and must be consented to in the tenant we will be working with. 
-- A quick solution is to use well-known application ID of the PowerShell (1950a258-227b-4e31-a9cf-717495945fc2, both in PROD and PPE), which is automatically consented to on any tenant.
-- Another way is to register your own application (see Registering an application for API access at the bottom of this page)
		
With this information, we can make a call to obtain access token:
Description	Obtain authorization token for Microsoft Graph, using administrative user credentials. 
	Make sure all parameter values are URL-encoded
Request (PROD)	POST https://login.windows.net/{tenantId}/oauth2/token
	Content-Type: application/x-www-form-urlencoded
	
	client_id={applicationClientId}&resource=https%3A%2F%2Fgraph.microsoft.com%2F&grant_type=password&username={userPrincipalName}&password={password}
Request (PPE)	POST https://login.windows-ppe.net/{tenantId}/oauth2/token
	Content-Type: application/x-www-form-urlencoded
	
	client_id={applicationClientId}&resource=https%3A%2F%2Fgraph.microsoft-ppe.com%2F&grant_type=password&username={userPrincipalName}&password={password}
Response	{
	  "token_type": "Bearer",
	   ...
	  "access_token": "eyJ0eXAiOiJKV…",
	  "refresh_token": "AQABAAAAAABu…"
	}
	

All further requests must include access token, i.e:

	GET https://graph.microsoft.com/beta/servicePrincipals
	Authorization: Bearer access_token


Provisioning API versions and environments

IMPORTANT: 
When doing operations with common directory objects, such as Service Principals, we need call MS Graph's public beta version API
(i.e. GET _https://graph.microsoft.com/beta/servicePrincipals?$select=id,appId,displayName&$filter=startswith(displayName, 'salesforce')

When doing operations on provisioning-specific objects,  we need to use provisioning-specific API version:
(i.e. GET https://graph.microsoft-ppe.com/testSynchronization/servicePrincipals/{id}/synchronization/jobs)

PPE
For PPE, use following root URL:  https://graph.microsoft-ppe.com/testSynchronization/servicePrincipals/{id}/synchronization/jobs)


Finding Service Principal Object(s)

You would need to know ID of the Service Principal object (NOT ServicePrincipal.AppId) when forming requests to Provisioning API.  Here we assume that service principal for your application is already added to the tenant (I.e. by adding application to your tenant in the Azure portal). You can easily find servicePrincipal of interest knowing either ServicePrincipal.AppId, or ServicePrincipal.displayName

Description	Find service principals by display name
Request	GET https://graph.microsoft.com/beta/servicePrincipals?$select=id,appId,displayName&$filter=startswith(displayName, 'salesforce')
Response	{
	     "value": [
	        {
	            "id": "bc0dc311-87df-48ac-91b1-259bd2c3a31c",
	            "appId": "f7808c5e-cb57-4e37-8094-406d302c0f8d",
	            "displayName": "Salesforce"
	        },
	        {
	            "id": "d813d7d7-0f41-4edc-b284-d0dfaf399d15",
	            "appId": "219561ee-1480-4c67-9aa6-63d861fae3ef",
	            "displayName": "salesforce 3"
	        }
	    ]
	 }

Description	Find service principals by AppId
Request	GET https://graph.microsoft.com/beta/servicePrincipals?$select=id,appId,displayName&$filter=AppId eq '219561ee-1480-4c67-9aa6-63d861fae3ef'
Response	{
	    "value": [
	        {
	            "id": "d813d7d7-0f41-4edc-b284-d0dfaf399d15",
	            "appId": "219561ee-1480-4c67-9aa6-63d861fae3ef",
	            "displayName": "salesforce 3"
	        }
	    ]
	}


Retrieving basic job information

List existing synchronization jobs
Request	GET https://graph.microsoft.com/testSynchronization/servicePrincipals/{id}/synchronization/jobs
	
	GET https://graph.microsoft.com/testSynchronization/servicePrincipals/60443998-8cf7-4e61-b05c-a53b658cb5e1/synchronization/jobs
Response	HTTP/1.1 200 OK
	{
	    "value": [
	        {           
	              "id": "SfSandboxOutDelta.e4bbf44533ea4eabb17027f3a92e92aa",
	            "templateId": "SfSandboxOutDelta",
	            "schedule": {
	                "expiration": null,
	                "interval": "PT20M",
	                "state": "Active"
	            },
	            "status": {.. }
	        }
	    ]
	}

Retrieve a job (and its current status)
Request	GET https://graph.microsoft.com/testSynchronization/servicePrincipals/{id}/synchronization/jobs/{jobId}
	
	GET https://graph.microsoft.com/testSynchronization/servicePrincipals/60443998-8cf7-4e61-b05c-a53b658cb5e1/synchronization/jobs/SfSandboxOutDelta.e4bbf44533ea4eabb17027f3a92e92aa
Response	HTTP/1.1 200 OK
	        {            
	            "id": "SfSandboxOutDelta.e4bbf44533ea4eabb17027f3a92e92aa",
	            "templateId": "SfSandboxOutDelta",
	            "schedule": {
	                "expiration": null,
	                "interval": "PT20M",
	                "state": "Active"
	            },
	            "status": {.. }
	        }
	


Retrieve effective schema
Request	GET https://graph.microsoft.com/testSynchronization/servicePrincipals/{id}/synchronization/jobs/{jobId}/schema
Response	HTTP/1.1 200 OK
	{
	    "directories": [..],
	    "synchronizationRules": [..]
	}



Registering your own application for API access

	Resources
	
	Register an app with the Azure AD v2.0 endpoint
	App authentication with Microsoft Graph
	https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-compare
	
	Register the application
	
		1. Sign into the App Registration Portal using administrative account in your tenant. This must be the tenant you  want to make API calls against
		2. Choose Add an app. Enter a name for the app, and choose Create application
			i. 
		3. Under Platforms, click Add Platform. Choose  Mobile. Web Platform with default Redirect URI will be added automatically
			i. 
		4. Under Microsoft Graph Permissions, add Directory.ReadWrite.All to Delegated Permissions. If you plan the application to authenticate as a service, add the same under Application Permission
			i. 
		5. Choose "Save"
		6. Copy Client Id (which is the same as Application Id) and Redirect URI (default value of "urn:ietf:wg:oauth:2.0:oob")
		7. Compose consent URL:
		https://login.windows.net/{tenantId}/oauth2/authorize
		?response_type=code&redirect_uri=http%3A%2F%2Flocalhost&prompt=admin_consent&client_id={applicationId}
		&resource=https%3A%2F%2Fgraph.microsoft.com
		8. Paste the URL in browser. Login with the administrator credentials and consent to the application. You should get redirected to http://localhost/X address, which will be not found and show error in the browser - ignore that. 
		9. You should be ready to go ahead with the API calls
	
