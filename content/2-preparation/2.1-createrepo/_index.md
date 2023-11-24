---
title : "Create GitHub Repository"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 2.1 </b> "
---
1. Go to [GitHub](https://github.com/) and log in.
2. Create new repository name ```FCJ-CICD-CDK-Beanstalk-Workshop```.

![Create Repository](/images/2.preparation/2.1createrepo/2.1.1createrepo.png?pc=90pt)
3. After repository created successfully, save its **HTTPS** link to use later.

![Create Repository](/images/2.preparation/2.1createrepo/2.1.2createrepo.png?pc=90pt)
4. Go to [GitHub Setting](https://github.com/settings/profile), click **Developer settings**.

![Create Repository](/images/2.preparation/2.1createrepo/2.1.3createrepo.png?pc=90pt)
5. At **Personal access tokens**, choose **Tokens (clasic)**.

![Access Token](/images/2.preparation/2.1createrepo/2.1.4accesstoken.png?pc=90pt)
6. Click **Generate new token**.
7. Click **Generate new token (classic)**.

![Access Token](/images/2.preparation/2.1createrepo/2.1.5accesstoken.png?pc=90pt)
8. Input ```Access token for FCJ Workshop``` at **Note**.

![Access Token](/images/2.preparation/2.1createrepo/2.1.6accesstoken.png?pc=90pt)
9. At **Scope** field, choose **repo** (to read the repository) and **admin:repo_hook** (if you plan to use webhooks).

![Access Token](/images/2.preparation/2.1createrepo/2.1.7accesstoken.png?pc=90pt)
10. Then, click **Generate token**.

![Access Token](/images/2.preparation/2.1createrepo/2.1.8accesstoken.png?pc=90pt)
11. Save the token to use later.

![Access Token](/images/2.preparation/2.1createrepo/2.1.9accesstoken.png?pc=90pt)