# Detaillierte Problemstellung – Web Vulnerability Scanner (WVS)

**Autoren:** Marius Hopp, Paul Lehnart, Maurice Mundi  
**Datum:** 22. Mai 2025  
**Version:** 2.0

## 1. Kontext: Die wachsende Bedrohungslage für Web-Anwendungen

Web-Anwendungen sind das Rückgrat der modernen digitalen Wirtschaft und Gesellschaft. Sie dienen als primäre Schnittstellen für:

- **Kundeninteraktionen:** Online-Shops, Buchungsportale, Service-Center.
- **Mitarbeiterproduktivität:** Interne Verwaltungsplattformen, Kollaborationstools, Wissensdatenbanken.
- **Partner-Ökosysteme:** B2B-Plattformen, Lieferkettenmanagement-Systeme, API-Gateways.

Ihre Allgegenwart und Zugänglichkeit machen sie jedoch zu einem Hauptziel für Cyberkriminelle. Die Statistiken sind alarmierend:

- **OWASP Top 10 2024:** Die Analyse des Open Web Application Security Project (OWASP) zeigt konstant, dass ein Großteil der häufigsten und kritischsten Schwachstellen Web-Anwendungen direkt betreffen. Im Jahr 2024 sind dies **8 von 10 Top-Schwachstellen**, darunter altbekannte, aber immer noch grassierende Probleme wie Injection-Angriffe (z. B. SQL-Injection, Cross-Site Scripting – XSS), Broken Access Control (fehlerhafte Zugriffskontrollen) und Security Misconfiguration (fehlerhafte Sicherheitskonfigurationen).
- **Häufigstes Einfallstor:** Studien und Berichte von Sicherheitsunternehmen bestätigen übereinstimmend, dass Web-Anwendungen der am häufigsten ausgenutzte Angriffsvektor sind, um in Unternehmensnetzwerke einzudringen.

Die Konsequenzen einer einzigen, erfolgreich ausgenutzten Schwachstelle können verheerend sein und reichen weit über technische Aspekte hinaus:

- **Offenlegung sensibler Daten:**
  - **Personenbezogene Daten:** Diebstahl von Kundendaten, Mitarbeiterinformationen oder Patientendaten. Dies führt nicht nur zu einem massiven Vertrauensverlust, sondern kann auch empfindliche Bußgelder nach sich ziehen (z. B. gemäß DSGVO bis zu 20 Mio. € oder 4 % des weltweiten Jahresumsatzes).
  - **Geschäftsgeheimnisse:** Verlust von geistigem Eigentum, strategischen Plänen oder Finanzdaten, was die Wettbewerbsfähigkeit untergräbt.
- **Lahmlegung von Betriebsabläufen:**
  - **Ransomware-Angriffe:** Verschlüsselung kritischer Systeme nach dem Hochladen einer Web-Shell über eine Schwachstelle, was zu Betriebsstillstand und hohen Lösegeldforderungen führen kann.
  - **Denial-of-Service (DoS/DDoS):** Überlastung der Web-Anwendung, sodass legitime Nutzer nicht mehr darauf zugreifen können, was direkte Umsatzeinbußen bedeutet.
- **Dauerhafter Imageschaden:**
  - **Reputationsverlust:** Negative Medienberichterstattung und Verlust des Kundenvertrauens sind oft langwierig und kostspielig zu reparieren.
  - **Verlust der Markenintegrität:** Defacement der Webseite oder Verbreitung von Malware über kompromittierte Systeme.

### Die besondere Herausforderung für kleine und mittelgroße Unternehmen (KMU):

Während große Konzerne oft über dedizierte Sicherheitsteams und erhebliche Budgets für Cybersicherheit verfügen, stehen KMU vor spezifischen Hürden:

- **Budgetbeschränkungen:** Die Kosten für kommerzielle SaaS-Scanner, externe Penetrationstests oder spezialisierte Sicherheitsberater sind oft prohibitiv hoch.
- **Fehlende interne Ressourcen:** Es mangelt häufig an Fachpersonal mit tiefgreifendem Security-Know-how, um komplexe Open-Source-Tools effektiv zu konfigurieren, Ergebnisse korrekt zu interpretieren und Maßnahmen abzuleiten.
- **Fokus auf das Kerngeschäft:** Sicherheitsaspekte werden oft als sekundär betrachtet, bis ein Sicherheitsvorfall eintritt.
