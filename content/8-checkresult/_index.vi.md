---
title : "Kiểm tra kết quả"
date: 2024-01-01
weight : 8 
chapter : false
pre : " <b> 8. </b> "
---
### Kiểm tra kết quả
Tại bảng điều khiển CodePipeline, sau khi giai đoạn UpdatePipeline chọn code mới cho giai đoạn bổ sung, nó sẽ tự thay đổi và thêm 2 giai đoạn mới, một cho Assets và Pre-Prod. Sau khi giai đoạn UpdatePipeline hoàn tất thành công, quy trình sẽ chạy lại từ đầu. Lần này nó sẽ không dừng lại ở giai đoạn UpdatePipeline. Nó sẽ chuyển tiếp sang các giai đoạn mới Assets và Pre-prod để triển khai ứng dụng, môi trường Beanstalk và ứng dụng MyWebApp.

1. Đi đến [CodePipeline](https://ap-southeast-1.console.aws.amazon.com/codesuite/codepipeline/pipelines?region=ap-southeast-1). Các giai đoạn Assets và Pre-prod sẽ được thêm vào khi giai đoạn UpdatePipeline được xây dựng thành công.

![Verify the result](/images/8.checkresult/8.1checkresult.png?pc=90pt)

2. Sau khi tất cả các giai đoạn trong CodePipeline được xây dựng, hai CloudFormation stacks được tạo.
    + Stack đầu tiên tên **Pre-Prod-WebService** để tạo Elastic Beanstalk Application và Environment.
    + Stack thứ hai tên **awseb-e-xxxxxxxxxx-stack** để tạo Auto Scaling Group và Load Balancer cho Elastic Beanstalk Environment.
Đi đến [Auto Scaling Group](https://ap-southeast-1.console.aws.amazon.com/ec2/home?region=ap-southeast-1#AutoScalingGroups:) và [Load Balancer](https://ap-southeast-1.console.aws.amazon.com/ec2/home?region=ap-southeast-1#LoadBalancers) để kiểm tra điều đó.

3. Đi đến [Elastic Beanstalk](https://ap-southeast-1.console.aws.amazon.com/elasticbeanstalk/home?region=ap-southeast-1#/applications) để kiểm tra kết quả. 
4. Nhấn vào ứng dụng tên **MyWebApp**.
5. Nhấn vào môi trường **MyWebAppEnvironment**.
6. Nhấn **Domain** của môi trường để thấy kết quả của ứng dụng.
![Verify the result](/images/8.checkresult/8.2checkresult.png?pc=90pt)
![Verify the result](/images/8.checkresult/8.3checkresult.png?pc=90pt)
![Verify the result](/images/8.checkresult/8.4checkresult.png?pc=90pt)
![Verify the result](/images/8.checkresult/8.5checkresult.png?pc=90pt)

***Xin chúc mừng, CDK Pipeline của bạn đã được tạo thành công***.

### Cập nhật triển khai ứng dụng Node.js
1. Tại ./src/index.html, tạo sự thay đổi và lưu lại. Ví dụ: Thay thế **Welcome to the FCJ Workshop** thành ```Welcome to the FCJ Workshop, this is my first project!!!```.

2. Đẩy code của bạn lên GitHub repository.

{{% notice %}}
    git add . && git commit -am 'Changing index.html' && git push
{{% /notice %}}
![Verify the result](/images/8.checkresult/8.7checkresult.png?pc=90pt)

3. Đi đến [CodePipeline](https://ap-southeast-1.console.aws.amazon.com/codesuite/codepipeline/pipelines?region=ap-southeast-1), bạn sẽ thấy commit id với code mà bạn đã thay đổi đang được xây dựng.

![Verify the result](/images/8.checkresult/8.9checkresult.png?pc=90pt)

4. Khi tất cả các giai đoạn hoàn tất, tải lại đường dẫn của Elastic Beanstalk Environment để thấy kết quả.

![Verify the result](/images/8.checkresult/8.10checkresult.png?pc=90pt)

{{% notice info%}}
**Xin chúc mừng, CDK Pipeline của bạn đã triển khai phiên bản ứng dụng mà bạn đã thay đổi**.
{{% /notice %}}