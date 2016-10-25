# Getting Started

Welcome to cloud.ca's API documentation.

The cloud.ca API allows you to manage your configuration as well as provision and manage your resources in a simple programmatic way using standard HTTP requests.

This API is following the [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) approach. All requests should be made over SSL. Request and Response bodies, including errors, are encoded in JSON.



## Authentication

> To authenticate, use this code:

```shell
curl "api_endpoint_here"
  -H "MC-Api-Key: [your-api-key]"
```

> Make sure to replace `[your-api-key]` with your API key.

Authentication is done via the API Key which you can find in the API Keys section in the cloud.ca web interface under the user profile menu. If you don't see cloud.ca API section, it means your user account doesn't have the required **API keys for cloud.ca** permission.

cloud.ca expects for the API key to be included in all API requests to the server in a header that looks like the following:

`MC-Api-Key: [your-api-key]`

<aside class="notice">
You must replace <code>"[your-api-key]</code> with your personal cloud.ca API key.
</aside>

## Authorization
All cloud.ca API calls go through the same RBAC (Role-based Access Control) logic that is enforced when performing the equivalent operation from the cloud.ca portal. Your user account associated with the provided API key need to be assigned the required permission(s) via your system and/or environment roles in order to be authorized to execute the various API calls. If you lack the required permission, the API call will fail and return an error message indicating what went wrong.

## Requests
The cloud.ca API can be used by any tool that is fluent in HTTP. The appropriate HTTP method should be used depending on the desired action.

Method | Purpose
------ | ------- 
GET | Used to retrieve information about a resource.
PUT | Used to create (or provision) a new resource. 
POST | Used to update a resource or perform an operation on it. 
DELETE | Used to remove/delete a resource. 

POST and PUT requests must have a JSON encoded body and the `Content-Type: application/json` header.

<aside class="notice">
Remember to url-encode all querystring parameters!
</aside>

### Paging & Sorting

```shell
curl -X GET "https://api.cloud.ca:443/v1/users;orderby=tenant,userName" \
  -H "MC-Api-Key: [your-api-key]"
```

```shell
curl -X GET "https://api.cloud.ca:443/v1/users;orderby=lastLogin%20DESC" \
  -H "MC-Current-Page: 1" \
  -H "MC-Page-Size: 10" \
  -H "MC-Api-Key: [your-api-key]"\

```

#### Configuration APIs
When retrieving a list of configuration items, **paging** is controlled by adding the following headers to your request:

HTTP request header | Description
------------------- | ----------- 
MC-Current-Page | The page of data to retrieve
MC-Page-Size | The number of items to display per page


To control **sorting** when retrieve a list of configuration items, you can also add the `orderby` **path** parameter to specified the field(s) to be used for sorting, along with the desired sorting order.

## Responses

> A successful request to return a single ressource will look like this:

```json
{
  "data": {
    "_comment" : " JSON representation of resource goes here"
  }
}
```

> A successful request to return a collection of resource will look like this:

```json
{
  "data": [
    { "_comment" : "JSON representation of first resource goes here" },
    { "_comment" : "JSON representation of second resource goes here" }
  ],
  "metadata": {
    "pageSize": 2,
    "pageCurrent": 1,
    "recordCount": 4,
    "sortField": "templateName",
    "sortOrder": "ASC"
  }
}
```

> An unsuccessful request will look like this:

```json
{
  "taskId": "d59b760c-dfa9-449f-be81-0f9efcf2946c",
  "taskStatus": "FAILED",
  "errors": [
    {
      "code": 2012,
      "message": "Cannot stop an instance that isn't in the running state",
      "context": {
        "id": "4534cc36-bc46-48bc-ac5c-3ee4e42f0a44",
        "currentState": "Stopped",
        "expectedStates": [
          "Running"
        ],
        "type": "instances"
      }
    }
  ]
}
```

When an API request is issued, the response body will be formatted in the same standard JSON structure (see detailed [JSON Schema](http://json-schema.org/) of the response in the sidebar), although some of the object and attributes may or may not be included depending on the context.

The returned HTTP Status will be one of the following codes:

Status code | Reason
----------- | ------- 
200 | The request was successful.
400 | Bad request -- Occurs when invalid parameters are provided or when quota limit is exceeded.
404 | Forbidden -- You are not authorized to perform this request.
404 | Not Found -- Cannot locate the specified resource.
500 | An unexpected error occured.

## Asynchronous operations

Some operations take longer to execute, and to avoid blocking on the response until it is fully completed, these are treated in an asynchronous fashion. This means the API will return immediately, and provide you a `taskId` that is your reference to the ongoing background task. Using the [Tasks](#tasks) API, you can query the task's status to find if it has completed and obtain the result of the operation.

The operations that are affected by this are identified as such with a ***(async)*** suffix in this documentation.

<aside class="warning">
It is a good practice to limit the polling rate on the task API to no more than once per second.
</aside>



