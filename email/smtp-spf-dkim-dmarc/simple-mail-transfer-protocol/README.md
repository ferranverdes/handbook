# Simple Mail Transfer Protocol (SMTP)

* The SMTP protocol adds the most recent header entries on top.

## Sending an email using Telnet

```
telnet smtp.destination.com 25
```

```shell
EHLO origin.com
MAIL FROM: john.doe@origin.com
RCPT TO: richard.roe@destination.com
DATA
Date: Tue, 13 Apr 2021 15:00:00 +0000
From: John Doe <john.doe@origin.com>
Subject: Can we meet today?
To: richard.roe@destination.com

Hi Richard,

Can we meet this afternoon at 5pm?

Cheers,
John
.
QUIT
```

## Email headers

Header lines identify particular routing information of the message, including the sender, recipient, date and subject. Some headers are mandatory, such as the From, To and Date headers. Others are optional, but very commonly used, such as Subject and CC.

|Source: [*"What is an Email Header?" from WhatIsMyIPAddress*.][1]|
|:--:|

### HELO & EHLO

The SMTP HELO clause is the stage of the SMTP protocol where a SMTP server introduce them selves to each other. The sending server will identify who it is and the receiving server will (as per RFC) accept any given name. There is no requirement to give the correct information at this stage.

EHLO is the Enhanced SMTP (ESMTP) version of HELO. ESMTP adds a number of essential additions to the SMTP protocol.

|Source: [*"What is the SMTP HELO/EHLO clause?" from GMS*.][2]|
|:--:|

### MAIL FROM (a.k.a Return-Path)

Specifies the return address in case of delivery failure or other fatal error.

The return path may also be referred to as bounce address, reverse path, envelope from, envelope sender and MAIL FROM.

#### Further reading

* [SPF shortcomings with Return-Path in spoofed emails][4]

#### MAIL FROM vs From?

From:

* It's the author of the email, the person who has written the email.
* It's what you see when opening an email.

MAIL FROM:

* It's the return address in case of delivery failure.
* Can be empty. This is the case when the email in question is a delivery failure email.

### DATA

Everything in the data section will be visible to the recipient.

|Source: [*"Configuring and Managing SPF, DKIM, and DMARC" from Pluralsight*.][3]|
|:--:|

[1]: https://whatismyipaddress.com/email-header
[2]: https://www.gordano.com/knowledge-base/what-is-the-smtp-heloehlo-clause/
[3]: https://www.pluralsight.com/courses/configuring-managing-spf-dkim-dmarc
[4]: https://securepractice.co/blog/spf-shortcomings-with-return-path-in-spoofed-emails
