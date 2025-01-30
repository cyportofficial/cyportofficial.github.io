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


Bei der Entscheidung, ob CRL (Certificate Revocation List) oder OCSP (Online Certificate Status Protocol) zur 
Überprüfung des Zertifikatsstatus verwendet werden soll, hängt die Wahl vor allem vom jeweiligen Use Case und den 
Anforderungen an die Sicherheit ab. Um diese Entscheidung zu treffen, ist es jedoch wichtig, die Funktionsweise beider
Technologien zu verstehen.

Die Revokation von Zertifikaten ist in der Praxis anwendungsabhängig und hängt stark vom spezifischen Szenario ab. Zu 
den gängigen Use Cases gehören Web PKI, Unternehmens-PKI und IoT-PKI, wobei in jedem dieser Fälle unterschiedliche 
Anforderungen an die Verfügbarkeit und Vertraulichkeit bestehen können.

TODO: Unternehmens-PKI und IoT-PKI umschreiben bzw. Layer-7 erwähnen JOSE Standard 

Ein allgemeines Problem bei der Revokation von Zertifikaten betrifft die Tatsache, dass Root-Zertifizierungsstellen 
(Root CAs) über CRLs oder OCSP nicht direkt revoziert werden können. Dies führt zu zwei Hauptproblemen: Ein „Hard Fail“
(Fehlgeschlagene Überprüfung) kann zu erheblichen Verfügbarkeitsproblemen führen, während ein „Soft Fail“ 
(unsichere Überprüfung) die Vertraulichkeit verletzt und somit als unzureichend angesehen wird. Darüber hinaus kann 
diese Revokationstechnik nicht gegen falsch ausgestellte Zertifikate vorgehen, die noch nicht entdeckt wurden.

Die Nutzung von CRLs bringt zudem einige spezifische Probleme mit sich. So können CRLs sehr groß werden, was 
insbesondere für Geräte mit begrenztem Speicher, wie etwa IoT-Geräte, problematisch ist. Hinzu kommt, dass CRLs oft 
nicht aktuell sind und deren Download aufwendig sein kann. 

OCSP hingegen bietet eine andere Herangehensweise zur Zertifikatsüberprüfung. Allerdings gibt es auch hier 
Schwierigkeiten: Der Datenschutz der Nutzer kann beeinträchtigt werden, da IP-Adressen in den Anfragen übertragen 
werden. Zudem weist OCSP eine hohe Latenz von etwa 300 ms auf, was in einigen Szenarien, wie zum Beispiel bei captive 
Portals, zu Funktionsproblemen führen kann. Eine Lösung für diese Herausforderungen stellt das sogenannte OCSP-Stapling 
dar, bei dem das Zertifikatsstatus-Token im Cache gespeichert oder nach einer ungültigen Anfrage erneut angefordert wird, 
um Latenzprobleme zu minimieren.

Letztlich hängt die Wahl zwischen CRL, OCSP oder OCSP-Stapling vom jeweiligen Use Case und den spezifischen 
Sicherheitsanforderungen ab. Dabei sind verschiedene Fragen zu klären, um die richtige Lösung zu finden: 
* Wie wichtig sind Verfügbarkeit und Authentizität? 
* Ist der Schutz der Privatsphäre der Nutzer von Bedeutung? 
* Welche Speicherkapazitäten stehen zur Verfügung? 
* Wie viele Zertifikate oder Root-Zertifikate müssen überprüft werden? 
* Muss eine Signatur bei der Verwendung oder bei der Erstellung nicht-revoziert sein?

https://arstechnica.com/information-technology/2017/07/https-certificate-revocation-is-broken-and-its-time-for-some-new-tools/
https://letsencrypt.org/2024/12/05/ending-ocsp/
https://blog.cloudflare.com/high-reliability-ocsp-stapling/
https://www.chromium.org/Home/chromium-security/crlsets/
https://blog.mozilla.org/security/2015/03/03/revoking-intermediate-certificates-introducing-onecrl/
https://letsencrypt.org/2022/09/07/new-life-for-crls/




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

