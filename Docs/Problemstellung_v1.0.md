## 1 Einleitung  
Cyber-Security ist inzwischen zu einer betriebs­wirtschaftlichen Grund­voraussetzung digitaler Geschäfts­modelle avanciert. Das Bundes­amt für Sicherheit in der Informationstechnik (BSI) meldete im ersten Halbjahr 2024 einen massiven Zuwachs automatisierter Angriffe auf öffentlich erreichbare Web­anwendungen – darunter **Proxy-Shell-/Log4Shell-Exploit-Ketten**, deren initiale Schwachstelle in veralteten oder falsch konfigurierten Web-Stacks lag. Solche Einstiegs­punkte dienen Ransomware-Akteuren häufig als *Initial Access Vector*, bevor Verschlüsselung oder Datendiebstahl erfolgen. Die hohe Korrelation zwischen **unsicheren Web­diensten** und **erfolgreichen Ransom­ware-Kampagnen** macht deutlich: Wer gängige Web-Schwachstellen nicht beherrscht, öffnet Cyber­kriminellen das Tor – mit Folgekosten, die laut *Allianz Risk Barometer 2025* weltweit als größtes Unternehmens­risiko gelten.  

## 2 Problemhintergrund  
KMU, Start-ups und agile Entwickler*innenteams operieren in schnellen Release-Zyklen, besitzen aber selten dedizierte Security-Ressourcen. Fehlkonfigurierte Server, veraltete Bibliotheken oder unzureichende Input-Validierung lassen klassische Attacken – SQL-Injection, Cross-Site Scripting (XSS), unsichere HTTP-Header – ungehindert bis in die Produktion durchdringen. Die Folgen reichen von Daten- und Produktions­ausfällen bis zu Erpressung und Reputations­schäden.  

## 3 Zielgruppen und Personas  

| Persona | Rolle & Umfeld | Hauptziele | Schmerzpunkte | Relevanz für **WVS** |
|---------|---------------|------------|---------------|----------------------|
| **Julia Hoffmann**<br>Chief Technology Officer (CTO) bei einem SaaS-Start-up (≈ 20 MA) | Führt und optimiert eine Continuous-Deployment-Pipeline | Schnelle Releases **ohne Sicherheits­einbußen**; verlässliches Security-Gate in jedem Build | Keine Zeit für manuelle Pen-Tests; begrenztes Budget; geringe interne Security-Expertise | WVS muss sich nahtlos in CI/CD integrieren, minimalen Konfig-Aufwand haben und für Dev-Teams verständliche Reports erzeugen |
| **Martin Schuster**<br>System- & Netzwerkadministrator in einem mittelständischen Fertigungs­betrieb (≈ 250 MA) | Betreibt mehrere interne B2B-Webportale und Legacy-Systeme | Einhaltung regulatorischer Vorgaben (IT-SiG 2.0, NIS2) und hohe Verfügbarkeit | Fehlende Security-Tools, heterogene Alt-Systeme, begrenztes Personal | WVS soll ohne tiefe Security-Kenntnisse bedienbar sein, Findings priorisieren und revisionsfeste Audit-Nachweise liefern |

## 4 Rechtlicher & normativer Rahmen  

| Rechtsquelle | Relevante Verpflichtung | Verbindung zu OWASP Top 10 |
|--------------|------------------------|---------------------------|
| **DSGVO Art. 32** | „Stand der Technik“ für Vertraulichkeit, Integrität & Verfügbarkeit personenbezogener Daten | OWASP-Kontrollen gelten als markt­übliche Benchmarks und belegen den Stand der Technik |
| **NIS2-Richtlinie (Art. 21)** | Risikomanagement­pflichten & Meldung erheblicher Vorfälle | Kontinuierliche OWASP-Scans zeigen systematisches Schwachstellen­management |
| **IT-SiG 2.0 § 8a (DE)** | KRITIS & „Unternehmen im besonderen öffentlichen Interesse“: angemessene technische & organisatorische Maßnahmen | Abdeckung der OWASP Top 10 demonstriert *state-of-the-art*-Schutz gegenüber BSI-Auditor*innen |
| **ISO/IEC 27001:2022, A.14** | Sichere System­entwicklung: Schwachstellen früh erkennen & beheben | OWASP kategorisiert die »kritischsten Web-Risiken« und dient als Umsetzungshilfe |

