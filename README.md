
# Enforce a default retention period for Cloudwatch Log Groups

Amazon CloudWatch Logs allows to set a retention period when creating a Log Group. However, many log groups (e.g. those for lambda) are created automatically with the default retention policy of "Never Expire". This could bring to unwanted cost in case of noisy lambdas that may go unnoticed for a period of time.

This sample allows to automatize the enforcement of a default retention period for selected Cloudwatch Log Groups.

## Requirements

For this sample to work you should have the following prerequisites:
* An AWS Account
* An user or role with permisisons to deploy AWS CloudFormation stacks

## How it works

The solution consists of a AWS Lambda scheduled by a AWS EventBridge rule. Every time the schedule is triggered, the Lambda code runs and sets the new default retention period for all applicable AWS CloudWatch Log Group name. The log group names can be selected with a simple text pattern or with a regular expression.
The provided AWS CloudFormation template creates
* An AWS IAM Lambda service Role that allows the function to read existing log groups and update the retention period
* An AWS Lambda that reads existing log groups and updates the retention period
* An AWS EventBridge Rule that schedules the lambda function execution

## Installation Instructions

1. Download the AWS CloudFormation template `batch_default_log_group_retention.json` to your machine or upload it to an S3 bucket
2. Access to your AWS account and switch to the region in which you want to apply the sample
3. Navigate to AWS CloudFormation console and select Stacks>Create Stack with new Resources. Alternatively, you may follow this url: https://console.aws.amazon.com/cloudformation/home#/stacks/create
4. Select "Template is ready" and "Upload a template file". Choose the file that you previusly downloaded and click "Next"
5. Enter a name for the stack, for example "batch-default-log-group-retention"
6. Fill in the parameters and then click "Next. All parameters are required
    * *defaultLogRetentionPeriod* is the number of days that you want to set as default log group retention period e.g. 90
    * *logGroupPatternMatchType* is the match pattern type you want to use. 'Simple Text Pattern' will match a simple text in the log group name e.g. lambda matches /aws/lambda/loggroupname. To use a regular expression select 'Regular Expression'
    * *logGroupPatternMatchValue* represents the text you want to match in the log group name e.g. if you selected 'Simple Text Pattern', inserting lambda will match any log group name which name contains *lambda*. To achieve the same result with 'Regular Expression' you should insert .\*lambda.\* 
    * *scheduleInterval* is the run schedule interval, in hours e.g. 24. Minimum value allowed is 2 hours
7. Accept all defaults in next screen clicking Next
8. Review your configuration and then tick the checkbox "I acknowledge that AWS CloudFormation might create IAM resources" before clicking Submit
