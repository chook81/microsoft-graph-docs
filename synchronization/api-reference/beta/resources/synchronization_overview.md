# Azure AD Synchronization API Overview

Azure Active Directory (Azure AD) identity synchronization (also called "provisioning") allows you to automate the creation, maintenance, and removal of identities in cloud (SaaS) applications such as Dropbox, Salesforce, ServiceNow, and more. For introductory information on synchronization in Azure AD, see following articles:

* [Automate user provisioning and deprovisioning to SaaS applications with Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-app-provisioning)
*  [Managing user account provisioning for enterprise apps in the Azure portal](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-enterprise-apps-manage-provisioning)

## What is the purpose of the Azure AD Synchronization API? 

Synchronization API, part of Microsfot Graph API, allows programmatic management of identity synchronization. Using the API one can create, start and stop synchronization jobs, make changes to their synchronization schema, and verify current synchronization status. 

## Object model overview

### Synchronization Job

Synchronization job performs synchronization by periodically running in the background, polling for changes in one directory and pushing them to another directory. Synchronization job is always specific to a particular instance of an application in your tenant. As part of the synchronization job setup, generally you would need to give authorization to read/write objects in your target directory, and customize job's synchronization schema to suit your needs. For more information, please see [synchronization job](synchronization_synchronizationjob.md).

### Synchronization Schema

Synchronization schema contains the bulk of a particular synchronization setup. On a high level, it defines what objects will be synchronized and how. Most often you will want to customize some of the attribute mappings to suit your needs, or add a scoping filter to synchronize only objects which satisfy a certain condition. For more information, please see [synchronization schema](synchronization_synchronizationschema.md).

#### Directory Definition

Directory definition provides synchronization engine information about a directory and its objects. Directory definition tells synchronization engine, for example, that directory has objects named "User" and "Group" and what attributes are supported for those objects. In order for the object and attribute to used in [object mappings](synchronization_objectMapping.md), they must to be defined as part of the directory definition. For more information, please see [directory definition](synchronization_directoryDefinition.md).

#### Synchronization Rule

Synchronization rules are at the core of the synchronization setup. They instruct synchronization on how the synchronization should be performed. That includes what objects should be synchronized, how objects from source directory should be matched with objects in target directory, and how attributes should be transformed going from source to target directory. For more information, please see [synchronization rule](synchronization_synchronizationrule.md).

#### Object Mapping

Object mappings are the main part of the synchronization rule. Single object mapping defines how a given object should be synchronized from source directory to target. In particular, it defines how object in source directory should be matched with an object in target directory, what (if any) scoping filters should be used to decide if we want to provision a given object, and how object attributes should be transformed going from source to target directory. For more information, please see [object mapping](synchronization_objectMapping.md).

### Synchronization Template

Synchronization template provides pre-configured synchronization settings for a particular application. These settings will be used by default for any [synchronization job](synchronization_synchronizationjob.md) based on the template.  Template is controlled by the developer of the application, although anyone can retrieve the template to see the default settings, most importantly [synchronization schema](synchronization_synchronizationschema.md). For more information, please see [synchronization template](synchronization_template.md).

## Getting started with the Azure AD Synchronization API

Azure AD Synchronization API is part of Microsoft Graph, and uses the same OAuth 2.0 authorization as MS Graph does. Before making any requests to the API, you would need to obtain an access token. For detailed information on authentication with Microsoft Graph, see [Get access tokens to call Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/auth_overview)

### Using Graph Explorer