> **Einordnung:** Die OWASP Top 10 sind kein Gesetz, aber ein international anerkanntes **Best-Practice-Verzeichnis**. Organisationen, die ihre Web­anwendungen systematisch gegen diese zehn Risiko­klassen testen, dokumentieren proaktiv den „Stand der Technik“.

## 5 Relevanz der OWASP Top 10 (2021) für ein Python-basiertes WVS  

| Adressierbare Kategorie | Warum automatisierbar? | Typische Prüfansätze von **WVS** |
|-------------------------|------------------------|---------------------------------|
| **A03 Injection** | Fuzzing & Timing-Analyse mit `requests` / `httpx` | SQL-, NoSQL-, LDAP-Payloads; Reflected & Stored XSS |
| **A05 Security Misconfiguration** | Header- & TLS-Inspection, Abgleich mit Benchmarks | Fehlen von CSP, HSTS, X-Frame-Options; Directory Listing |
| **A06 Vulnerable & Outdated Components** | Version-Scraping + CVE-Abgleich via NVD-API | Veraltete Framework-Versionen, ungepatchte Libraries |
| **A02 Cryptographic Failures** | Zertifikats- & Cookie-Analyse | Schwache Cipher-Suites, fehlende `Secure` / `HttpOnly`-Flags |
| **A10 Server-Side Request Forgery (SSRF)** | Manipulierte Ziel-URLs & Response-Analyse | Offene Redirects, interne Ressourcen-Leaks |
| **A07 Identification & Authentication Failures** | Session-Cookie-Checks, Rate-Limit-Tests | Predictable Session-IDs, fehlende MFA-Claims |
| **A01 Broken Access Control** (teilweise) | Rollen-/Pfad-Manipulation | Ungeprüfte Direkt­zugriffe auf Admin-Endpunkte |

*(A04 Insecure Design, A08 Software & Data Integrity Failures sowie A09 Logging/Monitoring sind überwiegend prozessual; **WVS** kann hier vor allem Awareness schaffen.)*

## 6 Technische Herausforderung  
Ein OWASP-konformer Web-Scan verlangt:

* **Payload-Generierung** (Blind-SQLi, DOM-XSS, SSRF-Chains)  
* **Header- & TLS-Analyse** (CSP, HSTS, ALPN-Check)  
* **Risikobewertung** (CVSS, EPSS-Scores)  
* **Kontinuierliche Re-Scans** nach Patches  

Manuelle Tests skalieren nicht mit dem DevOps-Takt. Ein automatisierter Scanner muss performant, CI/CD-freundlich und rechtssicher dokumentierend sein.

## 7 Motivation für das Projekt  
Der geplante **Web Vulnerability Scanner (WVS)** schließt diese Lücke, indem er:

1. die oben genannten OWASP-Schwachstellen automatisiert erkennt,  
2. sich als Self-Service in bestehende CI/CD-Pipelines integriert,  
3. Findings risikoorientiert **für Entwickler*innen** aufbereitet,  
4. technische Reports erzeugt, die DSGVO, NIS2 und IT-SiG 2.0 auditsicher unterstützen,  
5. sich bewusst auf jene OWASP-Kategorien beschränkt, die **durch Python-basierte Scans ohne externe Tools (z. B. ZAP) zuverlässig abgedeckt** werden können (A02, A03, A05, A06, A07, A10, teilweise A01).

## 8 Problemstellung (kompakt)  
> **Wie kann ein automatisiertes, CI/CD-integrierbares und audit-fähiges Analyse-Werkzeug entwickelt werden, das technisch versierten, aber nicht spezialisierten Nutzergruppen aus KMU und Start-ups ermöglicht, Web­anwendungen kontinuierlich auf die adressierbaren OWASP-Top-10-Schwachstellen (Injection, Misconfiguration, SSRF u. a.) zu prüfen, die Ergebnisse risikobasiert für Entwickler*innen aufzubereiten und zugleich die steigenden gesetzlichen Anforderungen von DSGVO, NIS2 und IT-SiG 2.0 nach »Stand der Technik« zu erfüllen?**  

Die Beantwortung dieser Leitfrage bildet die Bewertungs­grundlage für das Software­projekt (4. Semester, DHBW Mannheim) und verbindet **praktische OWASP-Konformität** mit **rechtlicher Nachweis­barkeit** im Umfeld wachsender Cyber- und Ransomware-Risiken.
