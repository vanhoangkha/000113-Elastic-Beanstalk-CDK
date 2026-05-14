---
title : "Triển khai ứng dụng web với Elasitc Beanstalk và CDK Pipeline"
date: 2024-01-01
weight : 1 
chapter : false
pre : " <b> 1 </b> "
---
# Triển khai ứng dụng web với Elasitc Beanstalk và CDK Pipeline
### Tổng quan
Trong bài thực hành này, chúng ta sẽ khám phá làm thế nào để tạo đường ống CICD bằng CDK để triển khai một ứng dụng lên Elastic Beanstalk một cách tự động.
![Architect](/images/arch.png?pc=90pt)

### Nội dung
1. [Giới thiệu](./1-introduction/)
2. [Chuẩn bị](./2-preparation/)
    - 2.1 [Tạo GitHub Repository](./2-preparation/2.1-createrepo/)
    - 2.2 [Chuẩn bị môi trường](./2-preparation/2.2-setupenv/)
3. [Xây dựng ứng dụng web](./3-buildapp/)
4. [Tạo hạ tầng bằng CDK](./4-createinfrausingcdk/)
5. [Thêm Elastic BeanStalk vào CDK](./5-addbeanstalkcdk/)
6. [Tạo CDK Pipeline stack](./6-createcdkpipelinestack/)
7. [Triển khai ứng dụng](./7-deployapp/)
8. [Kiểm tra kết quả](./8-checkresult/)
9. [Dọn dẹp tài nguuyên](./9-cleanup/)