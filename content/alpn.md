---
title: "TLS-ALPN-01"
date: 2023-08-26T15:00:00+01:00
tags: ['tls','ssl','alpn','acme','ssl-certificate','website','hosting']
---

# Automated Certificate Management Environment Transport Layer Security Application‑Layer Protocol Negotiation Challenge Extension 🤓🤓🤓

Have you even tried to host your own website on your **own** server *(not some cloud VPS shit)*?
If so, you might have been stopped by *port forwarding™️ ✨✨✨*.
Everything is nice and easy **until** you have to add the whole TLS thingy.

To add TLS encryption to your website, you have to generate a public and private key. The private key **must** be signed by a certificate signing authority to avoid MITM (Man-In-The-Middle) attacks by transferring trust from the server that says *bro, im totally w3.org, trust me* to the certificate signing authority, which actually tests (**challenges**) the server to prove that it owns w3.org.
There is nothing difficult in forwarding some ports, **but** often your ISP will block port 80, which is needed for the [HTTP-01 challenge](https://letsencrypt.org/docs/challenge-types/#http-01-challenge).

So... maybe let's try a different challenge. How about [*DNS-01 challenge*](https://letsencrypt.org/docs/challenge-types/#dns-01-challenge)?
This, on the other hand, requires that you can set CNAME records in your domain, which again might be blocked (or at least it was in my case).

At this point, young Mikołaj gave up.
But **3 years later** I was testing some Go(lang) web frameworks, and they often listed *autotls* on their feature lists.
I've decided to test what it is and **magically ✨✨✨** it generated a signed private key for me! 😲 At this point, I was amazed and wanted to find out **HOW?!**.
If you don't know exactly what you want to ask about, there isn't a better place than *StackOverflow™️*, so I've asked [this](https://stackoverflow.com/questions/76968320/how-did-gin-generate-ssl-certificate-for-me-although-port-80-and-cname-are-block) question.
I got the response that this magic spell can be used by saying *Automated Certificate Management Environment Transport Layer Security Application‑Layer Protocol Negotiation Challenge Extension*, TLS-ALPN-01 for short.

After **a lot** of ~~googling~~ *searching-information-on-the-internet* I learned that TLS-ALPN-01 does this magic by not requiring port 80 to do the challenge, the only thing that you need is open port 443 (HTTPS port).
I could not use this type of challenge before because it is not implemented in the *certbot* yet. Hopefully, I found [this](https://caddyserver.com/) web server that had it implemented, and the only thing I needed to do to have a static HTTPS website is `sudo caddy file-server --root /mnt/HDD/git/website/public --domain lubiak.k.vu`... **awesome ✨✨✨**.
