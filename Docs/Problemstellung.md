# Detaillierte Problemstellung – Web Vulnerability Scanner (WVS)

**Autoren:** Marius Hopp, Paul Lehnart, Maurice Mundi
**Datum:** 22. Mai 2025
**Version:** 2.0

## 1. Kontext: Die wachsende Bedrohungslage für Web-Anwendungen

Web-Anwendungen sind das Rückgrat der modernen digitalen Wirtschaft und Gesellschaft. Sie dienen als primäre Schnittstellen für:

* **Kundeninteraktionen:** Online-Shops, Buchungsportale, Service-Center.
* **Mitarbeiterproduktivität:** Interne Verwaltungsplattformen, Kollaborationstools, Wissensdatenbanken.
* **Partner-Ökosysteme:** B2B-Plattformen, Lieferkettenmanagement-Systeme, API-Gateways.

Ihre Allgegenwart und Zugänglichkeit machen sie jedoch zu einem Hauptziel für Cyberkriminelle. Die Statistiken sind alarmierend:

* **OWASP Top 10 2024:** Die Analyse der Open Web Application Security Project (OWASP) zeigt konstant, dass ein Großteil der häufigsten und kritischsten Schwachstellen Web-Anwendungen direkt betreffen. Im Jahr 2024 sind dies **8 von 10 Top-Schwachstellen**, darunter altbekannte, aber immer noch grassierende Probleme wie Injection-Angriffe (z.B. SQL-Injection, Cross-Site Scripting - XSS), Broken Access Control (fehlerhafte Zugriffskontrollen) und Security Misconfiguration (fehlerhafte Sicherheitskonfigurationen).
* **Häufigstes Einfallstor:** Studien und Berichte von Sicherheitsunternehmen bestätigen übereinstimmend, dass Web-Anwendungen der am häufigsten ausgenutzte Angriffsvektor sind, um in Unternehmensnetzwerke einzudringen.

Die Konsequenzen einer einzigen, erfolgreich ausgenutzten Schwachstelle können verheerend sein und reichen weit über technische Aspekte hinaus:

* **Offenlegung sensibler Daten:**
    * **Personenbezogene Daten:** Diebstahl von Kundendaten, Mitarbeiterinformationen oder Patientendaten. Dies führt nicht nur zu einem massiven Vertrauensverlust, sondern kann auch empfindliche Bußgelder nach sich ziehen (z.B. gemäß DSGVO bis zu 20 Mio. € oder 4 % des weltweiten Jahresumsatzes).
    * **Geschäftsgeheimnisse:** Verlust von geistigem Eigentum, strategischen Plänen oder Finanzdaten, was die Wettbewerbsfähigkeit untergräbt.
* **Lahmlegung von Betriebsabläufen:**
    * **Ransomware-Angriffe:** Verschlüsselung kritischer Systeme nach dem Hochladen einer Web-Shell über eine Schwachstelle, was zu Betriebsstillstand und hohen Lösegeldforderungen führen kann.
    * **Denial-of-Service (DoS/DDoS):** Überlastung der Web-Anwendung, sodass legitime Nutzer nicht mehr darauf zugreifen können, was direkte Umsatzeinbußen bedeutet.
* **Dauerhafter Imageschaden:**
    * **Reputationsverlust:** Negative Medienberichterstattung und Verlust des Kundenvertrauens sind oft langwierig und kostspielig zu reparieren.
    * **Verlust der Markenintegrität:** Defacement der Webseite oder Verbreitung von Malware über kompromittierte Systeme.

**Die besondere Herausforderung für kleine und mittelgroße Unternehmen (KMU):**

Während große Konzerne oft über dedizierte Sicherheitsteams und erhebliche Budgets für Cybersicherheit verfügen, stehen KMU vor spezifischen Hürden:

* **Budgetbeschränkungen:** Die Kosten für kommerzielle SaaS-Scanner, externe Penetrationstests oder spezialisierte Sicherheitsberater sind oft prohibitiv hoch.
* **Fehlende interne Ressourcen:** Es mangelt häufig an Fachpersonal mit tiefgreifendem Security-Know-how, um komplexe Open-Source-Tools (wie z.B. OWASP ZAP, Burp Suite Community) effektiv zu konfigurieren, die Ergebnisse korrekt zu interpretieren und daraus handlungsorientierte Maßnahmen abzuleiten.
* **Fokus auf das Kerngeschäft:** Sicherheitsaspekte werden oft als sekundär betrachtet, bis ein Sicherheitsvorfall eintritt.

