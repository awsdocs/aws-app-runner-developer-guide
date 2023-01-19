# Handling App Runner events in EventBridge<a name="monitor-ev"></a>

Using Amazon EventBridge, you can set up event\-driven rules that monitor a stream of real\-time data from your AWS App Runner service for certain patterns\. When a pattern for a rule is matched, EventBridge initiates an action in a target such as AWS Lambda, Amazon ECS, AWS Batch, and Amazon SNS\. For example, you can set a rule for sending out email notifications by signaling an Amazon SNS topic whenever a deployment to your service fails\. Or, you can set a Lambda function to notify a Slack channel whenever a service update fails\. For more information about EventBridge, see [Amazon EventBridge User Guide](https://docs.aws.amazon.com/eventbridge/latest/userguide/)\.

App Runner sends the following event types to EventBridge
+ *Service status change* – A change in the status of an App Runner service\. For example, a service status changed to `DELETE_FAILED`\.
+ *Service operation status change* – A change in the status of a long, asynchronous operation on an App Runner service\. For example, a service started to create, a service update successfully completed, or a service deployment completed with errors\.

## Creating an EventBridge rule to act on App Runner events<a name="monitor-ev.rule"></a>

An EventBridge *event* is an object that defines some standard EventBridge fields, such as the source AWS service and the detail \(event\) type, and an event\-specific set of fields with the event details\. To create an EventBridge rule, you use the EventBridge console to define an *event pattern* \(which events should bet tracked\) and specify a *target action* \(what should be done on a match\)\. An event pattern is similar to the events that it matches\. You specify a subset of fields to match, and for each field, you specify a list of possible values\. This topic provides examples of App Runner events and event patterns\.

For more information about creating EventBridge rules, see [Creating a rule for an AWS service](https://docs.aws.amazon.com/eventbridge/latest/userguide/create-eventbridge-rule.html) in the *Amazon EventBridge User Guide*\.

**Note**  
Some services support *pre\-defined patterns* in EventBridge\. This simplifies how an event pattern is created\. You select field values on a form, and EventBridge generates the pattern for you\. At this time, App Runner doesn't support pre\-defined patterns\. You have to enter the pattern as a JSON object\. You can use the examples in this topic as a starting point\.

## App Runner event examples<a name="monitor-ev.event-examples"></a>

These are some examples to events that App Runner sends to EventBridge\.
+ A service status change event\. Specifically, a service that changed from the `OPERATION_IN_PROGRESS` to the `RUNNING` status\.

  ```
  { 
    "version": "0",
    "id": "6a7e8feb-b491-4cf7-a9f1-bf3703467718",
    "detail-type": "AppRunner Service Status Change",
    "source": "aws.apprunner",
    "account": "111122223333",
    "time": "2021-04-29T11:54:23Z",
    "region": "us-east-2",
    "resources": [
      "arn:aws:apprunner:us-east-2:123456789012:service/my-app/8fe1e10304f84fd2b0df550fe98a71fa"
    ],
    "detail": {
      "PreviousStatus": "OPERATION_IN_PROGRESS",
      "CurrentStatus": "RUNNING",
      "ServiceName": "my-app",
      "ServiceId": "8fe1e10304f84fd2b0df550fe98a71fa",
      "Message": "Service status is set to RUNNING.",
      "Severity": "INFO"
    }
  }
  ```
+ An operation status change event\. Specifically, an `UpdateService` operation that completed successfully\.

  ```
  { 
    "version": "0",
    "id": "6a7e8feb-b491-4cf7-a9f1-bf3703467718",
    "detail-type": "AppRunner Service Operation Status Change",
    "source": "aws.apprunner",
    "account": "111122223333",
    "time": "2021-04-29T18:43:48Z",
    "region": "us-east-2",
    "resources": [
      "arn:aws:apprunner:us-east-2:123456789012:service/my-app/8fe1e10304f84fd2b0df550fe98a71fa"
    ],
    "detail": {
      "operationStatus": "UpdateServiceCompletedSuccessfully",
      "ServiceName": "my-app",
      "ServiceId": "8fe1e10304f84fd2b0df550fe98a71fa",
      "Message": "Service update completed successfully. New application and configuration is deployed.",
      "Severity": "INFO"
    }
  }
  ```

## App Runner event pattern examples<a name="monitor-ev.pattern-examples"></a>

The following examples demonstrate event patterns that you can use in EventBridge rules to match one or more App Runner events\. An event pattern is similar to an event\. Include only the fields that you want to match, and provide a list instead of a scalar to each one\.
+ Match all service status change events for services of a specific account, where the service is no longer in `RUNNING` status\.

  ```
  { 
    "detail-type": [ "AppRunner Service Status Change" ],
    "source": [ "aws.apprunner" ],
    "account": [ "111122223333" ],
    "detail": {
      "PreviousStatus": [ "RUNNING" ]
    }
  }
  ```
+ Match all operation status change events for services of a specific account, where the operation failed\.

  ```
  { 
    "detail-type": [ "AppRunner Service Operation Status Change" ],
    "source": [ "aws.apprunner" ],
    "account": [ "111122223333" ],
    "detail": {
      "operationStatus": [
        "CreateServiceFailed",
        "DeleteServiceFailed",
        "UpdateServiceFailed",
        "DeploymentFailed",
        "PauseServiceFailed",
        "ResumeServiceFailed"
      ]
    }
  }
  ```

## App Runner event reference<a name="monitor-ev.ref"></a>

### Service status change<a name="monitor-ev.ref.service"></a>

A service status change event has `detail-type` set to `AppRunner Service Status Change`\. It has the following detail fields and values:

```
"PreviousStatus": "any valid service status",
"CurrentStatus": "any valid service status",
"ServiceName": "your service name",
"ServiceId": "your service ID",
"Message": "Service status is set to CurrentStatus.",
"Severity": "varies"
```

### Operation status change<a name="monitor-ev.ref.operation"></a>

An operation status change event has `detail-type` set to `AppRunner Service Operation Status Change`\. It has the following detail fields and values:

```
"Status": "see following table",
"ServiceName": "your service name",
"ServiceId": "your service ID",
"Message": "see following table",
"Severity": "varies"
```

The following table lists all possible status codes and related messages\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/apprunner/latest/dg/monitor-ev.html)
