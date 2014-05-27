---
layout: post
title: Heartbleed Bug
author: C. Cherry
published: 10-Apr-2014
description: Existant since December 31, 2011, the Heartbleed bug in the OpenSSL 1.0.1 Series up through 1.0.1f, as well as the 1.0.2-beta, exploits the heartbeat mechanism used by transport layer security (TLS) to keep secure connections alive.
category: Cyber Security, Cryptography
tags: Bug, Cryptography, Heartbleed, HTTPS, OpenSSL, Security
---

Earlier this week, ironically as I was writing an article about Always On SSL (AOSSL) and why every web application and site should use AOSSL, the Heartbleed bug in OpenSSL was announced. Existant since December 31, 2011, the Heartbleed bug in the OpenSSL 1.0.1 Series up through 1.0.1f, as well as the 1.0.2-beta, exploits the heartbeat mechanism used by Transport Layer Security (TLS) to keep secure connections alive. Discovered during routine testing by security researchers at Google and Codenomicon, the Heartbleed bug allows attackers to incrementally request up to 64KB of secure memory from an affected server, thus exposing everything from user credentials (i.e., usernames and passwords) to potentially a service’s private keys which would render the in-place encryption schema completely useless. As cryptographer and research professor at Johns Hopkins University, Matthew Green, describes the Heartbleed bug:

>The problem is fairly simple: there’s a tiny vulnerability — a simple missing bounds check — in the code that handles TLS ‘heartbeat’ messages. By abusing this mechanism, an attacker can request that a running TLS server hand over a relatively large slice (up to 64KB) of its private memory space. Since this is the same memory space where OpenSSL also stores the server’s private key material, an attacker can potentially obtain (a) long-term server private keys, (b) TLS session keys, (c) confidential data like passwords, (d) session ticket keys.[^1]

And this is not just theoretical. In discussing the Heartbleed bug and how they discovered it, Codenomicon posted on their official bug website that:

>We have tested some of our own services from attacker’s perspective. We attacked ourselves from outside, without leaving a trace. Without using any privileged information or credentials we were able steal from ourselves the secret keys used for our X.509 certificates, user names and passwords, instant messages, emails and business critical documents and communication.[^2]

So it becomes more clear why cryptography experts like Bruce Schneier are calling the Heartbleed bug catastrophic.

>“Catastrophic” is the right word. On the scale of 1 to 10, this is an 11.[^3]

With as much as two-thirds of the internet left vulnerable by the Heartbleed bug, it is difficult to see how anyone will not be directly or indirectly affected by this serious security flaw.[^4] That said, IT departments and webmasters have been working overtime to patch the Heartbleed bug since it was first announced. Upgrading to OpenSSL 1.0.1g or recompiling an older version of OpenSSL with the compile time option -DOPENSSL_NO_HEARTBEATS should sufficiently patch the flaw. However, that in and of itself is not enough to restore security.

SSL certificates will need to be reissued for all impacted servers, and importantly, new private keys will need to be generated. Simply replacing an SSL certificate will not suffice as it should be assumed that the Heartbleed bug exposed all private keys. In theory, all of the SSL certificates on impacted servers should be blacklisted in order to prevent hackers from masquerading as legitimate sites and services using intercepted private keys, a problem that will likely manifest over the coming weeks and months. Unfortunately, Certificate Authorities (CA), which provide the backbone of trust on the internet, are unlikely to do this given the amount of work that is involved, and the fact that the enormous blacklist would then have to be pushed out to all browsers, systems, and devices (not an inconsequential task).

Additionally, all impacted sites that use session keys and session cookies should invalidate them. This is in order to prevent session hijacking.

While the full impact of the Heartbleed bug is yet unknown, there are steps that everyone needs to take, including end-users, in order to minimize the impact of the flaw. As outlined above, webmasters and IT specialists need to do their part to secure their sites as quickly as possible. Unsure if your site is secure? Filippo Valsorda has created an application to test websites for Heartbleed.

In the meantime, if at all possible, you should avoid using the internet for any secure transactions (e.g., online banking, taxes, etc.) until a site in question has notified its users that it was either unaffected by the Heartbleed bug (the case for many servers not running on Apache or NGINX webservers), or has since taken the above outlined measures to fix the Heartbleed bug. Only then should you resume use of secure online applications, and the first thing you should do is change your password.

It is important to note that changing your password before a site has taken the appropriate measures to patch the Heartbleed bug will actually make you more vulnerable to attack as you are essentially handing your password to anyone that is eavesdropping. Given the publicity of the bug, and the simplicity and untraceable nature of its exploitation, it is certain that hackers are lying in wait. As a user, perhaps the bright side of the Heartbleed bug is that as you begin changing your passwords on various sites, it affords you the opportunity to reassess your own security. The use of password managers and strong passwords have become an imperative, and if you are not using them, now is a good time to start.

As the Heartbleed bug is further analyzed in the coming months, we will likely have some validation on just how severe this security flaw was (and is for those that are slow to implement corrective actions). Indeed, there is already evidence emerging that hackers, likely state intelligence agencies, knew about the flaw and had been exploiting it in the wild prior to its announcement on Monday.[^5] Scarily, even for hackers that were unaware of the flaw, if they had collected encrypted traffic, the Heartbleed bug allows for retrospective decryption absent the implementation of perfect forward security (PFS). This means that all of the encrypted traffic on previously vulnerable systems is potentially accessible. Yet another example of why all sites should be layering their security, and why web users should never consider anything transmitted over the internet truly safe.[^6]

###### The official reference for the Heartbleed bug is CVE-2014-0160. ######

### <center>Notes</center> ###

[^1]: Matthew Green, “Attack of the week: OpenSSL Heartbleed,” Cryptographic Engineering, April 8, 2014, accessed April 9, 2014, http://blog.cryptographyengineering.com/2014/04/attack-of-week-openssl-heartbleed.html.


[^2]: “The Heartbleed Bug,” Codenomicon, last modified April 10, 2014, accessed April 9, 2014, http://heartbleed.com/.


[^3]: Bruce Schneier, “Heartbleed,” Schneier on Security, April 9, 2014, accessed April 9, 2014, https://www.schneier.com/blog/archives/2014/04/heartbleed.html.


[^4]: Dan Goodin, “Critical crypto bug in OpenSSL opens two-thirds of the Web to eavesdropping,” Ars Technica, April 7, 2014, accessed April 7, 2014, http://arstechnica.com/security/2014/04/critical-crypto-bug-in-openssl-opens-two-thirds-of-the-web-to-eavesdropping/.


[^5]: Peter Eckersley, “Wild at Heart: Were Intelligence Agencies Using Heartbleed in November 2013?,” Electronic Frontier Foundation, April 10, 2014, accessed April 10, 2014, https://www.eff.org/deeplinks/2014/04/wild-heart-were-intelligence-agencies-using-heartbleed-november-2013.


[^6]: It is important to recognize that the Heartbleed bug is not a flaw in the SSL/TLS protocol, but rather a flaw in how it is implemented through affected OpenSSL versions. As with most flaws in encryption, it is not the math that is bad, but the security of its implementation by people through software.
