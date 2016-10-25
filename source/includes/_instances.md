# Instances

## Get list of instances

```shell
curl -X GET "https://api.cloud.ca:443/v1/services/compute-east/demo-env/instances"
  -H "MC-Api-Key: [your-api-key]"
```
> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "zoneId": "04afdbd1-e32d-4999-86d0-96703736dded",
      "templateId": "8f52a74e-e637-40e8-a8dc-f56fd0b71ab9",
      "templateName": "CentOS 7 HVM base (64bit)",
      "computeOfferingId": "3caab5ed-b5a2-4d8a-82e4-51c46168ee6c",
      "zoneName": "QC-1",
      "computeOfferingName": "1vCPU.512MB",
      "networkId": "d5a68379-a9ee-404f-9492-a1964b374d6f",
      "networkName": "Web-Testing",
      "vpcId": "9eb1592c-f92f-4ddd-9799-b58caf896328",
      "vpcName": "Testing-VPC",
      "ipAddress": "10.164.212.68",
      "projectId": "a295c6de-1737-4df2-aa49-9bd749fa2489",
      "isPasswordEnabled": true,
      "macAddress": "02:00:2b:67:00:30",
      "cpuCount": 1,
      "memoryInMB": 512,
      "hostname": "backup-test",
      "username": "cca-user",
      "affinityGroupIds": [],
      "id": "9db8ff2f-b49b-466d-a2f3-c1e6def408f4",
      "name": "backup-test",
      "state": "Running"
    },
    {
      "zoneId": "04afdbd1-e32d-4999-86d0-96703736dded",
      "templateId": "b6b0506a-4454-4f93-951c-8c9d220a466c",
      "templateName": "CentOS 6.6 base (64bit)",
      "computeOfferingId": "3caab5ed-b5a2-4d8a-82e4-51c46168ee6c",
      "zoneName": "QC-1",
      "computeOfferingName": "1vCPU.512MB",
      "networkId": "d5a68379-a9ee-404f-9492-a1964b374d6f",
      "networkName": "Web-Testing",
      "vpcId": "9eb1592c-f92f-4ddd-9799-b58caf896328",
      "vpcName": "Testing-VPC",
      "ipAddress": "10.164.212.56",
      "projectId": "a295c6de-1737-4df2-aa49-9bd749fa2489",
      "isPasswordEnabled": true,
      "macAddress": "02:00:6b:c5:00:2e",
      "cpuCount": 1,
      "memoryInMB": 512,
      "hostname": "i-john-664",
      "username": "cca-user",
      "affinityGroupIds": [],
      "id": "06f969bf-5893-4548-860e-14416e70c16d",
      "name": "i-john-664",
      "state": "Stopped"
    }
  ],
  "metadata": {
    "recordCount": 2
  }
}
```


This endpoint retrieves all instances in a given environment.

### HTTP Request

`GET https://api.cloud.ca:443/v1/services/{service_code}/{env_name}/instances`

### Parameters

Parameter | Type | Required | Description
--------- | ---- | ------- | -----------
service_code | path | true | Service code of an environment.
env_name | path | true | Environment name.
org_id | query | false | Organization id (not required if environment is in your own organization).
sort_by | query | false | Name of field to sort on.
sort_order | query | false | Sort order `ASC` or `DESC`
page_number | query | false | The page number to retrieve
page_size | query | false | Number of items per page

## Add new instance (async)

> Here is the absolute minimum information required to create a new instance:

```shell
curl -X POST -H "Content-Type: application/json" -H "MC-Api-Key: [your-api-key]" -d "{
  \"name\" : \"myInstance2\",
  \"templateId\" : \"15601ee5-3db8-4021-9872-e5248a7f885a\",
  \"computeOfferingId\": \"e213fb17-ab2e-45ff-9679-e30f905f35a2\",
  \"networkId\" : \"d5a68379-a9ee-404f-9492-a1964b374d6f\"
}" "https://api.cloud.ca:443/v1/services/compute-east/testing/instances"
```
> The above command returns JSON structured like this:

```json
{
  "taskId": "b2f82e2a-123e-4f86-a4c7-dc9b850dd11e",
  "taskStatus": "PENDING"
}
```

> After querying the [Tasks](#tasks) endpoint with above `taskID`, a successful execution will return a result like this:

```json
{
   "data":{
      "id":"b2f82e2a-123e-4f86-a4c7-dc9b850dd11e",
      "status":"SUCCESS",
      "created":"2016-07-12T09:37:34.955-04:00",
      "result":{
         "cpuCount":1,
         "memoryInMB":1024,
         "affinityGroupIds":[

         ],
         "networkId":"d5a68379-a9ee-404f-9492-a1964b374d6f",
         "state":"Running",
         "hostname":"myInstance",
         "isPasswordEnabled":true,
         "projectId":"a295c6de-1737-4df2-aa49-9bd749fa2489",
         "macAddress":"02:00:01:16:00:33",
         "type":"CloudStack",
         "password":"fJub6i",
         "zoneName":"QC-1",
         "zoneId":"04afdbd1-e32d-4999-86d0-96703736dded",
         "id":"5bf7352c-eed2-43dc-83f1-89917fb893ca",
         "templateId":"15601ee5-3db8-4021-9872-e5248a7f885a",
         "computeOfferingId":"e213fb17-ab2e-45ff-9679-e30f905f35a2",
         "computeOfferingName":"1vCPU.1GB",
         "name":"myInstance",
         "networkName":"Web-Testing",
         "templateName":"CentOS 7.2 HVM base (64bit)",
         "ipAddress":"10.164.212.242"
      }
   }
}
```


This endpoint create a new instances in a given environment.

### HTTP Request

`POST https://api.cloud.ca:443/v1/services/{service_code}/{env_name}/instances`

### Parameters

Parameter | Type | Required | Description
--------- | ---- | ------- | -----------
service_code | path | true | Service code of an environment.
env_name | path | true | Environment name.
org_id | query | false | Organization id (not required if environment is in your own organization).
instance | body | true | The JSON-formatted instance object to be created


## Delete an instance (async)

```shell
curl -X DELETE "https://api.cloud.ca:443/v1/services/compute-east/demo-env/instances/5bf7352c-eed2-43dc-83f1-89917fb893ca" \
  -H "MC-Api-Key: [your-api-key]"
```
> The above command returns JSON structured like this:

```json
{  
   "taskId":"07be5466-dd81-4965-929e-e72cfa35046f",
   "taskStatus":"PENDING"
}
```

> Schema for `cleanup_options` parameter:

```json
{
   "purgeImmediately":true,
   "publicIpIdsToRelease":[
      "string"
   ],
   "volumeIdsToDelete":[
      "string"
   ],
   "deleteSnapshots":true
}
```

This endpoint deletes an instance. The instance needs to be in running, stopped or error state for the operation to work.

### HTTP Request

`DELETE https://api.cloud.ca:443/v1/services/{service_code}/{env_name}/instances/{id}`

### Parameters

Parameter | Type | Required | Description
--------- | ---- | ------- | -----------
service_code | path | true | Service code of an environment.
env_name | path | true | Environment name.
id | path | true | Id of instance.
org_id | query | false | Organization id (not required if environment is in your own organization).
cleanup_options | body | false | JSON-formatted cleanup options



