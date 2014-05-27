---
layout: post
title: Why Every Website Should Use Always On SSL (AOSSL)
author: C. Cherry
published: 11-Apr-2014
description: This article explains that while serving websites over Hypertext Transfer Protocol Secure (HTTPS) does not eliminate all sources of insecurity (e.g., the recently discovered Heartbleed bug) and indeed exacts a latency penalty on the connection between the webserver and the client, the advantages of HTTPS and establishing Always On SSL (AOSSL), even for nonsensitive transactional websites, far exceed the disadvantages.
category: Cyber Security, Cryptography
tags: AOSSL, Cryptography, Cyber Security, HTTPS, SSL, SSL Certificate, Website Security
---

# {{ page.title }} #

##### 11-Apr-2014 #####

A few weeks prior to SecBrief.org’s launch, I found myself in a discussion with a colleague about my plans for the site’s web-stack, as well as best practices and security considerations for modern websites and web applications. As you might imagine, this conversation went in a lot of different directions and was full of differing opinions. However, when asked why SecBrief.org needed a Secure Socket Layer (SSL) Certificate to secure a website which does not engage in e-commerce or collect sensitive user information, it was demonstrative of some of the widespread assumptions and misconceptions that people have about the internet, security, and online privacy.

In an attempt to address some of these issues, this article explains that while serving websites over Hypertext Transfer Protocol Secure (HTTPS) does not eliminate all sources of insecurity (e.g., the recently discovered Heartbleed bug) and indeed exacts a latency penalty on the connection between the webserver and the client, the advantages of HTTPS and establishing Always On SSL (AOSSL), even for nonsensitive transactional websites, far exceed the disadvantages.

Before getting into why AOSSL should be implemented universally, a bit of background information is needed. The de facto standard for all internet communication is TCP/IP. TCP, standing for Transmission Control Protocol, is found at the Transport layer, and is one of the four Abstraction layers (the others being the: Link, Internet, and Application layers) making up the Internet Protocol suite; which, simply put, handles how data is formatted, addressed, transmitted, routed, and received.

Internet Protocol (IP) is the chief protocol found in the Internet layer of the Internet Protocol suite. It delivers packets, which are small packages of data that include control information such as the IP address of the sender and recipient, as well as packet information needed for reliably delivering the packets to their destination. Packets also contain user data, sometimes referred to as the payload, which include a small portion of the overall packet structure that encapsulates data being transmitted.

With regards to the web, TCP and Hypertext Transfer Protocol, or HTTP, are principal. HTTP is one of the higher level protocols, and is found at the Application layer. Essentially, it’s job is to relay requests and responses between a client and a server (e.g., your web browser and the SecBrief.org server).

Security on the web is achieved through the use of the Transport Layer Security (TLS) protocol and the older Secure Sockets Layer (SSL) protocol. Though not a protocol per se, Hypertext Transfer Protocol Secure (HTTPS) is the result of layering the HTTP protocol on top of the SSL/TLS protocol.

And this is where things get pretty muddy, but basically, SSL/TLS use X.509 certificates for both authentication and encryption. Generally signed by a Certificate Authority (CA), X.509 certificates take advantage of an encryption technique that relies on a public and private-key pair for encryption and decryption. This form of asymmetric cryptography is rooted in the mathematical relationship between the private and public-key.

While the private-key is never exchanged and is kept safely on the server, the public-key is derived from the private-key and is made available through the X.509 certificate. Thus, whatever can be decrypted by the public-key is assumed to have come from the holder of the private-key. Where authentication is concerned, a certificate signed by a CA binds a single public-key to an individual site upon validation; thereby telling you that you can trust anything that you decrypt using that particular public-key offered by a site with a “verified identity.”

