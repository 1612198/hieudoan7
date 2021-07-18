---
title: "[web] SSL/TLS Certificate"
date: 2021-07-18T09:59:19+07:00
draft: False
---

# SSL/TLS Certificate Explanation
18/07/2021

After spending a few days researching the SSL/TLS certificate, I want to take a note for myself and show what I have understood in this blog post. Leave a comment if you have any disagreements or concerns about this topic.

## I. What is SSL Certificate?
First, SSL is the abbreviation of Secure Socket Layer and TLS is the abbreviation of Transport Layer Security which is the upgradation of SSL. In this post scope, we use the term SSL implying for both SSL/TLS.

An SSL Certificate is a digital certificate that authenticates a website's identity and enables an encrypted connection. In addition, it's actually a trusted way to transmit website (server) 's public key to client. 

## II. What is SSL Certificate used for?
As we mentioned earlier, SSL Certificate use to prove whether the website we're trying to connect is trusted or not.

## III. How does SSL Certificate work?
An SSL certificate comprises 3 certificates: End Entity Cert, Intermediate Cert and Root Cert which is also known as the 'chain of trust'.

When the client connects to the server through HTTPS protocol, the first phase is SSL handshake and it's where the validation of certification occurs. The client says "hello" to the server, the server responses an SSL Cert for the client to validate, just imagine the server say: "hey, this is my identity, check it before you accept to connect to me". How does the client validate the SSL Cert received from the server?
This process takes place in the order from End Entity Cert to Root Cert, client will use the pubic key carried in the parent cert to validate the current cert except for the root cert because the root cert's parent doesn't exist. So the question is how the client validate the root cert to ensure this SSL Cert is trusted or more specific to ensure the website public key is signed by a trusted Certificate Authority (CA). The magic is the client store all the public keys (or something like that to allow it to validate the root cert) in their local machine. If the Root CA doesn't match any of the stored CAs, the client can decide to install itself or conclude the cert isn't valid.

## IV. Summary

## References
1. [Hussein Video](https://www.youtube.com/watch?v=r1nJT63BFQ0&t=616s)
2. [security stackexchange question](https://security.stackexchange.com/questions/56389/ssl-certificate-framework-101-how-does-the-browser-actually-verify-the-validity?rq=1)
3. [blog keyfactor.com](https://www.keyfactor.com/blog/what-is-ssl/)

