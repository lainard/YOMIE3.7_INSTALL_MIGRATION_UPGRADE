# Projectovereenkomst: YomieCRM Platform Upgrade

## Migratie naar PHP 8.4 & CodeIgniter 4.7

---

## 1. Partijen

**Dienstverlener:**
ADITUM IT
BTW-nummer: BE 1019.631.039

**Opdrachtgever:**
Yomie

---

## 2. Projectsamenvatting

Deze overeenkomst betreft de volledige migratie van het YomieCRM-platform van de huidige stack naar een moderne, ondersteunde stack:

| Component | Huidig | Doel |
|-----------|--------|------|
| PHP Versie | 7.4 (End-of-Life) | 8.4 |
| Framework | CodeIgniter 3.x | CodeIgniter 4.7 |
| Infrastructuur | Docker (php:7.4-apache) | Docker (php:8.4-apache) |

De volledige scope is gedocumenteerd in [TODO.md](./TODO.md).

---

## 3. Reden voor Upgrade

- **PHP 7.4** heeft op 28 november 2022 het einde van de levensduur bereikt. Er worden geen beveiligingspatches meer uitgebracht, waardoor het platform kwetsbaar is.
- **CodeIgniter 3** bevindt zich in onderhoudsmodus zonder nieuwe functies of actieve ontwikkeling.
- De upgrade brengt aanzienlijke prestatieverbeteringen (PHP 8.4 JIT), moderne beveiligingspraktijken, langetermijnondersteuning en betere onderhoudbaarheid.

---

## 4. Scope van het Werk

Het project bestaat uit 12 fasen zoals beschreven in [TODO.md](./TODO.md):

1. Voorbereiding & Audit
2. PHP 8.4 Compatibiliteitsfixes
3. CodeIgniter 4.7 Projectopzet
4. Migratie Models (15+ modellen)
5. Migratie Controllers (30+ controllers over admin/api/client/group)
6. Migratie Custom Libraries (20+ bibliotheken)
7. Migratie Views
8. Migratie Helpers & Hooks
9. Cron Jobs & CLI Commands
10. Docker & Infrastructuur Update
11. Testen & Kwaliteitsborging
12. Gefaseerde Deployment & Go-Live

---

## 5. Tijdlijn

Geschatte doorlooptijd: **26 weken (ongeveer 6 maanden)** vanaf de ondertekeningsdatum van deze overeenkomst.

| Mijlpaal | Doelweek | Oplevering |
|----------|----------|------------|
| Audit Afgerond | Week 2 | Codebase-inventaris & compatibiliteitsrapport |
| PHP 8.4 Draaiend (CI3) | Week 4 | Applicatie draait op PHP 8.4 met bestaand framework |
| CI4 Basis Gereed | Week 6 | Nieuwe projectstructuur, routing, authenticatie werkend |
| Models Gemigreerd | Week 9 | Alle database-modellen overgezet en getest |
| Controllers Gemigreerd | Week 13 | Alle endpoints functioneel in CI4 |
| Libraries & Views Klaar | Week 18 | Volledige functionaliteitspariteit |
| Testen Afgerond | Week 24 | Alle integraties geverifieerd |
| Productie Go-Live | Week 26 | Uitgerold met rollback-plan |

Tijdlijnen zijn schattingen en kunnen verschuiven door onvoorziene complexiteit. Elke significante vertraging (meer dan 2 weken) wordt tijdig gecommuniceerd.

---

## 6. Op te Leveren Resultaten

- Volledig functioneel YomieCRM draaiend op PHP 8.4 + CodeIgniter 4.7
- Bijgewerkte Docker-infrastructuur (Dockerfile, docker-compose, cron-configuratie)
- Alle bestaande functionaliteiten behouden en werkend:
  - Authenticatie & SSO
  - Klantenportaal
  - Facturatie & billing
  - Exact Online integratie
  - Zoho CRM synchronisatie
  - Twikey / Mollie betalingsverwerking
  - Peppol-factuurverwerking
  - E-mail (Brevo/SendInBlue)
  - Helpdesk & Knowledge Base
  - API-endpoints (mobiel, web, printer)
  - Cron/geplande taken
- Bijgewerkte documentatie (README, INSTALL)
- Rollback-plan in geval van kritieke problemen na deployment

---

## 7. Wat NIET Inbegrepen Is

Tenzij expliciet schriftelijk overeengekomen:

