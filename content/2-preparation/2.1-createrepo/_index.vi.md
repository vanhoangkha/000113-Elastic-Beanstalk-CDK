---
title : "Tạo GitHub Repository"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---
1. Đi đến [GitHub](https://github.com/) và đăng nhập.
2. Tạo một repo tên ```FCJ-CICD-CDK-Beanstalk-Workshop```.

![Create Repository](/images/2.preparation/2.1createrepo/2.1.1createrepo.png?pc=90pt)
3. Sau khi repo được tạo thành công , lưu lại đường dẫn **HTTPS** để sử dụng sau.

![Create Repository](/images/2.preparation/2.1createrepo/2.1.2createrepo.png?pc=90pt)
4. Đi đến [GitHub Setting](https://github.com/settings/profile), nhấn **Developer settings**.

![Create Repository](/images/2.preparation/2.1createrepo/2.1.3createrepo.png?pc=90pt)
5. Tại **Personal access tokens**, chọn **Tokens (clasic)**.

![Access Token](/images/2.preparation/2.1createrepo/2.1.4accesstoken.png?pc=90pt)

6. Nhấn **Generate new token**.
7. Nhấn **Generate new token (classic)**.

![Access Token](/images/2.preparation/2.1createrepo/2.1.5accesstoken.png?pc=90pt)
8. Nhập ```Access token for FCJ Workshop``` tại **Note**.

![Access Token](/images/2.preparation/2.1createrepo/2.1.6accesstoken.png?pc=90pt)
9. Tại **Scope**, chọn **repo** (để đọc repository) và **admin:repo_hook** (nếu bạn có kể hoạch sử dụng webhooks).

![Access Token](/images/2.preparation/2.1createrepo/2.1.7accesstoken.png?pc=90pt)
10. Sau đó, nhấn **Generate token**.

![Access Token](/images/2.preparation/2.1createrepo/2.1.8accesstoken.png?pc=90pt)
11. Lưu lại token để sử dụng sau.

![Access Token](/images/2.preparation/2.1createrepo/2.1.9accesstoken.png?pc=90pt)