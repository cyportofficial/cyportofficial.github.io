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
Überprüfung des Zertifikatsstatus verwendet werden soll, hängt die Wahl vor allem vom jeweiligen Anwendungsfall und den 
Anforderungen an die Sicherheit ab. Um diese Entscheidung zu treffen, ist es jedoch wichtig, die Funktionsweise beider
Technologien zu verstehen.

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

Letztlich hängt die Wahl zwischen CRL, OCSP oder OCSP-Stapling vom jeweiligen Anwendungsfall und den spezifischen 
Sicherheitsanforderungen ab. Dabei sind verschiedene Fragen zu klären, um die richtige Lösung zu finden: 
* Wie wichtig sind Verfügbarkeit und Authentizität? 
* Ist der Schutz der Privatsphäre der Nutzer von Bedeutung? 
* Welche Speicherkapazitäten stehen zur Verfügung? 
* Wie viele Zertifikate oder Root-Zertifikate müssen überprüft werden?


---


...und zu welchen Zeitpunkt ist die Revozierungsinformation von Bedeutung, bei einer Signaturerstellung oder bei der 
Verifikation? Wie kann ich Replay-Angriffe verhindern oder beweisen, dass zu einem bestimmten Zeitpunkt ein Zertifikat
gültig war? Viele Fragen die einer genauen Analysen bedürfen um fundierte Entscheidungen zu treffen und ein Design
zu entwickeln, welches den realen Bedrohungen entgegen wirkt.


**Nachweise**

[RFC5280 "Internet X.509 Public Key Infrastructure Certificate
and Certificate Revocation List (CRL) Profile"](https://datatracker.ietf.org/doc/html/rfc5280)

[RFC6960 "X.509 Internet Public Key Infrastructure Online Certificate Status Protocol - OCSP"](https://datatracker.ietf.org/doc/html/rfc6960)

[RFC7515 "JSON Web Signature (JWS)"](https://datatracker.ietf.org/doc/html/rfc7515)

[RFC5126 "CMS Advanced Electronic Signatures (CAdES)"](https://datatracker.ietf.org/doc/html/rfc5126)