Diese Diskrepanz zwischen der wachsenden Bedrohung und den begrenzten Abwehrmöglichkeiten für KMU schafft eine gefährliche Sicherheitslücke.

## 2. Konkretes Problem: Die unsichtbare Gefahr und ihre Folgen

Aus dem oben genannten Kontext ergeben sich mehrere konkrete, miteinander verknüpfte Probleme, mit denen sich Betreiber von Web-Anwendungen, insbesondere KMU, konfrontiert sehen:

* **Unentdeckte und persistente Schwachstellen:**
    * Viele Unternehmen sind sich des wahren Zustands ihrer Web-Sicherheit nicht bewusst. Kritische Lücken wie SQL-Injection, Cross-Site Scripting (XSS), unsichere direkte Objektverweise (IDOR), veraltete Komponenten mit bekannten Schwachstellen oder unsichere HTTP-Header-Konfigurationen bleiben oft monate- oder jahrelang unentdeckt.
    * Die Betreiber erfahren von diesen Schwachstellen häufig erst, wenn es zu spät ist – nämlich nach einem erfolgreichen Angriff, einem Datenleck oder einer Meldung durch externe Sicherheitsforscher.
    * **Beispiel Persona "Kleinunternehmer Klaus":** Klaus betreibt einen Online-Shop für regionale Produkte. Er hat keine IT-Abteilung und verlässt sich auf die Standardkonfiguration seiner Shop-Software. Er ahnt nicht, dass ein veraltetes Plugin eine XSS-Lücke enthält, über die Kundendaten abgegriffen werden könnten.

* **Fragmentierte und komplexe Tool-Landschaft:**
    * Die Überprüfung verschiedener Aspekte der Web-Sicherheit erfordert oft den Einsatz mehrerer, spezialisierter Werkzeuge. Passive Analysen (z.B. Überprüfung von TLS-Zertifikaten, HTTP-Security-Headern, Content Security Policy) und aktive Scans (z.B. Test auf Injection-Schwachstellen, Umgehung von Authentifizierungsmechanismen) sind selten in einem einzigen, einfach zu bedienenden Tool vereint.
    * Die Einarbeitung in mächtige, aber komplexe Tools wie OWASP ZAP oder Burp Suite ist zeitaufwendig und erfordert technisches Fachwissen, das in KMU oft nicht vorhanden ist. Die Standardeinstellungen dieser Tools sind nicht immer optimal für jede Anwendung.
    * **Beispiel Persona "Marketing-Managerin Maria":** Maria ist für die Webseite ihres mittelständischen Unternehmens verantwortlich. Sie hört von Sicherheitsrisiken, findet aber die verfügbaren Tools zu technisch und weiß nicht, wo sie anfangen soll.

* **Aufwändige und unverständliche Berichterstellung:**
    * Die Roh-Protokolle und Log-Dateien, die von vielen Security-Scannern generiert werden, sind meist sehr technisch, detailreich und für Nicht-Experten (z.B. das Management) kaum verständlich oder priorisierbar.
    * Die Erstellung von aussagekräftigen, Management-tauglichen Berichten, die klare Handlungsempfehlungen enthalten, ist ein manueller und zeitaufwendiger Prozess. Kommerzielle Tools bieten dies oft nur in teureren Versionen an.
    * Ohne klare Priorisierung und verständliche Erklärungen bleiben selbst erkannte Schwachstellen oft unbehoben.

* **Fehlende kontinuierliche Überwachung und Automatisierung:**
    * Sicherheitsüberprüfungen werden oft nur sporadisch oder als einmalige Aktion durchgeführt (z.B. vor einem Launch). In der dynamischen Welt der Webentwicklung, mit häufigen Updates und neuen Features, können jedoch jederzeit neue Schwachstellen entstehen.
    * Manuelle Scans geraten im Alltagsgeschäft leicht in Vergessenheit oder werden aufgrund von Zeitmangel aufgeschoben.
    * Die Integration von Sicherheitstests in bestehende Entwicklungs- und Release-Pipelines (DevSecOps) ist für viele KMU eine große Hürde, da entsprechende Tools und Expertise fehlen.