One convenient way to get your feet wet with the API is to use [Graph Explorer](https://graph.microsoft.io/en-us/graph-explorer) web application (make sure you start "private" browser session). Graph Explorer will handle authentication for you, so you don't need to worry about access tokens.

* Sign-in with the administrative account for the tenant you will be working with
* Paste requests into Graph Explorer and click 'GO'

### Using arbitrary REST client (Postman, Fiddler, etc)

To request access token, you will need to have the following:

* Tenant Identifier. Unique identifier of the tenant you will be working with
* Administrative user credentials for the same tenant
* Client Application Id (application which is performing  API calls). This application must be registered in Azure Active Directory, have Directory.ReadWrite.All permissions for Microsoft Graph, and must be consented to in the tenant we will be working with.
	- A quick solution is to use well-known application ID of the PowerShell (1950a258-227b-4e31-a9cf-717495945fc2), which is automatically consented to on any tenant.
	- Another way is to register your own application (see [Register an app with the Azure AD v2.0 endpoint](https://graph.microsoft.io/en-us/docs/authorization/auth_register_app_v2.htm))

With this information, we can make a call to obtain access token:
Description	Obtain authorization token for Microsoft Graph, using administrative user credentials. **Make sure all parameter values are URL-encoded**

##### Request
<!-- { "blockType": "ignored" } -->
```http
POST https://login.windows.net/{tenantId}/oauth2/token
Content-Type: application/x-www-form-urlencoded

client_id={applicationClientId}&resource=https%3A%2F%2Fgraph.microsoft.com%2F&grant_type=password&username={userPrincipalName}&password={password}
```

##### Response
<!-- { "blockType": "ignored" } -->
```http
HTTP1.1/OK
{
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV…",
    "refresh_token": "AQABAAAAAABu…"
}
```

All further requests must include access token, i.e:
<!-- { "blockType": "ignored" } -->
```http
GET https://graph.microsoft.com/beta/servicePrincipals
Authorization: Bearer access_token
```

### Finding Service Principal Object(s)

You would need to know ID of the Service Principal object (NOT ServicePrincipal.AppId) when forming requests to Synchronization API.  Here we assume that service principal for your application is already added to the tenant (I.e. by adding application to your tenant in the Azure portal). You can easily find servicePrincipal of interest knowing either ServicePrincipal.AppId, or ServicePrincipal.displayName

#### Find service principals by display name

##### Request
<!-- { "blockType": "ignored" } -->
```http
GET https://graph.microsoft.com/beta/servicePrincipals?$select=id,appId,displayName&$filter=startswith(displayName, 'salesforce')
```

##### Response
<!-- { "blockType": "ignored" } -->
```http
HTTP/1.1 200 OK
{
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
```

#### Find service principals by AppId

##### Request
<!-- { "blockType": "ignored" } -->
```http
GET https://graph.microsoft.com/beta/servicePrincipals?$select=id,appId,displayName&$filter=AppId eq '219561ee-1480-4c67-9aa6-63d861fae3ef'
```

##### Response
<!-- { "blockType": "ignored" } -->
```http
HTTP/1.1 200 OK
{
    "value": [
        {
            "id": "d813d7d7-0f41-4edc-b284-d0dfaf399d15",
            "appId": "219561ee-1480-4c67-9aa6-63d861fae3ef",
            "displayName": "salesforce 3"
        }
    ]
}
```

### Retrieving basic job information

#### List existing synchronization jobs

##### Request
<!-- { "blockType": "ignored" } -->
```http
GET https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs
GET https://graph.microsoft.com/beta/servicePrincipals/60443998-8cf7-4e61-b05c-a53b658cb5e1/synchronization/jobs
```

##### Response
<!-- { "blockType": "ignored" } -->
```http
HTTP/1.1 200 OK
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
            "status": {}
        }
    ]
}
```

#### Retrieve job status

##### Request
<!-- { "blockType": "ignored" } -->
```http
GET https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs/{jobId}

GET https://graph.microsoft.com/beta/servicePrincipals/60443998-8cf7-4e61-b05c-a53b658cb5e1/synchronization/jobs/SfSandboxOutDelta.e4bbf44533ea4eabb17027f3a92e92aa
```

##### Response
<!-- { "blockType": "ignored" } -->
```http
    HTTP/1.1 200 OK
    {
        "id": "SfSandboxOutDelta.e4bbf44533ea4eabb17027f3a92e92aa",
        "templateId": "SfSandboxOutDelta",
        "schedule": {
            "expiration": null,
            "interval": "PT20M",
            "state": "Active"
        },
        "status": {..}
    }
```

#### Retrieve effective schema

##### Request
<!-- { "blockType": "ignored" } -->
```http
GET https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs/{jobId}/schema
```

##### Response
<!-- { "blockType": "ignored" } -->
```http
HTTP/1.1 200 OK
{
    "directories": [],
    "synchronizationRules": []
}
```


## See also

* [Synchronization schema](../resources/synchronization_synchronizationschema.md)
* [Configure synchronization with directory extension attributes](../resources/synchronization_howto_directory_extensions.md)
* [Configure synchronization with custom target attributes](../resources/synchronization_howto_custom_attributes.md)
* [Get access tokens to call Microsoft Graph](https://graph.microsoft.io/en-us/docs/authorization/auth_overview.htm)
* [Register an app with the Azure AD v2.0 endpoint](https://graph.microsoft.io/en-us/docs/authorization/auth_register_app_v2.htm)


