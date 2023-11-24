---
title : "Create CICD Pipeline Stack"
date :  "`r Sys.Date()`" 
weight : 6 
chapter : false
pre : " <b> 6. </b> "
---
### Defining an Empty Pipeline
For this step, we are only creating these predefined stages - Source, Build and UpdatePipelineand hence it is an empty pipeline. In the next section, we will add stages (PublishAssets, Stage1) and actions to it to suit the needs of our application.

1. Create new file **lib/cdk-pipeline-stack.ts**. 
2. Replace **OWNER** and **REPO** in the code below:

{{% notice %}}
    import { CodePipeline, CodePipelineSource, ShellStep } from 'aws-cdk-lib/pipelines';
    import { Construct } from 'constructs';
    import {  Stack, StackProps } from 'aws-cdk-lib';

    /**
    * The stack that defines the application pipeline
    */
    export class CdkPipelineStack extends Stack {
    constructor(scope: Construct, id: string, props?: StackProps) {
        super(scope, id, props);

        const pipeline = new CodePipeline(this, 'Pipeline', {
        // The pipeline name
        pipelineName: 'MyServicePipeline',

        // How it will be built and synthesized
        synth: new ShellStep('Synth', {
            // Where the source can be found
            input: CodePipelineSource.gitHub('OWNER/REPO', 'main'),   
            // Build and run cdk synth
            commands: [
            'npm ci',
            'npm run build',
            'npx cdk synth'
            ],
        }),
        });

        // This is where we add the application stages
    }
    }

{{% /notice %}}

![Create CDK Infrastructure](/images/6.cicdpipeline/6.1addfile.png?pc=90pt)

The code defines the following basic properties of the pipeline:
+ Name for the pipeline.
+ Where to find the source in GitHub. This is Source stage. Every time we push new commits to this repo, the pipeline is triggered.
+ How to do the build and synthesis. For this use case, the Build stage will install a standard NPM build (this type of build runs npm run build followed by npx cdk synth).

3. Put the following code in **bin/cdk-pipeline-eb-demo.ts**.
4. Replace **ACCOUNT** and the **REGION** in there.

{{% notice %}}
    #!/usr/bin/env node
    import 'source-map-support/register';
    import * as cdk from 'aws-cdk-lib';
    import { CdkPipelineStack } from '../lib/cdk-pipeline-stack';

    const app = new cdk.App();

    new CdkPipelineStack(app, 'CdkPipelineStack', {
    env: { account: 'ACCOUNT', region: 'REGION' },
    });

    app.synth();
{{% /notice %}}
![Create CDK Infrastructure](/images/6.cicdpipeline/6.2accountid.png?pc=90pt)

{{% notice info%}}
Replace **ACCOUNT** with your Account ID and **REGION** with Region ID, ex: ```ap-southeast-1``` for region Singapore.
{{% /notice %}}

Copy your account ID here.
![Create CDK Infrastructure](/images/6.cicdpipeline/6.3accountid.png?pc=90pt)

5. Add the following to our cdk.json file in the "context" section, add a comma accordingly:

{{% notice %}}
    {
    ...
    "context": {
        "@aws-cdk/core:newStyleStackSynthesis": true
    }
    }

{{% /notice %}}
![Create CDK Infrastructure](/images/6.cicdpipeline/6.4cdkjson.png?pc=90pt)

### Connect GitHub to CodePipelines
For AWS CodePipeline to read from this GitHub repo, we also need to configure the GitHub personal access token we created earlier.
This token should be stored as a plaintext secret (not a JSON secret) in AWS Secrets Manager under the exact name github-token.
1. Copy this command.

{{% notice %}}
    aws secretsmanager  create-secret --name github-token --description "Github access token for cdk" --secret-string GITHUB_ACCESS_TOKEN --region REGION
{{% /notice %}}

2. Replace **GITHUB_ACCESS_TOKEN** with your plaintext secret had been created at [2.1 Create GitHub repository](../2-preparation/2.1-createrepo/).
3. Replace **REGION** with your Region ID, ex: ```ap-southeast-1```.
4. Paste and run it at terminal of your Cloud9 environment.
![Create CDK Infrastructure](/images/6.cicdpipeline/6.5accesstoken.png?pc=90pt)