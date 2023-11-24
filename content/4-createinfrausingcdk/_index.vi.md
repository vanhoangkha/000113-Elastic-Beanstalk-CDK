---
title : "Tạo hạ tầng bằng CDK"
date :  "`r Sys.Date()`" 
weight : 4 
chapter : false
pre : " <b> 4. </b> "
---
### Tạo ứng dụng CDK
1. Tạo một đường dẫn mới và đi đến đường dẫn đó.

{{% notice %}}
    cd ..
    mkdir cdk-pipeline-eb-demo
    cd cdk-pipeline-eb-demo
{{% /notice %}}

{{% notice info %}}
If your NodeJS application is still running, press **Ctrl + C** to stop.
{{% /notice %}}

2. Khởi tạo ứng dụng CDK mà được sử dụng để tạo cơ sở hạ tầng.

{{% notice %}}
    npx cdk init app --language typescript
{{% /notice %}}

![Create CDK Infrastructure](/images/4.createcdkinfra/4.1createcdk.png?pc=90pt)

3. Khởi tạo git repository.

{{% notice %}}
    git branch -m main
{{% /notice %}}

### Di chuyển ứng dụng lên GitHub
1. Sau khi GitHub repository được tạo, chúng ta sẽ đẩy ứng dụng lên đó. Di chuyển các tệp ứng dụng đến tập tin mới **src**.

{{% notice %}}
    cp -r ../my_webapp ./src
    echo '!src/*' >> .gitignore
    echo 'src/package-lock.json' >> .gitignore
    echo 'src/node_modules' >> .gitignore
{{% /notice %}}

2. Đẩy tất cả các tệp lên GitHub. Thay thế **YOUR_USERNAME** với github org của bạn và **YOUR_REPOSITORY** với tên của repository.

{{% notice %}}
    git add .
    git commit -m "initial commit"
    git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git 
    git config credential.helper 'cache --timeout=3600'
    git push -u origin main
{{% /notice %}}

3. Bạn sẽ được yêu cầu cung cấp **Username** và **Password**.

{{% notice note%}}
 Mật khẩu là access token của tài khoản GitHub được tạo ra ở bước [2.1- Create GitHub repository](../2-preparation/2.1-createrepo/)
{{% /notice %}}

![Create CDK Infrastructure](/images/4.createcdkinfra/4.2createcdk.png?pc=90pt)

### Tạo code cho stack tài nguyên
Chúng ta sẽ xóa tệp mặc định được tạo ra bởi CDK và triển khai code cho stack tài nguyên ElasticBeanstalk.
1. Tại **./cdk-pipeline-eb-demo/lib**, xóa tệp **cdk-pipeline-eb-demo.ts**.
2. Tạo tệp mới tên **eb-appln-stack.ts**.
3. Dán đoạn code bên dưới và lưu.

{{% notice %}}
    import * as cdk from 'aws-cdk-lib';
    import { Construct } from 'constructs';
    // Add import statements here

    export interface EBEnvProps extends cdk.StackProps {
        // Autoscaling group configuration
    minSize?: string;
    maxSize?: string;
    instanceTypes?: string;
    envName?: string;
    }

    export class EBApplnStack extends cdk.Stack {
    constructor(scope: Construct, id: string, props?: EBEnvProps) {
        super(scope, id, props);

        // The code that defines your stack goes here

    }
    }
{{% /notice %}}

![Create CDK Infrastructure](/images/4.createcdkinfra/4.3createcdk.png?pc=90pt)

Một stack tài nguyên là một tập hợp các tài nguyên cơ sở hạ tầng đám mây - trong trường hợp này là tất cả tài nguyên AWS — sẽ được cung cấp trong một tài khoản cụ thể. Tài khoản nơi các tài nguyên này sẽ được cung cấp là stack mà bạn đã đặt cấu hình trong điều kiện tiên quyết. Trong stack tài nguyên này, chúng ta sẽ tạo các tài nguyên sau: 
+ **IAM Instance profile and role**: Một vùng chưa cho AWS Identity and Access Management (IAM) role mà chúng ta có thể sử dụng để truyền thông tin role đến phiên bản Amazon EC2 khi phiên bản khởi động.
+ **S3 Assets**: Giúp tải ứng dụng đã nén lên Amazon Simple Storage Service (S3) và sẽ cung cấp cho ứng dụng CDK để lấy vị trí đối tượng.
+ **Elastic Beanstalk App**: Một tập hợp logic các thành phần Elastic Beanstalk, bao gồm môi trường, phiên bản và cấu hình môi trường.
+ **Elastic Beanstalk App Version**: Một sự lặp lại cụ thể, được gắn nhãn của code có thể triển khai cho một ứng dụng web. Phiên bản ứng dụng trỏ tới một đối tượng Amazon S3 chứa code có thể triển khai, trong trường hợp này là tệp zip mà chúng tôi sẽ tải lên S3 bằng S3 Assets. Ứng dụng có thể có nhiều phiên bản và mỗi phiên bản ứng dụng là duy nhất.
+ **Elastic Beanstalk Environment**: Tập hợp các tài nguyên AWS chạy phiên bản ứng dụng. Mỗi môi trường chỉ chạy một phiên bản ứng dụng tại một thời điểm.


### Tải ứng dụng lên S3 một cách tự động
1. Tại tệp **lib/eb-appln-stack.ts**, thêm đoạn code này ở phần trên của tệp.

{{% notice %}}
    import * as s3assets from 'aws-cdk-lib/aws-s3-assets';
{{% /notice %}}

2. Dưới đoạn **The code that defines your stack goes here**, thêm đoạn code này :

{{% notice %}}
    // Construct an S3 asset Zip from directory up.
    const webAppZipArchive = new s3assets.Asset(this, 'WebAppZip', {
      path: `${__dirname}/../src`,
    });
{{% /notice %}}

![Create CDK Infrastructure](/images/4.createcdkinfra/4.4createcdk.png?pc=90pt)