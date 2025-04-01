---
title: "Route53で設定されているドメインに対してCertbotで証明書を発行する"
emoji: "🔒"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["certbot", "route53", "aws", "ssl", "conoha"]
published: true
publication_name: "gmomedia"
---

# 
![](/images/certbot-route53/1.png =350x)


```
sudo apt-get update
sudo apt-get install -y certbot python3-certbot-dns-route53


certbot certonly --dns-route53 -d pf-assessment.intra.gmo-media.jp


root@pf-assessment:~# certbot certonly --dns-route53 -d pf-assessment.intra.gmo-media.jp
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Enter email address (used for urgent renewal and security notices)
 (Enter 'c' to cancel): it-strategy@gmo-media.jp

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please read the Terms of Service at
https://letsencrypt.org/documents/LE-SA-v1.5-February-24-2025.pdf. You must
agree in order to register with the ACME server. Do you agree?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: Y

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing, once your first certificate is successfully issued, to
share your email address with the Electronic Frontier Foundation, a founding
partner of the Let's Encrypt project and the non-profit organization that
develops Certbot? We'd like to send you email about our work encrypting the web,
EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: N
Account registered.
Requesting a certificate for pf-assessment.intra.gmo-media.jp