Now when discussing trust, therein lays a multitude of assumptions. The primary assumption is that the Public-key infrastructure (PKI) centered around the issuance and revocation of X.509 certificates by CAs is a secure model. The model itself is centered around trust. As an end-user, when you see the padlock and green bar in your address bar, the infrastructure, namely your browser which is checking a site’s X.509 certificate against it’s list of root certificates issued by CAs, is telling you that you can trust the site that you are attempting to access and that the connection is secure.

However, a problem exists in that after a CA and its root certificate are considered trusted, the trust is transparently passed down all the way to the user. In other words, once trust is established at the highest level, it is blindly passed down to the lowest level. This is called a hierarchy of trust. In theory, hierarchies of trust should all have a single root authority (i.e., a body that oversees the policies and practices of all CAs). However, in actuality, individual CAs do not have to account to any single body, though most are now at least audited by WebTrust. This means that the policies put in place by any individual CA are up to the CA to follow. Therefore the level of adherence to Certificate Practice Statements may vary.

The very fact that CAs and other members within the PKI are for profit undermines security. Additionally, sites that do not want to pay for a certificate and present self-signed certificates are punished with error messages questioning the trust of the site. Moreover, the arbitrary expiration dates, particularly ones that extend outwards of a year or more without any cryptological rationality, reduce the strength of certificates. Using a blacklist instead of a whitelist makes managing revoked/expired certificates difficult, especially over time, which reduces security. Another thing to think about is that CAs are subject to the laws within the jurisdictions where they conduct business. It is therefore completely reasonable to assume that state intelligence agencies could compel CAs to compromise the security of their clients in the name of “national security.”

Further undermining the PKI is the astronomical rise in Malicious Signed Binaries (MSB). MSB are malware which present valid or seemingly valid certificates which were either stolen, purchased, or altered. Nearly nonexistent a few years ago, three times as many MSB were discovered in 2013 than in 2012, totalling 5.7 million.[^1] This is having a tremendous impact on the the reputations of CAs and threatens the overall CA model.

Other more structural problems with the PKI as put forth by Ellison and Schneier include: the logic of trust which creates the foundation by which CAs operate; the lack of enforcement of private-key security; the potential insecurity of the verifying computer; the unreliable nature of associating a public-key with a name; lack of DNS authority in certificates; end-users are left to make the ultimate judgement of trust, usually with zero knowledge of the certificate; the practice of including Registration Authorities in the certificate issuance chain; how CAs verify identities; improper certificate implementation; and assumptions of security.[^2]

So if the PKI and it’s foundation of trust are so spotty, why would it be advisable to recommend that all web applications and sites use SSL certificates let alone bother with AOSSL? An entirely reasonable question, but first let’s address some misconceptions about HTTPS.

First, there is the misconception that HTTPS is always slower than HTTP. All things optimized and considered equal, yes, that is the case. However, rarely do web developers and designers perfectly optimize their sites. A site transferred over HTTPS leveraging a secure Content Distribution Network (CDN), aggressive compression, consolidated/minified dependencies, optimized media, among many other techniques which reduce load times, would knock the doors off of even a moderately optimized site transferring over HTTP. The introduction of SPDY, an Application layer protocol spearheaded by Google, offers even further reduction in latency for secure connections.[^3] So the point has been made, secure connections can be fast too.

Many also assume that security is prohibitively expensive. Yes, it is true that Extended Validation (EV) certificates and wildcard certificates (i.e., certificates that cover subdomains) can cost hundreds of dollars and sometimes even thousands of dollars, but for most sites, simple Domain Validation (DV) is sufficient. DV certificates can be found for around $10 on the inexpensive end (even <a href="https://cert.startcom.org/">free</a>), and around $100 on the high end. There of course will be a small computational cost exacted by the webserver in processing secure connections, but this fact is negligible for the vast majority of websites and applications.

What about virtual environments where a good portion of the internet is hosted, isn’t it true that you cannot have SSL certificates when you use shared hosting? Well, I cannot spin this as it is not a misconception. Indeed, this remains the largest current barrier to universal adoption of AOSSL because while multiple domains can be hosted on a single IP address, as is the case in most virtual environments, SSL certificates require a unique IP address that is unshared with other domains. The problem is that IPv4 address are running out and until IPv6 is fully adopted, IP addresses will remain a precious commodity.

