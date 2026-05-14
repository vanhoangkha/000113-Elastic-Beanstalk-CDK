---
title : "Chuẩn bị môi trường"
date: 2024-01-01
weight : 2 
chapter : false
pre : " <b> 2.2 </b> "
---
1. Đi đến [Cloud9](https://ap-southeast-1.console.aws.amazon.com/cloud9control/home?region=ap-southeast-1#/product).
2. Nhấn **Create environment**.

![Create environment](/images/2.preparation/2.2setupenv/2.2.1cloud9.png?pc=90pt)

3. Nhập tên ```FCJ-Env```.

4. Nhập mô tả ```Cloud9 environment for FCJ Workshop```.

5. Tại **Environment type**, chọn **New EC2 instance**.

6. Tại **Instance type**, chọn **Additional instance types**.

7. Tại **Additional instance types**, chọn **t2.medium**.

![Create environment](/images/2.preparation/2.2setupenv/2.2.2cloud9.png?pc=90pt)

8. Kéo xuống.
9. Sau đó, nhấn **Create**.

10. Sau khi môi trường của bạn được tạo thành công, nhấn **Open** để truy cập môi trường.

![Create environment](/images/2.preparation/2.2setupenv/2.2.3cloud9.png?pc=90pt)
11. Trong terminal, kiểm tra phiên bản của **npm**, **cdk** và **node**.

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

12. Nâng cấp phiên bản của **npm** lên mới nhất.

{{% notice %}}
    npm install -g npm@latest
{{% /notice %}}

![Create environment](/images/2.preparation/2.2setupenv/2.2.5cloud9.png?pc=90pt)

13. Nâng cấp phiên bản của **cdk** lên mới nhất.

{{% notice %}}
    npm install --force -g aws-cdk@latest
{{% /notice %}}

![Create environment](/images/2.preparation/2.2setupenv/2.2.6cloud9.png?pc=90pt)