- Nieuwe functieontwikkeling tijdens de migratieperiode
- UI/UX-herontwerp of frontend framework-wijzigingen
- Databaseschema-herontwerp of datamodel-herstructurering
- Migratie naar een andere database-engine
- Migratie naar een andere hostingprovider of cloudplatform
- Wijzigingen aan de mobiele app buiten API-compatibiliteit

---

## 8. Verantwoordelijkheden Opdrachtgever

Yomie verbindt zich ertoe:

- Toegang te verlenen tot alle benodigde omgevingen (staging, productie, databases)
- Documentatie of verduidelijking te bieden over bedrijfslogica waar de code onduidelijk is
- Een contactpersoon aan te wijzen die beschikbaar is voor vragen tijdens het project
- Deel te nemen aan testen en elke mijlpaal af te tekenen
- Geen grote feature-aanvragen te doen tijdens de migratie om scope creep te voorkomen
- Een onderhoudsvenster te voorzien voor de definitieve productie-overschakeling

---

## 9. Prijsstelling & Betaling

| Item | Bedrag |
|------|--------|
| Totale Projectkosten | EUR __________ |
| Betalingsschema | Zie hieronder |

**Betaling per Mijlpaal:**

| Mijlpaal | Percentage | Bedrag | Vervaldatum |
|----------|-----------|--------|-------------|
| Overeenkomst getekend (start) | 20% | EUR ____ | Bij ondertekening |
| Fase 2 afgerond (PHP 8.4 compatibel) | 20% | EUR ____ | ~Week 4 |
| Fase 5 afgerond (Controllers gemigreerd) | 20% | EUR ____ | ~Week 13 |
| Fase 11 afgerond (Testen klaar) | 20% | EUR ____ | ~Week 24 |
| Productie go-live (Eindoplevering) | 20% | EUR ____ | ~Week 26 |

Alle bedragen zijn exclusief BTW (21%) tenzij anders vermeld.

---

## 10. Garantie & Ondersteuning

- **30 dagen garantieperiode** na go-live voor bugfixes gerelateerd aan de migratie
- Fouten veroorzaakt door de migratie worden zonder bijkomende kosten opgelost tijdens de garantieperiode
- Nieuwe functie-aanvragen of wijzigingen buiten de migratiescope worden apart gefactureerd
- Na de garantieperiode gelden de standaard ondersteunings-/onderhoudsvoorwaarden

---

## 11. Risico & Rollback

- Het oude CI3-systeem blijft minstens 2 weken na go-live inzetbaar als rollback-optie
- Indien kritieke problemen worden gevonden die niet binnen 48 uur na go-live kunnen worden opgelost, wordt het systeem teruggedraaid naar de vorige versie
- Data aangemaakt tijdens de CI4-periode blijft behouden (dezelfde database)

---

## 12. Intellectueel Eigendom

- Alle code geproduceerd tijdens dit project is eigendom van Yomie
- ADITUM IT behoudt geen rechten op de custom code
- Gebruikte open-source bibliotheken blijven onder hun respectievelijke licenties

---

## 13. Vertrouwelijkheid

Beide partijen komen overeen alle bedrijfsinformatie, broncode, klantgegevens en interne processen als vertrouwelijk te behandelen en deze niet te delen met derden zonder schriftelijke toestemming.

---

## 14. Beeindiging

- Beide partijen kunnen deze overeenkomst opzeggen met 14 dagen schriftelijke kennisgeving
- Bij beeindiging wordt het tot dan voltooide werk opgeleverd en proportioneel gefactureerd
- Eventuele vooruitbetalingen voor niet-geleverde mijlpalen worden terugbetaald

---

## 15. Aanvaarding & Ondertekening

Door hieronder te tekenen gaan beide partijen akkoord met de voorwaarden in dit document en de scope zoals gedefinieerd in [TODO.md](./TODO.md).

---

**Voor Yomie (Opdrachtgever):**

Naam: ___________________________________

Functie: ___________________________________

Handtekening: ___________________________________

Datum: ___________________________________

---

**Voor ADITUM IT (Dienstverlener):**

Naam: ___________________________________

Functie: ___________________________________

Handtekening: ___________________________________

Datum: ___________________________________

BTW-nummer: BE 1019.631.039

---

*Dit document is opgesteld op 7 augustus 2026. Beide partijen dienen een ondertekend exemplaar te bewaren.*
