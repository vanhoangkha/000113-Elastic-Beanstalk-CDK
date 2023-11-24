---
title : "Xây dựng ứng dụng web"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 3. </b> "
---
### Xây dựng ứng dụng web

1. Tạo đường dẫn mới.

{{% notice %}}
    mkdir my_webapp
    cd my_webapp
{{% /notice %}}

2. Sau đó khởi tạo dự án Node.js.

{{% notice %}}
    npm init -y
{{% /notice %}}

### Tạo ứng dụng Express
1. Tải **Express**.

{{% notice %}}
    npm install express
{{% /notice %}}

2. Sau khi chạy dòng lệnh này, bạn sẽ thấy dependency xuất hiện trong tệp **package.json**. Thêm vào đó, tập tin **node_modules**  và tệp **package-lock.json** được tạo.

![Build application](/images/3.buildapp/3.1buildapp.png?pc=90pt)

3. Tạo tệp mới tên **app.js**.

{{% notice %}}
    touch app.js
{{% /notice %}}

4. Chọn tệp **app.js** và dán đoạn code này. Sau đó lưu.

{{% notice %}}
    var express = require('express');
    var app = express();
    var fs = require('fs');
    var port = 8080;

    app.listen(port, function() {
    console.log('Server running at http://127.0.0.1:', port);
    });
{{% /notice %}}


![Build application](/images/3.buildapp/3.2buildapp.png?pc=90pt)

### Tạo một REST API

1. Thêm đoạn code dưới đây vào tệp `app.js`.

{{% notice %}}
    var express = require('express');
    var app = express();
    var fs = require('fs');
    var port = 8080;
    /*global html*/

    // New code
    app.get('/test', function (req, res) {
        res.send('the REST endpoint test run!');
    });

    app.get('/', function (req, res) {
    html = fs.readFileSync('index.html');
    res.writeHead(200);
    res.write(html);
    res.end();
    });

    app.listen(port, function() {
    console.log('Server running at http://127.0.0.1:%s', port);
    });
{{% /notice %}}

### Xây dựng nội dung HTML
1. Tạo tệp tên **index.html**.

{{% notice %}}
    touch index.html
{{% /notice %}}

2. Chọn tệp **index.html**. Sau đó dán đoạn code bên dưới vào và lưu.

{{% notice %}}
    <html>
        <head>
            <title>Elastic Beanstalk App</title>
        </head>

        <body>
            <h1>Welcome to the FCJ Workshop</h1>
            <a href="/test">Call the test API</a>
        </body>
    </html>
{{% /notice %}}

![Build application](/images/3.buildapp/3.4buildapp.png?pc=90pt)

### Chạy thử đoạn code
1. Chọn tệp **package.json**.
2. Thay đổi nội dung tệp **package.json** và lưu.

{{% notice %}}
    "scripts": {
        "start": "node app.js"
    },
{{% /notice %}}

![Build application](/images/3.buildapp/3.5buildapp.png?pc=90pt)

3. Tại terminal, chạy lệnh.

{{% notice %}}
    npm start
{{% /notice %}}

4. Đi đến đường dẫn xuất hiện để thấy kết quả.

![Build application](/images/3.buildapp/3.6buildapp.png?pc=90pt)