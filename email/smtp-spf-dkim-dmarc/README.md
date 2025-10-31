# SMTP, SPF, DKIM and DMARC

## Email spoofing

Email spoofing is a technique used in spam and phishing attacks to mislead users into believing that a message comes from a person or entity they know manipulating  the header "From" address to make it appear as if it was sent by someone else.

### How to detect a spoofed email?

There may be some clues:

* Mismatched "From" address and display name.
* "Reply-to" header that doesn't match the source.
* Message content that's out of the ordinary.

### Inbound Spoofing Attacks

* Email security controls: including cloud-based email systems intended to detect and block incoming emails that contain malicious links or attachments.
* Identity-based protections.
* Employee training and reporting.

### Outbound Email Impersonation

* Sender Policy Framework (SPF).
* DomainKeys Identified Mail (DKIM).
* Domain-based Message Authentication, Reporting and Conformance (DMARC).

|Source: [*"What is Email Spoofing & How to Stop Attackers From Posing as You" from Agari*.][1]|
|:--:|

[1]: https://www.agari.com/email-security-blog/email-spoofing-and-defense/
