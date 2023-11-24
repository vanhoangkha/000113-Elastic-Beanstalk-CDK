---
title : "Clean up resources"
date :  "`r Sys.Date()`" 
weight : 9 
chapter : false
pre : " <b> 9. </b> "
---
### Delete CDK Pipeline and CloudFormation stack
1. At Cloud9 terminal, run this command:

{{% notice%}}
    cdk destroy
{{% /notice %}}

{{% notice%}}
    aws cloudformation delete-stack --stack-name Pre-Prod-WebService
{{% /notice %}}

{{% notice%}}
    aws cloudformation delete-stack --stack-name CDKToolkit
{{% /notice %}}

{{% notice note%}}
Confirm ***Y*** if requested.
{{% /notice %}}

### Delete GitHub token in AWS Secrets Manager.
1. At Cloud9 terminal, run this command:

{{% notice%}}
    aws secretsmanager delete-secret --secret-id github-token --force-delete-without-recovery
{{% /notice %}}

### Delete Cloud9 Environment.
1. Go to [Cloud9](https://ap-southeast-1.console.aws.amazon.com/cloud9control/home?region=ap-southeast-1#/).
2. Select **FCJ-Env** environment.
3. Click **Delete**.
4. Confirm ```Delete``` to delete.