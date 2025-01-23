---
layout: page
title:  OCSP is dead
date:   2025-01-04
tags: cryptography x.509
---
Ein revoziertes Zertifikat verliert seine Vertrauenswürdigkeit und kann ein Sicherheitsrisiko darstellen. Wird es 
dennoch verwendet, können Angreifer unbefugten Zugriff auf vertrauliche Daten oder Systeme erlangen. 

Neben CRLs 
#### Was ist eine CRL (Certificate Revocation List)?
Eine CRL ist ein wesentlicher Bestandteil der Public Key Infrastructure (PKI),
die dazu dient, digitale Zertifikate zu verwalten und ihre Gültigkeit sicherzustellen. Eine CRL ist eine Liste,
die von einer Zertifizierungsstelle (CA = Certificate Authority) veröffentlicht wird und alle Zertifikate enthält,
die vor ihrem Ablaufdatum für ungültig erklärt wurden.
[rfc5280]

### OCSP
#### Was ist OCSP?
OCSP steht für Online Certificate Status Protocol (Online-Zertifikat-Status-Protokoll). Es handelt sich um ein 
Protokoll, das zur Überprüfung des Gültigkeitsstatus von digitalen Zertifikaten, insbesondere im Zusammenhang mit 
SSL/TLS-Verbindungen, verwendet wird. OCSP wird eingesetzt, um festzustellen, ob ein Zertifikat zurückgezogen 
(widerrufen) wurde, bevor es abläuft.
[rfc6961]


https://letsencrypt.org/2024/12/05/ending-ocsp/
https://blog.cloudflare.com/high-reliability-ocsp-stapling/
