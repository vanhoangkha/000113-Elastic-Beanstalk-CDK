---
title : "Deploy application"
date :  "`r Sys.Date()`" 
weight : 7 
chapter : false
pre : " <b> 7. </b> "
---
### Bootstrap CDK in Your Account
1. Copy this command.

{{% notice %}}
    npx cdk bootstrap aws://ACCOUNT-NUMBER/REGION
{{% /notice %}}

2. Replace **ACCOUNT-NUMBER** with your Account ID.
3. Replace **REGION** with your Region ID
4. Paste and run it in terminal.
 
![Create CDK Infrastructure](/images/7.deployapp/7.1deployapp.png?pc=90pt)
5. After bootstraping, go to [CloudFormation](https://ap-southeast-1.console.aws.amazon.com/cloudformation/home?region=ap-southeast-1#/stacks). You will see a stack name **CDKToolkit** had been provisioned.

![Create CDK Infrastructure](/images/7.deployapp/7.2deployapp.png?pc=90pt)


### Build and Deploy the CDK Application
After we have bootstrapped our AWS account and Region, we are ready to build and deploy our CDK application.
1. At terminal of Cloud9 environment, build the CDK application.

{{% notice %}}
    npm run build
{{% /notice %}}

![Create CDK Infrastructure](/images/7.deployapp/7.3deployapp.png?pc=90pt)

2. If there are no errors in our application, this will succeed. We can now push all the code to the GitHub repository.

{{% notice %}}
    git add .
    git commit -m "empty pipeline"
    git push
{{% /notice %}}

![Create CDK Infrastructure](/images/7.deployapp/7.4deployapp.png?pc=90pt)

{{% notice warning%}}
Input the GitHub username and access token if necessary.
{{% /notice %}}

3. Deploy the CDK application in AWS.

{{% notice %}}
    npx cdk deploy
{{% /notice %}}
{{% notice warning%}}
Enter **Y** if was requested.
{{% /notice %}}

![Create CDK Infrastructure](/images/7.deployapp/7.5deployapp.png?pc=90pt)

4. After CDK application was deployed in AWS successfully, a CloudFormation name **CdkPipelineStack** will be created.
5. Go to [Stack of CloudFormation](https://ap-southeast-1.console.aws.amazon.com/cloudformation/home?region=ap-southeast-1#/stacks) to check the result.

![Create CDK Infrastructure](/images/7.deployapp/7.6deployapp.png?pc=90pt)

6. This CloudFormation stack will create an **Empty CodePipeline** name **MyServicePipeline**.
7. Go to [CodePipeline](https://ap-southeast-1.console.aws.amazon.com/codesuite/codepipeline/pipelines?region=ap-southeast-1) to see your pipeline.

![Create CDK Infrastructure](/images/7.deployapp/7.7deployapp.png?pc=90pt)

8. Click to **pipeline** to see stages.

![Create CDK Infrastructure](/images/7.deployapp/7.8deployapp.png?pc=90pt)

### Add a Deploy Stage for Elastic Beanstalk Environment
So far, we have provisioned an empty pipeline, and the pipeline isn’t deploying our web application yet. Now, we will create a stage to deploy application to Elastic Beanstalk.
1. Create a new file **lib/eb-stage.ts**.
2. Put the following code in it:

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

3. Add a new import line at the top of **lib/cdk-pipeline-stack.ts**.

{{% notice %}}
    import { CdkEBStage } from './eb-stage';
{{% /notice %}}

4. Add the following code after the mentioned comment.

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

5. Run **npm run build** and push source code to GitHub repository.

{{% notice %}}
    npm run build
    git add .
    git commit -m 'Add Pre-Prod stage'
    git push
{{% /notice %}}

![Create CDK Infrastructure](/images/7.deployapp/7.11deployapp.png?pc=90pt)

