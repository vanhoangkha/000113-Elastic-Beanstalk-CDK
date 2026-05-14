---
title : "Set up environment"
date: 2024-01-01
weight : 2 
chapter : false
pre : " <b> 2.2 </b> "
---
1. Go to [Cloud9](https://ap-southeast-1.console.aws.amazon.com/cloud9control/home?region=ap-southeast-1#/product).
2. Click **Create environment**.

![Create environment](/images/2.preparation/2.2setupenv/2.2.1cloud9.png?pc=90pt)

3. Input name ```FCJ-Env```.

4. Input description ```Cloud9 environment for FCJ Workshop```.

5. At **Environment type**, choose **New EC2 instance**.

6. At **Instance type**, choose **Additional instance types**.

7. At **Additional instance types**, choose **t2.medium**.

![Create environment](/images/2.preparation/2.2setupenv/2.2.2cloud9.png?pc=90pt)

8. Scroll down.
9. Then, click **Create**.

10. After your environment is created successfully, click **Open** to access to environment.

![Create environment](/images/2.preparation/2.2setupenv/2.2.3cloud9.png?pc=90pt)
11. In the terminal, check **npm**, **cdk** and **node** version.

{{% notice %}}
    npm --version
{{% /notice %}}
{{% notice %}}
    cdk --version
{{% /notice %}}
{{% notice %}}
    node --version
{{% /notice %}}

![Create environment](/images/2.preparation/2.2setupenv/2.2.4cloud9.png?pc=90pt)

12. Upgrade **npm** version to latest.

{{% notice %}}
    npm install -g npm@latest
{{% /notice %}}

![Create environment](/images/2.preparation/2.2setupenv/2.2.5cloud9.png?pc=90pt)

13. Upgrade **cdk** version to latest.

{{% notice %}}
    npm install --force -g aws-cdk@latest
{{% /notice %}}

![Create environment](/images/2.preparation/2.2setupenv/2.2.6cloud9.png?pc=90pt)
