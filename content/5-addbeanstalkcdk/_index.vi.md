---
title : "Thêm Elastic Beanstalk CDK "
date: 2024-01-01
weight : 5 
chapter : false
pre : " <b> 5. </b> "
---
### Thêm Elastic Beanstalk CDK Dependencies
kế tiếp, chúng ta sẽ tạo ứng dụng Elastic Beanstalk, phiên bản ứng dụng, và môi trường để chúng ta có thể triển khai ứng dụng web đã được tải lên S3 bằng S3 Assets.
1. Thêm dependency vào Elastic Beanstalk cho CDK tại phần trên của tệp **lib/eb-appln-stack.ts**.

{{% notice %}}
    import * as elasticbeanstalk from 'aws-cdk-lib/aws-elasticbeanstalk';
{{% /notice %}}

2. Đặt code phía dưới code của S3 Assets trong tệp **lib/eb-appln-stack.ts file**. Đoạn code này sẽ tạo một ứng dụng tên MyWebApp tại Elastic Beanstalk.

{{% notice %}}
    const appName = 'MyWebApp';
    const app = new elasticbeanstalk.CfnApplication(this, 'Application', {
        applicationName: appName,
    });
{{% /notice %}}

### Tạo phiên bản ứng dụng Elastic Beanstalk
Bây giờ chúng ta cần tạo phiên bản ứng dụng Elastic Beanstalk từ tài nguyên trên S3 đã được tạo trước đó. 
1. Phần code này sẽ tạo phiên bản ứng dụng bằng cách sử dụng S3 bucket name và S3 object key mà S3 Assets và CDK sẽ cũng cấp cho phương thức này.

{{% notice %}}
    // Create an app version from the S3 asset defined earlier
    const appVersionProps = new elasticbeanstalk.CfnApplicationVersion(this, 'AppVersion', {
        applicationName: appName,
        sourceBundle: {
            s3Bucket: webAppZipArchive.s3BucketName,
            s3Key: webAppZipArchive.s3ObjectKey,
        },
    });
{{% /notice %}}

2. Thêm một dependency để đảm bảo rằng ứng dụng Elastic Beanstalk application tồn tại trước khi tạo phiên bản ứng dụng. 

{{% notice %}}
    // Make sure that Elastic Beanstalk app exists before creating an app version
    appVersionProps.addDependency(app);
{{% /notice %}}

### Tạo Instance Profile
Để tạo môi trường Elastic Beanstalk, chúng ta sẽ cần cung cấp một instance profile để thông qua vai trò thông tin đến máy chủ Amazon EC2 khi nó khởi chạy.
1. Thêm IAM dependency trong CDK stack.

{{% notice %}}
    import * as iam from 'aws-cdk-lib/aws-iam';
{{% /notice %}}

2. Tạo phiên bản ứng dụng:
    + Điều đầu tiên đoạn code làm là tạo ra một IAM role mới (**myRole**).
    + Để cho phép máy chủ EC2 trong môi trường của chúng ta đảm nhận role, instance profile chỉ định Amazon EC2 với tư cách là một thực thể đáng tin cậy trong chính sách mối quan hệ tin cậy.
    + Với vai trò đó, sau đó chúng ta thêm chính sách được quản lý **AWSElasticBeanstalkWebTier**. Sau đó chúng ta tạo instance profile với vai trò đó và tên hồ sơ.

{{% notice %}}
    // Create role and instance profile
    const myRole = new iam.Role(this, `${appName}-aws-elasticbeanstalk-ec2-role`, {
        assumedBy: new iam.ServicePrincipal('ec2.amazonaws.com'),
    });

    const managedPolicy = iam.ManagedPolicy.fromAwsManagedPolicyName('AWSElasticBeanstalkWebTier')
    myRole.addManagedPolicy(managedPolicy);

    const myProfileName = `${appName}-InstanceProfile`

    const instanceProfile = new iam.CfnInstanceProfile(this, myProfileName, {
        instanceProfileName: myProfileName,
        roles: [
            myRole.roleName
        ]
    });
{{% /notice %}}

### Tạo môi trường Elastic Beanstalk
Phần cuối cùng chúng ta cần tạo là môi trường Elastic Beanstalk. Môi trường là một tập hợp các tài nguyên AWS chạy trên một phiên bản ứng dụng. Đối với môi trường, chúng ta sẽ cần cung cấp một số thông tin về cơ sở hạ tầng.
1. Để xác định các tùy chọn cấu hình, hãy thêm các dòng code sau:

