[![Build Status](https://travis-ci.org/cfn-modules/s3-bucket.svg?branch=master)](https://travis-ci.org/cfn-modules/s3-bucket)
[![NPM version](https://img.shields.io/npm/v/@cfn-modules/s3-bucket.svg)](https://www.npmjs.com/package/@cfn-modules/s3-bucket)

# cfn-modules: AWS S3 bucket

AWS S3 bucket with encryption and backups.


## Install

> Install [Node.js and npm](https://nodejs.org/) first!

```
npm i @cfn-modules/s3-bucket
```

## Usage

```
---
AWSTemplateFormatVersion: '2010-09-09'
Description: 'cfn-modules example'
Resources:
  Bucket:
    Type: 'AWS::CloudFormation::Stack'
    Properties:
      Parameters:
        KmsKeyModule: !GetAtt 'Key.Outputs.StackName' # optional
        BucketName: '' # optional
        Access: Private # optional
        Cors: Disabled # optional
        Versioning: 'true' # optional
        NoncurrentVersionExpirationInDays: '0' # optional
        ExpirationInDays: '0' # optional
        LambdaEventTargetLambdaModule1: '' # optional
        LambdaEventType1: 's3:ObjectCreated:*' # optional
        LambdaEventTargetLambdaModule2: '' # optional
        LambdaEventType2: 's3:ObjectRemoved:*' # optional
        LambdaEventTargetLambdaModule3: '' # optional
        LambdaEventType3: 's3:ReducedRedundancyLostObject' # optional
      TemplateURL: './node_modules/@cfn-modules/s3-bucket/module.yml'
```

## Examples

* [serverless-iam](https://github.com/cfn-modules/docs/tree/master/examples/serverless-iam)
* [serverless-image-resize](https://github.com/cfn-modules/docs/tree/master/examples/serverless-image-resize)

## Related modules

* [lambda-function](https://github.com/cfn-modules/lambda-function)

## Parameters

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Description</th>
      <th>Default</th>
      <th>Required?</th>
      <th>Allowed values</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>KmsKeyModule</td>
      <td>Stack name of <a href="https://www.npmjs.com/package/@cfn-modules/kms-key">kms-key module</a> (only works in combination with Access := [Private, PublicRead])</td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>BucketName</td>
      <td>name of the bucket</td>
      <td>auto generated value</td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>Access</td>
      <td>Access policy of the bucket</td>
      <td>Private</td>
      <td>no</td>
      <td>[Private, PublicRead, CloudFrontRead, CloudFrontAccessLogWrite, ElbAccessLogWrite, ConfigWrite, CloudTrailWrite, FlowLogWrite]</td>
    </tr>
    <tr>
      <td>Cors</td>
      <td>CORS policy of the bucket</td>
      <td>Disabled</td>
      <td>no</td>
      <td>[Disabled, AllowAll]</td>
    </tr>
    <tr>
      <td>Versioning</td>
      <td>Enable versioning to keep a backup if objects change</td>
      <td>true</td>
      <td>no</td>
      <td>[true, false, 'false-but-was-true']</td>
    </tr>
    <tr>
      <td>NoncurrentVersionExpirationInDays</td>
      <td>Remove noncurrent object versions after days (set to 0 to disable)</td>
      <td>0</td>
      <td>no</td>
      <td>[0-N]</td>
    </tr>
    <tr>
      <td>ExpirationInDays</td>
      <td>Remove objects after days (set to 0 to disable).</td>
      <td>0</td>
      <td>no</td>
      <td>[0-N]</td>
    </tr>
    <tr>
      <td>LambdaEventTargetLambdaModule1</td>
      <td>Stack name of lambda-function module to receive events from this S3 bucket. Also grants the Lambda function access to this bucket and this bucket access to the Lambda function.</td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>LambdaEventType1</td>
      <td>S3 bucket events you want to receive (can not be the same as LambdaEventType2 or LambdaEventType3)</td>
      <td>s3:ObjectCreated:*</td>
      <td>no</td>
      <td><a href="https://docs.aws.amazon.com/AmazonS3/latest/dev/NotificationHowTo.html#notification-how-to-event-types-and-destinations">Supported event types</a></td>
    </tr>
    <tr>
      <td>LambdaEventTargetLambdaModule2</td>
      <td>Stack name of lambda-function module to receive events from this S3 bucket. Also grants the Lambda function access to this bucket and this bucket access to the Lambda function.</td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>LambdaEventType2</td>
      <td>S3 bucket events you want to receive (can not be the same as LambdaEventType1 or LambdaEventType3)</td>
      <td>s3:ObjectRemoved:*</td>
      <td>no</td>
      <td><a href="https://docs.aws.amazon.com/AmazonS3/latest/dev/NotificationHowTo.html#notification-how-to-event-types-and-destinations">Supported event types</a></td>
    </tr>
    <tr>
      <td>LambdaEventTargetLambdaModule31</td>
      <td>Stack name of lambda-function module to receive events from this S3 bucket. Also grants the Lambda function access to this bucket and this bucket access to the Lambda function.</td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>LambdaEventType3</td>
      <td>S3 bucket events you want to receive (can not be the same as LambdaEventType1 or LambdaEventType2)</td>
      <td>s3:ReducedRedundancyLostObject</td>
      <td>no</td>
      <td><a href="https://docs.aws.amazon.com/AmazonS3/latest/dev/NotificationHowTo.html#notification-how-to-event-types-and-destinations">Supported event types</a></td>
    </tr>
  </tbody>
</table>

## Limitations

* Secure: Backups are only per object (you can not easily restore the whole bucket to a specific state)
* Secure: If you connect a Lambda function without setting the `BucketName` parameter the least privilege principle is softened: Invocations to the Lambda function are allowed from all S3 buckets inside your AWS account.
