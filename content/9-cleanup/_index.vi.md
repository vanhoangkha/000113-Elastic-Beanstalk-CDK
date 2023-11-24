---
title : "Dọn dẹp tài nguyên"
date :  "`r Sys.Date()`" 
weight : 9 
chapter : false
pre : " <b> 9. </b> "
---
### Xóa CDK Pipeline và CloudFormation stack
1. Tại Cloud9 terminal, chạy đoạn lệnh này:

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
Xác nhận ***Y*** nếu được yêu cầu.
{{% /notice %}}

### Xóa GitHub token trong AWS Secrets Manager
1. Tại Cloud9 terminal, chạy đoạn lệnh này:

{{% notice%}}
    aws secretsmanager delete-secret --secret-id github-token --force-delete-without-recovery
{{% /notice %}}

### Xóa môi trường Cloud9
1. Đi đến [Cloud9](https://ap-southeast-1.console.aws.amazon.com/cloud9control/home?region=ap-southeast-1#/).
2. Chọn môi trường **FCJ-Env**.
3. Nhấn **Delete**.
4. Xác nhận ```Delete``` để xóa.