Alright, so having now presented arguments for why you should not trust the PKI and provided only meager arguments with regards to misconceptions, it is time to convince you that universal AOSSL is still worthwhile. Earlier in the article, SSL/TLS was described ad nauseum, and it is clear that it provides a secure connection between a client and a server, but what are the limitations of this security?

Let’s start with the obvious. It will NOT prevent the NSA or any other sophisticated state intelligence agency from seeing your communication. There are just too many factors that go into true security, and undoubtedly the human component never fails when it comes to weakening security. Moving on.

Beginning with some caveats, HTTPS will not protect your site from all types of attacks to include: Distributed-denial-of-service (DDoS), Cross-site scripting (XSS), and SQL injection attacks. And now without further adieu, HTTPS and AOSSL when implemented properly can all but eliminate man-in-the-middle attacks, eavesdropping (meaningless ciphertext is all that will be seen by someone monitoring traffic), and session hijacking when binding the session to the cryptographic network credentials.[^4]

AOSSL is important because if only the login or checkout portion of a connection are secured, all follow-on traffic is not only vulnerable to intercept, but also places session cookies at risk where used. This sets attackers up for a sidejacking attack which occurs when an attacker intercepts all of the data being transmitted between the client and the server, including the session cookie. The attacker will then be able to use the intercepted session cookie to impersonate the victim without the need for user credentials because that user has already been authenticated by the application.[^5]

The takeaway is that HTTPS and AOSSL will not make you invincible, as either a user or a webmaster. It will however, take a particular website out of the nearly infinite pool of low hanging fruit that hackers have to choose from. Additionally, despite my many attempts to convince you otherwise, that padlock and green address bar, signifying a secure connection, still means something to most visitors. Another good reason is that AOSSL is all but an inevitability. HTTP 2.0 when adopted, will make TLS 1.2 mandatory. Thus, all internet connections will be secured by default. Lastly, with a movement towards improving online privacy, or at least the semblance thereof, it is an inexpensive courtesy that sites can pass along to their visitors with minimal impact on performance when implemented properly.

Webmasters should use HTTP Strict Transport Security (HSTS) when implementing AOSSL. Also take a look at <a href="https://otalliance.org/resources/AOSSL/OTA_Always-On-SSL-White-Paper.pdf">this publication</a>. As an end-user, I strongly encourage you to install <a href="https://www.eff.org/https-everywhere">HTTPS everywhere</a> because it will force a secure connection on websites that default to HTTP but are able to serve their site over HTTPS.

<center>
### Notes ### 
</center>

 
[^1]: “Malicious Signed Binaries Crush Certificate Authority Reputation,” McAfee, March 24, 2014, accessed March 24, 2014, http://www.mcafee.com/sg/security-awareness/articles/malicious-signed-binaries.aspx.


[^2]: Carl Ellison and Bruce Schneier, “Ten Risks of PKI: What You’re not Being Told about Public Key Infrastructure,” Computer Security Journal, Volume XVI, Number 1, 2000, accessed April 7, 2014, https://www.schneier.com/paper-pki.pdf.


[^3]: See the SPDY Whitepaper http://www.chromium.org/spdy/spdy-whitepaper.


[^4]: Willem Burgers et al., “Prevent Session Hijacking by Binding the Session to the Cryptographic Network Credentials,” Institute for Computing and Information Sciences, Radboud University Nijmegen, The Netherlands, accessed April 11, 2014, http://www.cs.ru.nl/~rverdult/Prevent_Session_Hijacking_by_Binding_the_Session_to_the_Cryptographic_Network_Credentials-NORDSEC_2013.pdf.


[^5]: See Firesheep http://codebutler.com/firesheep/.
