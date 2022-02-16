---
layout: post
title: "jenkins email config"
subtitle:   “501 mail from address must be same as authorization user”
date:       2017-12-15 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-05.jpg"
---
Jenkins的邮件设置

~~~bash
Failed to send out e-mail

com.sun.mail.smtp.SMTPSendFailedException: 501 mail from address must be same as authorization user;

nested exception is:

com.sun.mail.smtp.SMTPSenderFailedException: 501 mail from address must be same as authorization user

at com.sun.mail.smtp.SMTPTransport.issueSendCommand(SMTPTransport.java:2057)

at com.sun.mail.smtp.SMTPTransport.mailFrom(SMTPTransport.java:1580)

at com.sun.mail.smtp.SMTPTransport.sendMessage(SMTPTransport.java:1097)

at javax.mail.Transport.send0(Transport.java:195)

at javax.mail.Transport.send(Transport.java:124)

at hudson.tasks.Mailer$DescriptorImpl.doSendTestMail(Mailer.java:522)

………………………………….

Caused by: com.sun.mail.smtp.SMTPSenderFailedException: 501 mail from address must be same as authorization user at com.sun.mail.smtp.SMTPTransport.mailFrom(SMTPTransport.java:1587)

        ... 62 more

~~~


这个错的意思是，你的邮箱必须和Jenkins系统管理员的预留地址一致。
<br>

Smtp server登陆的邮箱必须和 Jenkins 系统管理员邮件地址 一样。