## 3. Relevanz und Risiken: Was auf dem Spiel steht

Die Nichtadressierung der genannten Probleme führt zu erheblichen Risiken, die die Existenz eines Unternehmens bedrohen können. Die folgende Tabelle verdeutlicht die potenziellen Auswirkungen:

| Risiko                                     | Auswirkung für Unternehmen                                                                                                                               | Konkretisierung                                                                                                                                                                                                                                                                                          |
| :----------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Datenabfluss (z.B. via SQL-Injection, XSS, IDOR)** | Vertrauensverlust bei Kunden und Partnern, massive DSGVO-Strafen (bis zu 4% des globalen Jahresumsatzes oder 20 Mio. €), mögliche Schadenersatzforderungen, Verlust von Wettbewerbsvorteilen durch Diebstahl von Geschäftsgeheimnissen. | Abfluss von tausenden Kreditkartendaten, Veröffentlichung interner Strategiepapiere, Kompromittierung von Nutzer-Accounts durch Session-Hijacking.                                                                                                                                                    |
| **Site-Defacement / Malware-Verbreitung** | Schwerer Reputationsschaden, Verlust des Markenimages, Blacklisting durch Suchmaschinen (z.B. Google Safe Browse), Verunsicherung von Besuchern und Kunden. | Angreifer verändern das Aussehen der Webseite mit politischen Botschaften oder illegalen Inhalten. Die Webseite wird unbemerkt zur Schleuder für Viren und Trojaner, die die Computer der Besucher infizieren.                                                                                                 |
| **Dienstunterbrechung (z.B. durch DoS, Ransomware)** | Direkte Umsatzverluste durch Nichterreichbarkeit von Online-Shops oder Diensten, Produktivitätsverluste bei internen Systemausfällen, Imageschaden durch Unzuverlässigkeit. | Ein Online-Shop ist während des Weihnachtsgeschäfts für mehrere Tage nicht erreichbar. Interne Kollaborationsplattformen sind durch Ransomware verschlüsselt, was die Arbeit ganzer Abteilungen lahmlegt.                                                                                                          |
| **Compliance-Verstöße** | Nichtbestehen von Audits (z.B. ISO 27001, PCI DSS), Vertragsstrafen bei Nichteinhaltung von Service Level Agreements (SLAs), Verlust von Zertifizierungen, rechtliche Auseinandersetzungen. | Ein Finanzdienstleister verliert seine PCI DSS-Zertifizierung aufgrund nachgewiesener Sicherheitslücken, was ihm die Abwicklung von Kreditkartenzahlungen unmöglich macht. Ein Zulieferer kann die Sicherheitsanforderungen eines Großkunden nicht nachweisen und verliert den Auftrag.                     |
| **Übernahme von Systemen (z.B. durch Web-Shells)** | Unbemerkte Nutzung der Server-Ressourcen für kriminelle Aktivitäten (z.B. Spam-Versand, Mining von Kryptowährungen, Teil eines Bot-Netzes), Ausgangspunkt für weitere Angriffe im internen Netzwerk (Lateral Movement). | Der Webserver wird Teil eines Botnetzes für DDoS-Angriffe. Angreifer nutzen den kompromittierten Webserver, um auf sensible Datenbanken im Backend zuzugreifen.                                                                                                                                                  |

## 4. Zielsetzung des Projekts: Ein zugänglicher Schutzschild

Angesichts dieser vielschichtigen Problemstellung ist das übergeordnete Ziel dieses Projekts die Entwicklung eines **CLI-basierten Web Vulnerability Scanners (WVS)**. Dieser Scanner soll als niederschwellige, aber dennoch leistungsstarke Lösung dienen, um die Sicherheit von Web-Anwendungen proaktiv zu verbessern. Der Fokus liegt dabei auf:

