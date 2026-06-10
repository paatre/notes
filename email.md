# Email

## Simple Mail Transfer Protocol (SMTP)

```
                  +----------+                +----------+
      +------+    |          |                |          |
      | User |<-->|          |      SMTP      |          |
      +------+    |  Client- |Commands/Replies| Server-  |
      +------+    |   SMTP   |<-------------->|    SMTP  |    +------+
      | File |<-->|          |    and Mail    |          |<-->| File |
      |System|    |          |                |          |    |System|
      +------+    +----------+                +----------+    +------+
                   SMTP client                SMTP server
```

The objective of SMTP is to transfer mail reliably and efficiently. SMTP requires a reliable data stream, which, most of the time, but not always, is provided by the TCP protocol.

Multiple SMTP servers can be connected in an SMTP delivery network as a chain. This is called as "SMTP mail relaying" where one SMTP server forwards the mail to another SMTP server.
This means that the SMTP server that receives the mail is not always the one that delivers it to the recipient. When the SMPT server receives the mail, it check the recipient's domain,
and if the domain is not the same as the server's domain, it forwards the mail to the next SMTP server in the chain, working as a new SMPT client.

When an SMTP wants to send an email, it establishes a two-way transmission channel to an SMTP server. The transmission channel is established by the SMTP client connecting to the SMTP
server on port 25. After this, the client and the server complete a SMTP handshake.

This handshake involves multiple steps, where the client sends a command to the server, and the server replies with a reply code (normally "250 OK").

The commands in the SMTP protocol are:

1. `EHLO`/`HELO`: tells the SMTP-receiver what the client's domain name is.
1. `MAIL FROM`: tells the SMTP-receiver that a new mail transaction is starting and who the sender is. Sender is the email address of the person who is sending the email and therefore different than the SMTP client.
2. `RCPT TO`: tells the SMTP-receiver who the receiver party is.
3. `DATA`: tells the SMTP-receiver that the data (the email) is starting to which the server will reply with a 354 reply code. After the reply, the client sends the email data (Subject, From, To, content) line by line, and
   the email data ends with a line containing only a period.

### An example SMTP transaction

This SMTP example shows mail sent by Smith at host bar.com, and to
Jones, Green, and Brown at host foo.com.  Here we assume that host
bar.com contacts host foo.com directly.  The mail is accepted for
Jones and Brown.  Green does not have a mailbox at host foo.com.

```smtp
S: 220 foo.com Simple Mail Transfer Service Ready
C: EHLO bar.com
S: 250-foo.com greets bar.com
S: 250-8BITMIME
S: 250-SIZE
S: 250-DSN
S: 250 HELP
C: MAIL FROM:<Smith@bar.com>
S: 250 OK
C: RCPT TO:<Jones@foo.com>
S: 250 OK
C: RCPT TO:<Green@foo.com>
S: 550 No such user here
C: RCPT TO:<Brown@foo.com>
S: 250 OK
C: DATA
S: 354 Start mail input; end with <CRLF>.<CRLF>
C: Blah blah blah...
C: ...etc. etc. etc.
C: .
S: 250 OK
C: QUIT
S: 221 foo.com Service closing transmission channel
```

## Post Office Protocol version 3 (POP3)

TBW

## Internet Message Access Protocol (IMAP)

TBW

## Email authentication

### Sender Policy Framework (SPF)

SPF is a simple email-validation system designed to detect _email spoofing_ by providing a mechanism to allow receiving mail exchangers to check that incoming mail from a domain comes from a host authorized by that domain's administrators.

For example, without SPF, a spammer could send an email that appears to come from `example.com` but actually comes from `spammer.com`. The recipient's mail server would have no way to know that the email was not actually sent by `example.com`.

Before SPF, there were no restrictions on what a host could use the MAIL FROM address in an email and/or in the SMTP HELO/EHLO command. This allowed spammers to forge the "MAIL FROM" address in an email to make it appear as if it came from a different domain.

- [RFC 7208: Sender Policy Framework (SPF) for Authorizing Use of Domains in Email, Version 1](https://datatracker.ietf.org/doc/html/rfc7208)
- [What's is a DNS SPF record?](https://www.cloudflare.com/learning/dns/dns-records/dns-spf-record/)
- [Wikipedia: Email spoofing](https://en.wikipedia.org/wiki/Email_spoofing)
- [What is email spoofing?](https://www.cloudflare.com/learning/email-security/what-is-email-spoofing/)

### DomainKeys Identified Mail (DKIM)

RFC document: [RFC 6376](https://datatracker.ietf.org/doc/html/rfc6376)

### Domain-based Message Authentication, Reporting, and Conformance (DMARC)

RFC document: [RFC 7489](https://datatracker.ietf.org/doc/html/rfc7489)
