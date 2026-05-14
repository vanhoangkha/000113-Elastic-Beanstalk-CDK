---
title : "Check result"
date: 2024-01-01
weight : 8 
chapter : false
pre : " <b> 8. </b> "
---

### Verify the result
In CodePipeline console, once the UpdatePipeline stage picks up new code for an additional stage, it will self-mutate and add 2 new stages, one for the Assets and another for Pre-Prod. Once the UpdatePipeline stage has completed successfully, the pipeline will again run from start. This time it will not stop at UpdatePipeline stage. It will transition further to the new stages Assets and Pre-prod to deploy the Beanstalk application, environment and the MyWebApp application.

1. Go to [CodePipeline](https://ap-southeast-1.console.aws.amazon.com/codesuite/codepipeline/pipelines?region=ap-southeast-1). The new stages Assets and Pre-prod will be added when UpdatePipeline stage is built successfully.

![Verify the result](/images/8.checkresult/8.1checkresult.png?pc=90pt)

2. After all of stages in CodePipeline were built, two CloudFormation stacks are created.
    + The first stack name **Pre-Prod-WebService** to create Elastic Beanstalk Application and Environment.
    + The second stack name **awseb-e-xxxxxxxxxx-stack** to create Auto Scaling Group and Load Balancer for Elastic Beanstalk Environment.
Go to [Auto Scaling Group](https://ap-southeast-1.console.aws.amazon.com/ec2/home?region=ap-southeast-1#AutoScalingGroups:) and [Load Balancer](https://ap-southeast-1.console.aws.amazon.com/ec2/home?region=ap-southeast-1#LoadBalancers) to verify that.

3. Go to [Elastic Beanstalk](https://ap-southeast-1.console.aws.amazon.com/elasticbeanstalk/home?region=ap-southeast-1#/applications) to verify the result. 
4. Click to application name **MyWebApp**.
5. Click **MyWebAppEnvironment** environment.
6. Click **Domain** of environment to see the result of your application.
![Verify the result](/images/8.checkresult/8.2checkresult.png?pc=90pt)
![Verify the result](/images/8.checkresult/8.3checkresult.png?pc=90pt)
![Verify the result](/images/8.checkresult/8.4checkresult.png?pc=90pt)
![Verify the result](/images/8.checkresult/8.5checkresult.png?pc=90pt)

***Congratulation, your CDK Pipeline was created successfully***.

### Update the Node.js Application Deployment
1. At ./src/index.html, make the changes you want and save. Ex: Replace **Welcome to the FCJ Workshop** to ```Welcome to the FCJ Workshop, this is my first project!!!```.

2. Push your source code to GitHub repository.

{{% notice %}}
    git add . && git commit -am 'Changing index.html' && git push
{{% /notice %}}
![Verify the result](/images/8.checkresult/8.7checkresult.png?pc=90pt)

3. Go to [CodePipeline](https://ap-southeast-1.console.aws.amazon.com/codesuite/codepipeline/pipelines?region=ap-southeast-1), you will see the commit id with source code which you took the changes, is built.

![Verify the result](/images/8.checkresult/8.9checkresult.png?pc=90pt)

4. When all stages are done, reload the domain of Elastic Beanstalk Environment to see the result.

![Verify the result](/images/8.checkresult/8.10checkresult.png?pc=90pt)

{{% notice info%}}
**Congratulation, your CDK Pipeline deployed your changing application**.
{{% /notice %}}