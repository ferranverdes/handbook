# Sender Policy Framework (SPF)

* Published as RFC 7208 dated April 2014.
* SPF enables recipients to validate the Mail Transfer Agent's identity.
* SPF will check if the MAIL FROM identity is authorized.

## SPF Workflow

1. Email received.
2. DNS server queried for TXT records of the MAIL FROM domain.
3. SPF records extracted from TXT records (if they were found).
4. SPF evaluation performed on the MAIL FROM identity (it may require additional DNS queries).
5. Email delivered or rejected based on results.

## What happens when MAIL FROM is null?

If MAIL FROM is null, SPF validates postmaster@[EHLO header value] instead.

## When SPF breaks

It doesn’t work when there is a forwarder between the Mail Transfer Agent and the final recipient. 

In this case, the MAIL FROM header needs to be modified during forwarding so then the SPF evaluation is based on the forwarder's domain and SPF record.

It means:

* The forwarder will have full control over the SPF evaluation.
* The forwarder will receive bounced emails and must take responsibility for forwarding them to the original sender.

## SPF weaknesses

* Vulnerable to network attacks (it assumes the IP and DNS addresses are correct): MiTM, IP Spoofing, DNS Spoofing, etc.
* Vulnerable to cloud sharing attacks (the attacker may share the same machine to send emails).
* Sensitive to email forwarding (return address cannot be preserved).

|Source: [*"Configuring and Managing SPF, DKIM, and DMARC" from Pluralsight*.][1]|
|:--:|

[1]: https://www.pluralsight.com/courses/configuring-managing-spf-dkim-dmarc