* **Automatisierte Tiefenanalyse zur Identifikation kritischer Schwachstellen:**
    * **Passive Checks:** Umfassende Überprüfung von Konfigurationsaspekten wie TLS-Einstellungen (Zertifikatsgültigkeit, Cipher Suites, Protokollversionen), HTTP-Security-Header (Strict-Transport-Security, Content-Security-Policy, X-Frame-Options etc.), CSP (Content Security Policy) und CORS (Cross-Origin Resource Sharing).
    * **Aktive Checks:** Gezielte Tests auf verbreitete und gefährliche Schwachstellen wie SQL-Injection (SQLi), Cross-Site Scripting (XSS), Directory Traversal (Path Traversal) und grundlegende Schwächen in Authentifizierungs- und Autorisierungsmechanismen. Hierbei soll die etablierte Engine von OWASP ZAP über dessen API genutzt werden, um von dessen umfangreichen Testvektoren zu profitieren.
* **Ressourcenschonende und flexible Ausführung:**
    * Konzeption als Headless-Anwendung ohne grafische Benutzeroberfläche (GUI) für maximale Effizienz und geringen Ressourcenverbrauch.
    * Bereitstellung als Docker-fähiges Image für einfache Portabilität und Ausführung in verschiedensten Umgebungen (lokal, CI/CD-Pipelines, Cloud).
* **Modularer und erweiterbarer Aufbau:**
    * Entwicklung klar definierter und voneinander abgegrenzter Scanner-Module (z.B. Modul für Header-Analyse, Modul für SQLi-Tests).
    * Diese Modularität soll es ermöglichen, zukünftig neue Schwachstellenkategorien oder Testverfahren mit vertretbarem Aufwand zu integrieren und den Scanner aktuell zu halten.
* **Verständliches und vielseitiges Mehrkanal-Reporting:**
    * **Management-taugliche Berichte:** Generierung von übersichtlichen HTML- und/oder PDF-Berichten, die Schwachstellen priorisiert darstellen, deren Risiken erläutern und klare Handlungsempfehlungen geben.
    * **Maschinenlesbare Formate:** Ausgabe der Ergebnisse im JSON-Format zur nahtlosen Integration in CI/CD-Pipelines (z.B. automatisches Failen eines Builds bei kritischen Funden) und zur automatisierten Erstellung von Tickets in Issue-Tracking-Systemen (z.B. Jira, GitLab Issues).
* **Einfache Integration in bestehende Entwicklungs-Workflows:**
    * Umfassende Kommandozeilenoptionen (CLI-Flags) zur Steuerung des Scan-Prozesses (z.B. Ziel-URL, Intensität des Scans, zu prüfende Schwachstellenkategorien).
    * Verwendung von standardisierten Exit-Codes, um den Erfolg oder Misserfolg eines Scans sowie die Kritikalität der gefundenen Schwachstellen programmatisch auswertbar zu machen (essenziell für GitHub Actions, GitLab CI, Jenkins etc.).

## 5. Abgrenzung des Projekts (Scope für Version 1.0)

Um im Rahmen des Softwareprojekts ein realistisch erreichbares und qualitativ hochwertiges Ergebnis zu erzielen, wird der Fokus der initialen Version (v1.0) klar definiert:

* **Im Scope (v1.0):**
    * Konzentration auf die Erkennung der **gängigsten und kritischsten Web-Schwachstellen**, orientiert an den OWASP Top 10 (insbesondere Injection, XSS, Broken Access Control soweit über automatisierte Tests prüfbar, Security Misconfiguration).
    * Umfassende Analyse von **TLS/SSL-Konfigurationen** und **HTTP-Security-Headern**.
    * Grundlegende Tests für Directory Traversal und schwache Authentifizierungsmechanismen.
