---
title : "Build web application"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 3. </b> "
---
### Create the client app

1. Create new directory.

{{% notice %}}
    mkdir my_webapp
    cd my_webapp
{{% /notice %}}

2. Then we can initialize the Node.js project.

{{% notice %}}
    npm init -y
{{% /notice %}}

### Create Express app
1. Install **Express**.

{{% notice %}}
    npm install express
{{% /notice %}}

2. After running this command, we will see the dependency appear in the **package.json** file. Additionally, the **node_modules** directory and **package-lock.json** files are created.

![Build application](/images/3.buildapp/3.1buildapp.png?pc=90pt)

3. Create new file name **app.js**.

{{% notice %}}
    touch app.js
{{% /notice %}}

4. Select file **app.js** and paste this code. Then save it.

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

### Create a REST API

1. Add the following code in the `app.js` file.

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

### Create HTML content
1. Create file name **index.html**.

{{% notice %}}
    touch index.html
{{% /notice %}}

2. Select file **index.html**. Then paste the code below and save.

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

### Running the code locally
1. Select file **package.json**.
2. Update **package.json** with script and save.

{{% notice %}}
    "scripts": {
        "start": "node app.js"
    },
{{% /notice %}}

![Build application](/images/3.buildapp/3.5buildapp.png?pc=90pt)

3. In terminal, run command.

{{% notice %}}
    npm start
{{% /notice %}}

4. Go to the link that appear to see the result.

![Build application](/images/3.buildapp/3.6buildapp.png?pc=90pt)