{{% notice %}}
    // Example of some options which can be configured
    const optionSettingProperties: elasticbeanstalk.CfnEnvironment.OptionSettingProperty[] = [
        {
            namespace: 'aws:autoscaling:launchconfiguration',
            optionName: 'IamInstanceProfile',
            value: myProfileName,
        },
        {
            namespace: 'aws:autoscaling:asg',
            optionName: 'MinSize',
            value: props?.maxSize ?? '1',
        },
        {
            namespace: 'aws:autoscaling:asg',
            optionName: 'MaxSize',
            value: props?.maxSize ?? '1',
        },
        {
            namespace: 'aws:ec2:instances',
            optionName: 'InstanceTypes',
            value: props?.instanceTypes ?? 't2.micro',
        },
    ];

{{% /notice %}}

2. Thêm các dòng code dưới đây vào tệp **lib/eb-appln-stack.ts**:

{{% notice %}}
    // Create an Elastic Beanstalk environment to run the application
    const elbEnv = new elasticbeanstalk.CfnEnvironment(this, 'Environment', {
        environmentName: props?.envName ?? "MyWebAppEnvironment",
        applicationName: app.applicationName || appName,
        solutionStackName: 'SOLUTION-STACK-NAME',
        optionSettings: optionSettingProperties,
        versionLabel: appVersionProps.ref,
    });
{{% /notice %}}

3. Thay thế **SOLUTION-STACK-NAME** với [Phiên bản mới nhất của Solution Stack Name được hổ trợ](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html?sc_channel=el&sc_campaign=devopswave&sc_content=cicdcdkebaws&sc_geo=mult&sc_country=mult&sc_outcome=acq#platforms-supported.nodejs). Tại thời điểm này, phiên bản mới nhất là ```64bit Amazon Linux 2023 v6.0.2 running Node.js 18```.
![Create CDK Infrastructure](/images/5.addbeanstalk/5.1version.png?pc=90pt)

### Cuối cùng, code của tệp lib/eb-appln-stack.ts:

{{% notice %}}
    import * as cdk from 'aws-cdk-lib';
    import { Construct } from 'constructs';
    // Add import statements here
    import * as s3assets from 'aws-cdk-lib/aws-s3-assets';
    import * as elasticbeanstalk from 'aws-cdk-lib/aws-elasticbeanstalk';
    import * as iam from 'aws-cdk-lib/aws-iam';
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
        // Construct an S3 asset Zip from directory up.
        const webAppZipArchive = new s3assets.Asset(this, 'WebAppZip', {
        path: `${__dirname}/../src`,
        });
        
        const appName = 'MyWebApp';
        const app = new elasticbeanstalk.CfnApplication(this, 'Application', {
            applicationName: appName,
        });
        
        // Create an app version from the S3 asset defined earlier
        const appVersionProps = new elasticbeanstalk.CfnApplicationVersion(this, 'AppVersion', {
        applicationName: appName,
        sourceBundle: {
            s3Bucket: webAppZipArchive.s3BucketName,
            s3Key: webAppZipArchive.s3ObjectKey,
        },
        });
        
        // Make sure that Elastic Beanstalk app exists before creating an app version
        appVersionProps.addDependency(app);
        
        // Create role and instance profile
        const myRole = new iam.Role(this, `${appName}-aws-elasticbeanstalk-ec2-role`, {
            assumedBy: new iam.ServicePrincipal('ec2.amazonaws.com'),
        });
        
        const managedPolicy = iam.ManagedPolicy.fromAwsManagedPolicyName('AWSElasticBeanstalkWebTier')
        myRole.addManagedPolicy(managedPolicy);
        
        const myProfileName = `${appName}-InstanceProfile`
        
        const instanceProfile = new iam.CfnInstanceProfile(this, myProfileName, {
            instanceProfileName: myProfileName,
            roles: [
                myRole.roleName
            ]
        });

        // Example of some options which can be configured
        const optionSettingProperties: elasticbeanstalk.CfnEnvironment.OptionSettingProperty[] = [
            {
                namespace: 'aws:autoscaling:launchconfiguration',
                optionName: 'IamInstanceProfile',
                value: myProfileName,
            },
            {
                namespace: 'aws:autoscaling:asg',
                optionName: 'MinSize',
                value: props?.maxSize ?? '1',
            },
            {
                namespace: 'aws:autoscaling:asg',
                optionName: 'MaxSize',
                value: props?.maxSize ?? '1',
            },
            {
                namespace: 'aws:ec2:instances',
                optionName: 'InstanceTypes',
                value: props?.instanceTypes ?? 't2.micro',
            },
        ];

        // Create an Elastic Beanstalk environment to run the application
        const elbEnv = new elasticbeanstalk.CfnEnvironment(this, 'Environment', {
            environmentName: props?.envName ?? "MyWebAppEnvironment",
            applicationName: app.applicationName || appName,
            solutionStackName: '64bit Amazon Linux 2023 v6.0.2 running Node.js 18',
            optionSettings: optionSettingProperties,
            versionLabel: appVersionProps.ref,
        });

    }
    }
{{% /notice %}}