* **Außerhalb des Scopes (v1.0):**
    * **Tiefgreifende Business-Logic-Tests:** Schwachstellen, die ein tiefes Verständnis der individuellen Anwendungslogik erfordern (z.B. komplexe Berechtigungsfehler in spezifischen Workflows), sind nicht Ziel der ersten Version.
    * **Physische Layer-Angriffe oder Social Engineering:** Diese Angriffsvektoren liegen außerhalb des Rahmens eines Web Vulnerability Scanners.
    * **Client-seitige Schwachstellen jenseits von XSS:** Umfassende Analysen von JavaScript-Code auf client-seitige Logikfehler (außer DOM-basiertes XSS) sind vorerst nicht geplant.
    * **Dedizierte Performance-Tests (Lasttests, Stresstests):** Diese können optional in Betracht gezogen werden, falls das Zeitbudget und die Ressourcen dies nach Erfüllung der Kernziele erlauben.
    * **Automatisierte Ausnutzung (Exploitation) von Schwachstellen:** Der Scanner soll Schwachstellen identifizieren und melden, nicht jedoch aktiv ausnutzen.

Diese klare Abgrenzung ist entscheidend, um die Komplexität zu managen und ein funktionierendes Kernprodukt zu liefern, das später erweitert werden kann.

## 6. Erwarteter Nutzen und Wertbeitrag des WVS

Die erfolgreiche Entwicklung und Implementierung des Web Vulnerability Scanners (WVS) verspricht einen signifikanten Nutzen für die Zielgruppe, insbesondere für KMU:

* **Präventiver Schutz und Risikominimierung:**
    * **Früherkennung:** Schwachstellen werden proaktiv identifiziert, *bevor* sie von Angreifern entdeckt und ausgenutzt werden können. Dies ermöglicht es Betreibern, rechtzeitig Gegenmaßnahmen zu ergreifen.
    * **Reduktion der Angriffsfläche:** Durch das Beheben der gemeldeten Lücken wird die Sicherheit der Web-Anwendung signifikant erhöht.
* **Kosteneffizienz und Ressourcenschonung:**
    * **Open-Source-Stack:** Durch die Nutzung von Open-Source-Komponenten (insbesondere der OWASP ZAP API) und die Bereitstellung als Open-Source-Tool entstehen keine direkten Lizenzkosten für die Anwender.
    * **Automatisierung:** Reduziert den manuellen Aufwand für Sicherheitstests und ermöglicht regelmäßige Überprüfungen ohne hohen Personaleinsatz.
    * **Vermeidung Folgekosten:** Die Kosten für die Behebung eines Sicherheitsvorfalls (Datenleck, Systemausfall) übersteigen die Kosten für präventive Maßnahmen um ein Vielfaches.
* **Unterstützung bei Compliance-Anforderungen:**
    * **Nachweisbarkeit:** Die generierten Berichte können als Nachweis für durchgeführte Sicherheitsüberprüfungen im Rahmen von Audits (z.B. ISO 27001) oder zur Erfüllung von technischen und organisatorischen Maßnahmen (TOMs) gemäß DSGVO dienen.
    * **Standardisierung:** Hilft bei der Etablierung von grundlegenden Sicherheitsstandards innerhalb der Organisation.
* **Wissensgewinn und Handlungssicherheit:**
    * **Verständliche Berichte:** Klare und priorisierte Darstellung der gefundenen Schwachstellen inklusive Erklärungen zu deren Risiko und konkreten Handlungsempfehlungen zur Behebung.
    * **Sensibilisierung:** Fördert das Bewusstsein für Web-Sicherheit innerhalb des Unternehmens und befähigt auch technisch weniger versierte Personen, die Risiken zu verstehen.
    * **Keine kryptischen Scan-Logs:** Anwender erhalten verwertbare Informationen anstelle von schwer interpretierbaren Rohdaten.
* **Verbesserung der Softwarequalität und Entwicklungsprozesse:**
    * **Integration in DevSecOps:** Ermöglicht die frühe Erkennung von Sicherheitsproblemen bereits im Entwicklungsprozess ("Shift Left Security").
    * **Sicherheitsbewusstsein bei Entwicklern:** Regelmäßige Scans und verständliche Reports können Entwickler dabei unterstützen, sichereren Code zu schreiben.

Durch die Adressierung der genannten Probleme und das Erreichen der Projektziele leistet der WVS einen wichtigen Beitrag zur Verbesserung der Cybersicherheit von Web-Anwendungen, insbesondere für Organisationen, denen bisher die Mittel und das Know-how für umfassende Sicherheitstests fehlten.