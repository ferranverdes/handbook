# DomainKeys Identified Mail (DKIM)

* Published as RFC 6376 dated September 2011 with updates in RFC 8301 and RFC 8463.
* The SMTP server configured on the email client it's called Mail Submission Agent.
* In general, an author must first authenticate against a mail submission agent (using username and password, NTLM, etc). If the mailing agent can confirm its identity and it's configured to do so, it will be able to sign the email on behalf of the author.
* The dkim signature is added as soon as the mail is received by the mail submission agent:

![DKIM Signature][2]

|Source: [*"DomainKeys Identified Mail (DKIM) Signatures - RFC 6376" from IETF Datatracker*.][7]|
|:--:|

## DKIM workflow

1. Email received.
2. DKIM signature email headers injected at the beginning of the email data.
3. Email transferred (additional DKIM signatures might be added by forwarders).
4. DKIM evaluation performed (public keys retrieved from signers'DNS TXT records).
5. Email delivered or rejected based on results.

## DKIM evaluation results

* **Success**: signature verified.
* **Permfail**: signature found but invalid.
* **Tempfail**: transient error (e.g. DNS query timeout).

## DKIM signature tags

* **v**: version (must be 1).
* **a**: cryptographic algorithm.
* **b**: signature in Base64.
* **bh**: email hash in Base64.
* **d**: signer domain.
* **h**: list of protected header fields, in order of appearance.
* **s**: domain key selector.
* **c**: message canonicalization.
* **l**: protected body length (it allows footer injection by forwarders).

## Domain key selector + signer domain

* s=google
* d=example.com

The public key is expected to exist as a TXT record under the domain name:

```shell
google._domainkey.example.com
```

### Selector format

```shell
[selector]._domainkey
```

## How to know if a domain has DKIM enabled?

There is no way to be sure without a domain key selector.

The same domain can have several keys for different purposes and the selector can be anything, e.g. selector1.\_domainkey or whatever.\_domainkey and it's only introduced in the flag s=selectorname of DKIM-Signature header.

If the sender has also implemented DMARC, it is a clear indication that there should be both DKIM and SPF. Therefore:

* If a domain has \_dmarc.example.com. TXT there should be DKIM.
* If there is no \_dmarc.example.com TXT record, they may or may not have DKIM enabled.

DMARC Check Tools:

* [MXToolBox DMARC Check Tool][4]

|Source: [*"How to find out if a domain is using DKIM" from serverfault*.][3]|
|:--:|

## DKIM weaknesses

* Vulnerable to network attacks (it assumes the DNS addresses are correct): MiTM, DNS Spoofing, etc.
* No failure state (lack of signature proves nothing).

## Further reading

* [Email forwarding and DMARC DKIM SPF][5]
* [How to get email forwarders DMARC compliant?][6]

|Source: [*"Configuring and Managing SPF, DKIM, and DMARC" from Pluralsight*.][1]|
|:--:|

[1]: https://www.pluralsight.com/courses/configuring-managing-spf-dkim-dmarc
[2]: ./images/rfc-6376-the-email-is-signed.png
[3]: https://serverfault.com/questions/849198/how-to-find-out-if-a-domain-is-using-dkim
[4]: https://mxtoolbox.com/DMARC.aspx
[5]: https://easydmarc.com/blog/email-forwarding-and-dmarc-dkim-spf/
[6]: https://www.dmarcanalyzer.com/email-forwarders-dmarc/
[7]: https://datatracker.ietf.org/doc/html/rfc6376
