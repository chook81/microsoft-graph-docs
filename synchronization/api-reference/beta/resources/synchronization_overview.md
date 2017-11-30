# Azure AD synchronization API overview

Azure Active Directory (Azure AD) identity synchronization (also called "provisioning") allows you to automate the creation, maintenance, and removal of identities in cloud (software as a service, or SaaS) applications such as Dropbox, Salesforce, ServiceNow, and more. You can use the synchronization APIs in Microsoft Graph to manage identity synchronization programmatically, including:

- Create, start, and stop synchronization jobs
- Make changes to the synchronization schema for jobs
- Verify the current synchronization status 

For more information about synchronization in Azure AD, see:

* [Automate user provisioning and deprovisioning to SaaS applications with Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-app-provisioning)
* [Managing user account provisioning for enterprise apps in the Azure portal](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-enterprise-apps-manage-provisioning)

## Authorization

The Azure AD synchronization API uses OAuth 2.0 for authorization. Before making any requests to the API, you need to get an access token. For more information, see [Get access tokens to call Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/auth_overview). For information about the permissions your app needs to access synchronization resources, see [Directory permissions](../../../concepts/permissions_reference.md#directory-permissions).

### Requesting an access token

To request an access token, you need to have the following:

* The tenant identifier (a unique identifier for the tenant you will be working with).
* Administrative user credentials for the tenant.
* The client app ID for the application that is calling the API. 

    >**Note:** The application must be registered in Azure AD, have Directory.ReadWrite.All permissions for Microsoft Graph, and must be consented to in the tenant. To do this, you can:

	>- Use well-known PowerShell application ID (1950a258-227b-4e31-a9cf-717495945fc2), which is automatically consented to on any tenant.
	>- Register your own application. For more information, see [Register an app with the Azure AD v2.0 endpoint](../../../concepts/auth_register_app_v2.md).


The following example shows how to make the call to get an access token. Make sure that all parameter values are URL-encoded.

**Request**
<!-- { "blockType": "ignored" } -->
```http
POST https://login.windows.net/{tenantId}/oauth2/token
Content-Type: application/x-www-form-urlencoded

client_id={applicationClientId}&resource=https%3A%2F%2Fgraph.microsoft.com%2F&grant_type=password&username={userPrincipalName}&password={password}
```

**Response**
<!-- { "blockType": "ignored" } -->
```http
HTTP1.1/OK
{
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV…",
    "refresh_token": "AQABAAAAAABu…"
}
```

All subsequent requests must include the access token, as shown in the following example.

<!-- { "blockType": "ignored" } -->
```http
GET https://graph.microsoft.com/beta/servicePrincipals
Authorization: Bearer access_token
```

## Synchronization job

Synchronization jobs perform synchronization by periodically running in the background, polling for changes in one directory, and pushing them to another directory. The synchronization job is always specific to a particular instance of an application in your tenant. As part of the synchronization job setup, you need to give authorization to read and write objects in your target directory, and customize the job's synchronization schema.

For more information, see [synchronization job](synchronization_synchronizationjob.md).

## Synchronization schema

The synchronization schema defines what objects will be synchronized and how they will be synchronized. The synchronization schema contains most of the setup information for a particular synchronization job. Typically, you will customize some of the [attribute mappings](synchronization_attributemapping.md), or add a [scoping filter](synchronization_filter.md) to synchronize only objects that satisfy a certain condition.

The synchronization schema includes the following components:

- Directory definitions
- Synchronization rules
- Object mappings

For more information, see [synchronization schema](synchronization_synchronizationschema.md).

## Synchronization template

The synchronization template provides pre-configured synchronization settings for a particular application. These settings will be used by default for any [synchronization job](synchronization_synchronizationjob.md) that is based on the template. The application developer specifies the template; anyone can retrieve the template to see the default settings, including the [synchronization schema](synchronization_synchronizationschema.md).

For more information, see [synchronization template](synchronization_template.md).

## Find service principal objects

To make requests to the synchronization API, you need to know the ID of the service principal object. This example assumes that the you have already added the service principal for your application to the tenant (by adding the application to your tenant in the Azure portal). You can find the service principal object by either display name or app ID.

### Find service principal object by display name

The following example shows how to find service principal object by display name.

**Request** 

<!-- { "blockType": "ignored" } -->
```http
GET https://graph.microsoft.com/beta/servicePrincipals?$select=id,appId,displayName&$filter=startswith(displayName, 'salesforce')
```

**Response**

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

### Find service principal object by app ID

The following example shows how to find the service principal object by app ID.

**Request** 
<!-- { "blockType": "ignored" } -->
```http
GET https://graph.microsoft.com/beta/servicePrincipals?$select=id,appId,displayName&$filter=AppId eq '219561ee-1480-4c67-9aa6-63d861fae3ef'
```

**Response**
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

## List existing synchronization jobs

The following example shows you how to list existing synchronization jobs.

**Request**
<!-- { "blockType": "ignored" } -->
```http
GET https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs
GET https://graph.microsoft.com/beta/servicePrincipals/60443998-8cf7-4e61-b05c-a53b658cb5e1/synchronization/jobs
```

**Response**
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

## Get synchronization job status
The following example shows you how to get the status of a synchronization job.

**Request**
<!-- { "blockType": "ignored" } -->
```http
GET https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs/{jobId}

GET https://graph.microsoft.com/beta/servicePrincipals/60443998-8cf7-4e61-b05c-a53b658cb5e1/synchronization/jobs/SfSandboxOutDelta.e4bbf44533ea4eabb17027f3a92e92aa
```

**Response**
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

## Get synchronization schema
The following example shows you how to get the synchronization schema.

**Request**
<!-- { "blockType": "ignored" } -->
```http
GET https://graph.microsoft.com/beta/servicePrincipals/{id}/synchronization/jobs/{jobId}/schema
```

**Response**
<!-- { "blockType": "ignored" } -->
```http
HTTP/1.1 200 OK
{
    "directories": [],
    "synchronizationRules": []
}
```

## Next steps

Try the API in the [Graph Explorer](https://developer.microsoft.com/en-us/graph/graph-explorer). Graph Explorer will handle authentication for you, so you don't need to worry about access tokens.

## See also

* [Configure synchronization with directory extension attributes](../resources/synchronization_howto_directory_extensions.md)
* [Configure synchronization with custom target attributes](../resources/synchronization_howto_custom_attributes.md)



