# Core Resources

The following sections describe the various endpoints exposed by cloud.ca to manage customer's configuration and interact with various core functionality of the system. Using these, you can automate various workflows without having to log into the portal, or simply integrate differents aspects of cloud.ca with your own toolchain.

# Environments

# Users

# Tasks

## Get task status

> A response for an asynchronous operation will look like this:

```json
{
  "taskId": "668abbfd-d5f2-45d4-95a3-28aef4b671f5",
  "taskStatus": "PENDING"
}
```

> To verify if the operation is completed:

```shell
curl "https://api.cloud.ca:443/v1/tasks/668abbfd-d5f2-45d4-95a3-28aef4b671f5" \
  -H "MC-Api-Key: [your-api-key]"
```

> The response when the task is completed (if creating a new resource, a representation of the resource would also be included):

```json
{
  "data": {
    "id": "668abbfd-d5f2-45d4-95a3-28aef4b671f5",
    "status": "SUCCESS",
    "created": "2016-07-11T16:25:15.227-04:00"
  }
}
```

This endpoint retrieves information about an asynchronous task

### HTTP Request

`GET https://api.cloud.ca:443/v1/tasks/{id}`

### Parameters

Parameter | Type | Required | Description
--------- | ---- | ------- | -----------
id | path | true | Id for the task to find.


# Usage
