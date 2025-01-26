---
layout: page
title:  Certifcate Revocation ist kaputt
date:   2025-01-04
tags: cryptography x.509
---

Ein revoziertes Zertifikat verliert seine Vertrauenswürdigkeit und kann ein Sicherheitsrisiko darstellen. Wird es 
dennoch verwendet, können Angreifer unbefugten Zugriff auf vertrauliche Daten oder Systeme erlangen. Um solche Szenarien
zu verhindern, ist es wichtig, die Gültigkeit eines Zertifikats zu überprüfen. Dafür gibt es zwei gängige Mechanismen: 
die Certificate Revocation List (CRL) und das Online Certificate Status Protocol (OCSP).

Eine CRL ist eine Liste, die von einer Zertifizierungsstelle (CA) regelmäßig veröffentlicht wird und alle widerrufenen 
Zertifikate enthält. Diese Liste wird heruntergeladen und lokal überprüft, um sicherzustellen, dass das zu prüfende 
Zertifikat nicht widerrufen wurde. Allerdings kann dieser Ansatz zu Verzögerungen führen, da die Liste oft groß ist 
und manuelle Aktualisierungen erfordert.

Das OCSP hingegen ermöglicht eine schnellere und dynamische Abfrage des Status eines Zertifikats. Statt eine gesamte 
Liste herunterzuladen, sendet der Client eine Anfrage an den OCSP-Responder der Zertifizierungsstelle, der dann den 
aktuellen Status des Zertifikats (gültig, widerrufen oder unbekannt) zurückgibt. Dies sorgt für effizientere und 
zeitnahe Überprüfungen, erfordert jedoch eine aktive Internetverbindung und kann unter Umständen die Privatsphäre 
des Nutzers beeinträchtigen, da der Status individuell abgefragt wird."


Soll man nun eine CRL oder OCSP zur Prüfung des Zertifikatsstatus verwenden?




https://arstechnica.com/information-technology/2017/07/https-certificate-revocation-is-broken-and-its-time-for-some-new-tools/
https://letsencrypt.org/2024/12/05/ending-ocsp/
https://blog.cloudflare.com/high-reliability-ocsp-stapling/
https://www.chromium.org/Home/chromium-security/crlsets/
https://blog.mozilla.org/security/2015/03/03/revoking-intermediate-certificates-introducing-onecrl/
https://letsencrypt.org/2022/09/07/new-life-for-crls/

Die Funktionsweise von CRL and OCSP muss bekannt sein bzw. ist vorraussetzung

Revocation is UseCase based
- Web PKI
- Company PKI
- IoT PKI

Allgemeines Problem mit Revocation
- Root CAs nicht revozierbar
- Hard oder Soft Fail



Probleme CRL:
- große Liste
- CRL ist meist nicht aktuell 
- IoT Geräte mit begränzter Speicherkapazität
- Download von CRLs aufwendig
Lösungsidee:
- CLRSets (Chrome) und OneCRL(Firefox)


Problem OCSP:
- Privatsphäre der User wird verletzt
- Hohe Latenz
Lösungsidee:
- OCSP Stapling (DauerCaching oder neue Anfrage nach invalid Cache bei Anfrage)


```
Certificate Revocation List (CRL):
    Version 2 (0x1)
    Signature Algorithm: sha256WithRSAEncryption
    Issuer: /C=US/O=Example CA Inc/CN=Example Root CA
    Last Update: Jan 1 00:00:00 2023 GMT
    Next Update: Jan 5 00:00:00 2023 GMT
    Revoked Certificates:
        Serial Number: 01
            Revocation Date: Jan 2 12:00:00 2023 GMT
            CRL Entry Extensions:
                Reason Code: Key Compromise
        Serial Number: 0A
            Revocation Date: Jan 3 08:30:00 2023 GMT
            CRL Entry Extensions:
                Reason Code: CA Compromise
    Signature Algorithm: sha256WithRSAEncryption
        Signature: ...
```



**Nachweise**

[RFC5280 "Internet X.509 Public Key Infrastructure Certificate
and Certificate Revocation List (CRL) Profile"](https://datatracker.ietf.org/doc/html/rfc5280)

[RFC6960 "X.509 Internet Public Key Infrastructure Online Certificate Status Protocol - OCSP"](https://datatracker.ietf.org/doc/html/rfc6960)

