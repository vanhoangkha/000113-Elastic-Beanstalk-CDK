---
title : "Triển khai ứng dụng"
date :  "`r Sys.Date()`" 
weight : 7 
chapter : false
pre : " <b> 7. </b> "
---
### Khởi động CDK trong tài khoản của bạn
1. Sao chép đoạn mã này.

{{% notice %}}
    npx cdk bootstrap aws://ACCOUNT-NUMBER/REGION
{{% /notice %}}

2. Thay thế **ACCOUNT-NUMBER** với Account ID.
3. Thay thế **REGION** với Region ID
4. Dán và chạy tại terminal.
 
![Create CDK Infrastructure](/images/7.deployapp/7.1deployapp.png?pc=90pt)
5. Sau khi khởi động, đi đến [CloudFormation](https://ap-southeast-1.console.aws.amazon.com/cloudformation/home?region=ap-southeast-1#/stacks). Bạn sẽ thấy một stack tên là **CDKToolkit** vừa được tạo.

![Create CDK Infrastructure](/images/7.deployapp/7.2deployapp.png?pc=90pt)


### Xây dựng và triển khai ứng dụng CDK
Sau khi chúng ta khởi động xong tài khoản và vùng AWS, chúng ta đã sẵn sàng xây dựng và triển khai ứng dụng CDK.
1. Tại terminal của Cloud9, xây dựng ứng dụng CDK.

{{% notice %}}
    npm run build
{{% /notice %}}

![Create CDK Infrastructure](/images/7.deployapp/7.3deployapp.png?pc=90pt)

2. Nếu không xuất hiện lỗi thì quá trình này đã thành công. Bây giờ chúng ta có thể đẩy tất cả code lên GitHub repository.

{{% notice %}}
    git add .
    git commit -m "empty pipeline"
    git push
{{% /notice %}}

![Create CDK Infrastructure](/images/7.deployapp/7.4deployapp.png?pc=90pt)

{{% notice warning%}}
Nhập GitHub username và access token nếu được yêu cầu.
{{% /notice %}}

3. Triển khai ứng dụng CDK lên AWS.

{{% notice %}}
    npx cdk deploy
{{% /notice %}}
{{% notice warning%}}
Nhập **Y** nếu được yêu cầu.
{{% /notice %}}

![Create CDK Infrastructure](/images/7.deployapp/7.5deployapp.png?pc=90pt)

4. Sau khi ứng dụng CDK được triển khai lên AWS thành công, một CloudFormation tên **CdkPipelineStack** sẽ được tạo.
5. Đi đến [Stack of CloudFormation](https://ap-southeast-1.console.aws.amazon.com/cloudformation/home?region=ap-southeast-1#/stacks) để kiểm tra.

![Create CDK Infrastructure](/images/7.deployapp/7.6deployapp.png?pc=90pt)

6. CloudFormation stack này sẽ tạo một **Empty CodePipeline** tên **MyServicePipeline**.
7. Đi đến [CodePipeline](https://ap-southeast-1.console.aws.amazon.com/codesuite/codepipeline/pipelines?region=ap-southeast-1) để thấy đường ống của bạn.

![Create CDK Infrastructure](/images/7.deployapp/7.7deployapp.png?pc=90pt)

8. Nhấn vào **pipeline** để thấy các giai đoạn.

![Create CDK Infrastructure](/images/7.deployapp/7.8deployapp.png?pc=90pt)

### Thêm một giai đoạn Deploy cho môi trường Elastic Beanstalk
Cho đến nay, chúng ta đã cung cấp một đường ống rỗng và đường ống đó vẫn chưa triển khai ứng dụng web của chúng ta. Bây giờ, chúng ta sẽ tạo một giai đoạn để triển khai ứng dụng lên Elastic Beanstalk.
1. Tạo một tệp mới **lib/eb-stage.ts**.
2. Dán đoạn code bên dưới:

{{% notice %}}
    import {  Stage } from 'aws-cdk-lib';
    import { Construct } from 'constructs';
    import { EBEnvProps, EBApplnStack } from './eb-appln-stack';

    /**
    * Deployable unit of web service app
    */
    export class CdkEBStage extends Stage {
        
    constructor(scope: Construct, id: string, props?: EBEnvProps) {
        super(scope, id, props);

        const service = new EBApplnStack(this, 'WebService', {
            minSize : props?.minSize, 
            maxSize : props?.maxSize,
            instanceTypes : props?.instanceTypes,
            envName : props?.envName
        } );

    }
    }
{{% /notice %}}

![Create CDK Infrastructure](/images/7.deployapp/7.9deployapp.png?pc=90pt)

3. Thêm dòng code mới vào **lib/cdk-pipeline-stack.ts**.

{{% notice %}}
    import { CdkEBStage } from './eb-stage';
{{% /notice %}}

4. Thêm đoạn code vào phần bên dưới của đoạn **This is where we add the application stages**.

{{% notice %}}
    // deploy beanstalk app
    // For environment with all default values:
    // const deploy = new CdkEBStage(this, 'Pre-Prod');

    // For environment with custom AutoScaling group configuration
    const deploy = new CdkEBStage(this, 'Pre-Prod', { 
        minSize : "1",
        maxSize : "2"
    });
    const deployStage = pipeline.addStage(deploy); 
{{% /notice %}}

![Create CDK Infrastructure](/images/7.deployapp/7.10deployapp.png?pc=90pt)

5. Chạy **npm run build** và đẩy code lên GitHub repository.

{{% notice %}}
    npm run build
    git add .
    git commit -m 'Add Pre-Prod stage'
    git push
{{% /notice %}}

![Create CDK Infrastructure](/images/7.deployapp/7.11deployapp.png?pc=90pt)

