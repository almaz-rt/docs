---
editable: false
sourcePath: en/_api-ref-grpc/serverless/eventrouter/v1/eventrouter/api-ref/grpc/Rule/list.md
---

# EventRouter Service, gRPC: RuleService.List {#List}

Retrieves the list of rules in the specified folder.

## gRPC request

**rpc List ([ListRulesRequest](#yandex.cloud.serverless.eventrouter.v1.ListRulesRequest)) returns ([ListRulesResponse](#yandex.cloud.serverless.eventrouter.v1.ListRulesResponse))**

## ListRulesRequest {#yandex.cloud.serverless.eventrouter.v1.ListRulesRequest}

```json
{
  // Includes only one of the fields `busId`, `folderId`
  "busId": "string",
  "folderId": "string",
  // end of the list of possible fields
  "pageSize": "int64",
  "pageToken": "string",
  "filter": "string"
}
```

#|
||Field | Description ||
|| busId | **string**

ID of the bus to list rules in.

Includes only one of the fields `busId`, `folderId`.

ID of the folder on bus to list rules in. ||
|| folderId | **string**

ID of the folder to list rules in.

Includes only one of the fields `busId`, `folderId`.

ID of the folder on bus to list rules in. ||
|| pageSize | **int64**

The maximum number of results per response. ||
|| pageToken | **string**

Page token. To get the next page of results, set `page_token` to the
`next_page_token` returned by a previous list request. ||
|| filter | **string**

Supported fields for filter:
name
created_at ||
|#

## ListRulesResponse {#yandex.cloud.serverless.eventrouter.v1.ListRulesResponse}

```json
{
  "rules": [
    {
      "id": "string",
      "busId": "string",
      "folderId": "string",
      "cloudId": "string",
      "createdAt": "google.protobuf.Timestamp",
      "name": "string",
      "description": "string",
      "labels": "string",
      "filter": {
        // Includes only one of the fields `jqFilter`
        "jqFilter": "string"
        // end of the list of possible fields
      },
      "targets": [
        {
          // Includes only one of the fields `yds`, `ymq`, `function`, `container`, `gatewayWsBroadcast`, `logging`, `workflow`
          "yds": {
            "database": "string",
            "streamName": "string",
            "serviceAccountId": "string"
          },
          "ymq": {
            "queueArn": "string",
            "serviceAccountId": "string"
          },
          "function": {
            "functionId": "string",
            "functionTag": "string",
            "serviceAccountId": "string",
            "batchSettings": {
              "maxCount": "int64",
              "maxBytes": "int64",
              "cutoff": "google.protobuf.Duration"
            }
          },
          "container": {
            "containerId": "string",
            "containerRevisionId": "string",
            "path": "string",
            "serviceAccountId": "string",
            "batchSettings": {
              "maxCount": "int64",
              "maxBytes": "int64",
              "cutoff": "google.protobuf.Duration"
            }
          },
          "gatewayWsBroadcast": {
            "gatewayId": "string",
            "path": "string",
            "serviceAccountId": "string",
            "batchSettings": {
              "maxCount": "int64",
              "maxBytes": "int64",
              "cutoff": "google.protobuf.Duration"
            }
          },
          "logging": {
            // Includes only one of the fields `logGroupId`, `folderId`
            "logGroupId": "string",
            "folderId": "string",
            // end of the list of possible fields
            "serviceAccountId": "string"
          },
          "workflow": {
            "workflowId": "string",
            "serviceAccountId": "string",
            "batchSettings": {
              "maxCount": "int64",
              "maxBytes": "int64",
              "cutoff": "google.protobuf.Duration"
            }
          },
          // end of the list of possible fields
          "transformer": {
            // Includes only one of the fields `jqTransformer`
            "jqTransformer": "string"
            // end of the list of possible fields
          },
          "retrySettings": {
            "retryAttempts": "int64",
            "maximumAge": "google.protobuf.Duration"
          },
          // Includes only one of the fields `deadLetterQueue`
          "deadLetterQueue": {
            "queueArn": "string",
            "serviceAccountId": "string"
          },
          // end of the list of possible fields
          "status": "Status"
        }
      ],
      "deletionProtection": "bool",
      "status": "Status"
    }
  ],
  "nextPageToken": "string"
}
```

#|
||Field | Description ||
|| rules[] | **[Rule](#yandex.cloud.serverless.eventrouter.v1.Rule)**

List of rules. ||
|| nextPageToken | **string**

Token for getting the next page of the list of rules. ||
|#

## Rule {#yandex.cloud.serverless.eventrouter.v1.Rule}

#|
||Field | Description ||
|| id | **string**

ID of the rule. ||
|| busId | **string**

ID of the bus that the rule belongs to. ||
|| folderId | **string**

ID of the folder that the rule resides in. ||
|| cloudId | **string**

ID of the cloud that the rule resides in. ||
|| createdAt | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**

Creation timestamp. ||
|| name | **string**

Name of the rule. ||
|| description | **string**

Description of the rule. ||
|| labels | **string**

Resource labels as `key:value` pairs. ||
|| filter | **[Filter](#yandex.cloud.serverless.eventrouter.v1.Filter)**

Filter for the rule. ||
|| targets[] | **[Target](#yandex.cloud.serverless.eventrouter.v1.Target)**

Targets of the rule. ||
|| deletionProtection | **bool**

Flag that disallow deletion of the rule. ||
|| status | enum **Status**

Rule status

- `STATUS_UNSPECIFIED`
- `CREATING`: Rule creation in progress, rule is not enabled yet
- `ENABLED`: Rule is operating
- `UPDATING`: Rule update in progress, rule is disabled during update
- `DISABLED`: Rule is explicitly disabled by the user
- `DELETING`: Rule deletion in progress ||
|#

## Filter {#yandex.cloud.serverless.eventrouter.v1.Filter}

#|
||Field | Description ||
|| jqFilter | **string**

JQ filter for matching events.

Includes only one of the fields `jqFilter`. ||
|#

## Target {#yandex.cloud.serverless.eventrouter.v1.Target}

#|
||Field | Description ||
|| yds | **[YdsTarget](#yandex.cloud.serverless.eventrouter.v1.YdsTarget)**

Includes only one of the fields `yds`, `ymq`, `function`, `container`, `gatewayWsBroadcast`, `logging`, `workflow`. ||
|| ymq | **[YmqTarget](#yandex.cloud.serverless.eventrouter.v1.YmqTarget)**

Includes only one of the fields `yds`, `ymq`, `function`, `container`, `gatewayWsBroadcast`, `logging`, `workflow`. ||
|| function | **[FunctionTarget](#yandex.cloud.serverless.eventrouter.v1.FunctionTarget)**

Includes only one of the fields `yds`, `ymq`, `function`, `container`, `gatewayWsBroadcast`, `logging`, `workflow`. ||
|| container | **[ContainerTarget](#yandex.cloud.serverless.eventrouter.v1.ContainerTarget)**

Includes only one of the fields `yds`, `ymq`, `function`, `container`, `gatewayWsBroadcast`, `logging`, `workflow`. ||
|| gatewayWsBroadcast | **[GatewayWebsocketBroadcastTarget](#yandex.cloud.serverless.eventrouter.v1.GatewayWebsocketBroadcastTarget)**

Includes only one of the fields `yds`, `ymq`, `function`, `container`, `gatewayWsBroadcast`, `logging`, `workflow`. ||
|| logging | **[LoggingTarget](#yandex.cloud.serverless.eventrouter.v1.LoggingTarget)**

Includes only one of the fields `yds`, `ymq`, `function`, `container`, `gatewayWsBroadcast`, `logging`, `workflow`. ||
|| workflow | **[WorkflowTarget](#yandex.cloud.serverless.eventrouter.v1.WorkflowTarget)**

Includes only one of the fields `yds`, `ymq`, `function`, `container`, `gatewayWsBroadcast`, `logging`, `workflow`. ||
|| transformer | **[Transformer](#yandex.cloud.serverless.eventrouter.v1.Transformer)**

Transformer of the target. ||
|| retrySettings | **[RetrySettings](#yandex.cloud.serverless.eventrouter.v1.RetrySettings)**

Retry settings of the target. ||
|| deadLetterQueue | **[PutQueueMessage](#yandex.cloud.serverless.eventrouter.v1.PutQueueMessage)**

Dead letter queue.

Includes only one of the fields `deadLetterQueue`.

Dead letter settings of the target. ||
|| status | enum **Status**

Status of the target.

- `STATUS_UNSPECIFIED`
- `ENABLED`: Target is enabled.
- `DISABLED`: Target is disabled.
- `RESOURCE_NOT_FOUND`: Target does not exist.
- `PERMISSION_DENIED`: Service account does not have read permission on source.
- `SUBJECT_NOT_FOUND`: Service account not found. ||
|#

## YdsTarget {#yandex.cloud.serverless.eventrouter.v1.YdsTarget}

#|
||Field | Description ||
|| database | **string**

Required field. Stream database. ||
|| streamName | **string**

Required field. Full stream name, like /ru-central1/aoegtvhtp8ob********/cc8004q4lbo6********/test. ||
|| serviceAccountId | **string**

Required field. Service account, which has write permission on the stream. ||
|#

## YmqTarget {#yandex.cloud.serverless.eventrouter.v1.YmqTarget}

#|
||Field | Description ||
|| queueArn | **string**

Required field. Queue ARN.
Example: yrn:yc:ymq:ru-central1:aoe***:test ||
|| serviceAccountId | **string**

Required field. Service account which has write access to the queue. ||
|#

## FunctionTarget {#yandex.cloud.serverless.eventrouter.v1.FunctionTarget}

#|
||Field | Description ||
|| functionId | **string**

Required field. Function ID. ||
|| functionTag | **string**

Function tag, optional. ||
|| serviceAccountId | **string**

Service account which has call permission on the function, optional. ||
|| batchSettings | **[BatchSettings](#yandex.cloud.serverless.eventrouter.v1.BatchSettings)**

Batch settings. ||
|#

## BatchSettings {#yandex.cloud.serverless.eventrouter.v1.BatchSettings}

#|
||Field | Description ||
|| maxCount | **int64**

Maximum batch size: trigger will send a batch if number of events exceeds this value. ||
|| maxBytes | **int64**

Maximum batch size: trigger will send a batch if total size of events exceeds this value. ||
|| cutoff | **[google.protobuf.Duration](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/duration)**

Required field. Maximum batch size: trigger will send a batch if its lifetime exceeds this value. ||
|#

## ContainerTarget {#yandex.cloud.serverless.eventrouter.v1.ContainerTarget}

#|
||Field | Description ||
|| containerId | **string**

Required field. Container ID. ||
|| containerRevisionId | **string**

Container revision ID. ||
|| path | **string**

Endpoint HTTP path to invoke. ||
|| serviceAccountId | **string**

Service account which should be used to call a container, optional. ||
|| batchSettings | **[BatchSettings](#yandex.cloud.serverless.eventrouter.v1.BatchSettings)**

Batch settings. ||
|#

## GatewayWebsocketBroadcastTarget {#yandex.cloud.serverless.eventrouter.v1.GatewayWebsocketBroadcastTarget}

#|
||Field | Description ||
|| gatewayId | **string**

Required field. Gateway ID. ||
|| path | **string**

Required field. Path. ||
|| serviceAccountId | **string**

Required field. Service account which has permission for writing to websockets. ||
|| batchSettings | **[BatchSettings](#yandex.cloud.serverless.eventrouter.v1.BatchSettings)**

Batch settings. ||
|#

## LoggingTarget {#yandex.cloud.serverless.eventrouter.v1.LoggingTarget}

#|
||Field | Description ||
|| logGroupId | **string**

Includes only one of the fields `logGroupId`, `folderId`.

Log group ID or folder ID. ||
|| folderId | **string**

Includes only one of the fields `logGroupId`, `folderId`.

Log group ID or folder ID. ||
|| serviceAccountId | **string**

Required field. Service account which has permission for writing logs. ||
|#

## WorkflowTarget {#yandex.cloud.serverless.eventrouter.v1.WorkflowTarget}

#|
||Field | Description ||
|| workflowId | **string**

Required field. Workflow ID. ||
|| serviceAccountId | **string**

Required field. SA which should be used to start workflow. ||
|| batchSettings | **[BatchSettings](#yandex.cloud.serverless.eventrouter.v1.BatchSettings)**

Batch settings. ||
|#

## Transformer {#yandex.cloud.serverless.eventrouter.v1.Transformer}

#|
||Field | Description ||
|| jqTransformer | **string**

JQ string inrerpolation expression for changing event format.

Includes only one of the fields `jqTransformer`. ||
|#

## RetrySettings {#yandex.cloud.serverless.eventrouter.v1.RetrySettings}

#|
||Field | Description ||
|| retryAttempts | **int64**

Maximum number of retries (extra calls) before an action fails. ||
|| maximumAge | **[google.protobuf.Duration](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/duration)**

Event goes to dlq when its age exceeds this value. Default is 24h. ||
|#

## PutQueueMessage {#yandex.cloud.serverless.eventrouter.v1.PutQueueMessage}

#|
||Field | Description ||
|| queueArn | **string**

ID of the queue. ||
|| serviceAccountId | **string**

Required field. Service account which has write permission on the queue. ||
|#