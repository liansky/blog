---
title: 使用node发送E-mail
date: 2017-01-19 13:41:50
tags: node
categories: node
---

### 安装依赖
[nodemailer](https://github.com/nodemailer/nodemailer) 这个模块在github上star数量还是挺多的。
``` bash
  npm install nodemailer --save
```

### 配置主机、用户名、密码
我这里选用了163的邮箱，主机是smtp.163.com
qq邮箱要注意一下（在设置中开启SMTP服务才行），主机为smtp.qq.com

``` javascript
  var nodemailer = require('nodemailer');

  var transporter = nodemailer.createTransport({
      host: "smtp.163.com",   // 主机
      port: 465,              // SMTP 端口  不启用SSL 端口为25，否则为465
      secure: true,           // 使用 SSL
      auth: {
          user: 'admin@163.com',
          pass: '*******' 
      }
  });

```

### 配置邮件发送信息
这里要注意发件邮箱要与上面配置主机的邮箱一致

``` javascript
  var mailOptions = {
      from: 'admin@163.com',       // 发件地址
      to: 'test@163.com,test2@163.com',      // 收件列表
      subject: '邮件测试服务',          // 标题
      text: 'Hello world！',          // html、纯文本二选一
      // html: '<b>Hello world！</b>'    // html 内容
  };

```

### 发送邮件

``` javascript
transporter.sendMail(mailOptions, function(error, info){
    if(error){
        return console.log(error);
    }
    console.log('Message sent: ' + info.response);
});

```

### 更多信息
[nodemailer官网](https://nodemailer.com/)
[Nodemailer Well-Known](https://github.com/andris9/nodemailer-wellknown#supported-services)
