# Domain-based Message Authentication, Reporting and Conformance (DMARC)

* Published as RFC 7489 dated March 2015.

## What is DMARC?

* It's an email validation system designed to protect your company's email domain from being used for email spoofing, phishing scams and other cybercrimes.
* It enables recipients to validate the author's domain.
* It extends two existing email authentication mechanisms, Sender Policy Framework (SPF) and DomainKeys Identified Mail (DKIM).

## Problems to be solved by DMARC

* In a forwarding chain context, the SMTP servers involved can verify and aggregate their results from the SPF and DKIM checks performed, but the question is, have the SPF and DKIM checks of the original domain passed?

## SPF Alignment

* It occurs when the `From` field, which identifies the author's of the email, and the `MAIL FROM` field, which contains the Mail Transfer Agent's domain, are related.

## DKIM Alignment

* It occurs when the `From` field,  which identifies the author's of the email, and the DKIM signer domain used, which can be found in the `d` DKIM signature tag, are related.

## SPF & DKIM alignments modes

* Relaxed: domains must share the same root domain (office.example.com vs portal.example.com or example.com would be fine).
* Strict: both domains must exactly match.

## DMARC evaluation results

* Pass: alignment found with a passing SPF result or a successful DKIM result. Only one of them is required.
* Fail: no alignment found with any passing SPF result or any successful DKIM result.
* DMARC enables administrators to receive external SPF, DKIM and DMARC evaluation results.

## DMARC tags for the TXT record

### Policy

* `p`: dictates the action to be taken by the recipient in case of failure.
  * `none`: ignores failures. No further actions are taken.
  * `quarantine`: flag it as spam/suspicious.
  * `reject`: do not deliver it.

### Alignment

* `adkim`: DKIM alignment mode.
  * `r`: relaxed (default).
  * `s`: strict.
* `aspf`: SPF alignment mode.
  * `r`: relaxed (default).
  * `s`: strict.

### Reporting

* `ri`: time interval between aggregate reports, in seconds.
* `rua`: reporting URIs to send aggregate feedback reports.
* `ruf`: reporting URIs to send failure reports. WARNING (read below).

## DMARC TXT records examples

```
v=DMARC1; p=quarantine; aspf=s; adkim=s; rua=mailto:reports@vulnerable.es;
```

## DMARC workflow

1. Email received.
2. DNS server queried for TXT records related to the From field's domain. The process terminates if no DMARC record can be extracted among them.
3. SPF and DKIM evaluations performed.
4. Alignments checked (it must have passed SPF or been successful DKIM).
5. Email delivered to the recipient or rejected based on results and the discovered policy.

At this point, the SMTP server can check for other local policies, such as making sure it's not coming from a blacklisted server or classifying the mail differently than the suggestion made by DMARC policy specified on the sender's DNS server.

## DMARC Reporting Types

* These reports may come from anywhere in the world.

### Aggregate Feedback Report

It contains DMARC related data for a specific period of time.

* Multiple emails per report.
* Triggered at a regular, configurable interval. The recipient must save the results locally until the next reporting period.
* Identifies emails by timestamps and data required for DMARC evaluation.
* Uses XML 1.0 format.
* Can be compressed using gzip.
* Includes policy override reasons (e.g. local policy).

|Source: [*"DMARC aggregate reports explained" from URIports*.][1]|
|:--:|

### Failure Report

It contains forensic data for an email that failed DMARC.

* One email per report.
* Triggered instantaneously when a DMARC failure occurs.
* Identify emails by forwarding original email data (which can contain absolutely anything, including personal information and this can violate laws and contracts).
* Uses Abuse Reporting Format (ARF) format.

Make sure that your email server does not send failure reports.

## DMARC weaknesses

* Inherits SPF and DKIM vulnerabilities.
* `From` field local part not validated.

## Troubleshooting

* [Why do I receive a DMARC report for successful delivery?][2]

```xml
<record>
    <row>
      <source_ip>209.85.220.41</source_ip>
      <count>1</count>
      <policy_evaluated>
        <disposition>none</disposition>
        <dkim>pass</dkim>
        <spf>pass</spf>
      </policy_evaluated>
    </row>
    <identifiers>
      <header_from>example.com</header_from>
    </identifiers>
    <auth_results>
      <dkim>
        <domain>example.com</domain>
        <result>pass</result>
        <selector>google</selector>
      </dkim>
      <spf>
        <domain>example.com</domain>
        <result>pass</result>
      </spf>
    </auth_results>
  </record>
```

## Further reading

* [Frequently asked questions about DMARC][3]
* [Actually, DMARC works fine with mailing lists][4]

|Source: [*"Configuring and Managing SPF, DKIM, and DMARC" from Pluralsight*.][5]|
|:--:|

[1]: https://www.uriports.com/blog/dmarc-aggregate-reports-explained/
[2]: https://stackoverflow.com/questions/30342550/why-do-i-receive-a-dmarc-report-everyday
[3]: https://dmarc.org/wiki/FAQ
[4]: https://begriffs.com/posts/2018-09-18-dmarc-mailing-list.html
[5]: https://www.pluralsight.com/courses/configuring-managing-spf-dkim-dmarc
