Attacks: [[Open Relay ]]
Port: 25, 587 (newer TCP), 465 (+TLS)

Usually uses [[STARTTLS]] to encrypt

## Anti-Spam
With [[ESMPT]]+[[SMTP-Auth]] 

## Dangerous setting
If SMTP server has this config
```
mynetworks = 0.0.0.0/0
```
It can send fake emails and thus initialize communication between multiple parties. Another attack possibility would be to spoof the email and read it.

### User enum
Request might timeout so increase timeout period

## Tools
- [[Evolution]]