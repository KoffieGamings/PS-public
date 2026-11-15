# Requirements: Persoonlijk Progressiesysteem (PPS)

Dit document beschrijft **wat** het systeem moet doen.

## Eisen

### Captures

- **E1** Een capture maken kan via voicememo, tekstbericht of handmatig.
- **E2** Na mijn bevestiging staat een capture binnen 10 seconden op de juiste plek in de repo (`chain-of-events/`). Lukt opslaan niet, dan wordt het opnieuw geprobeerd; een capture gaat nooit verloren.
- **E3** Originele captures blijven bewaard en worden nooit gewijzigd, hernoemd of verwijderd. De bestandsnaam bevat datum, tijd en een korte slug.
- **E4** Een taak afvinken kan via een capture (voicememo of tekst) of door een vinkje aan te klikken.

### Avondreflectie

- **E5** Elke avond kan ik op mijn telefoon een reflectie doorlopen: wat ik die dag gedaan en bereikt heb, met de mogelijkheid om dingen aan te passen, toe te voegen en opmerkingen te maken.
- **E6** In de avondreflectie kan ik dingen voor morgen *parkeren* zonder ze uit te werken. Wat voor morgen is komt in de dagelijkse voornemens; twijfelgevallen en dingen voor later gaan naar de backlog.
- **E7** De avondreflectie toont het bereikte mooi opgemaakt, met vinkjes, sterretjes en dergelijke. *Zachte eis, maar belangrijk voor de beleving van het systeem.*

### Ochtendbriefing

- **E8** Elke ochtend kan ik op mijn telefoon een briefing doorlopen: een korte samenvatting van de vorige operationele dag en mijn taken en intenties voor vandaag. De briefing begint met mijn slaap- en wektijd (zie E15).
- **E9** De briefing toont een infographic van de operationele dag (wektijd, taken en intenties). *Zachte eis.*

### Voornemens en afsprakenpakketten

- **E10** Voornemens staan in `intentions/`: `daily/` voor dagelijkse voornemens (inclusief de 's avonds geparkeerde punten), `weekly/` voor wekelijkse voornemens (inclusief afsprakenpakketten) en `backlog/` voor alles wat nog geen dag heeft.
- **E11** Een backlog-item komt niet vanzelf op een dag te staan; ik bepaal zelf wanneer het naar de dagelijkse of wekelijkse voornemens gaat.
- **E12** Een afsprakenpakket is een weekset van ongeveer 3 tot 6 afspraken voor het dagelijks leven, die voortbouwt op die van de vorige week. Per dag kan ik afvinken welke afspraken ik ben nagekomen, via een capture of een kort bericht. Een pakket staat in `intentions/weekly/ap-YYYYMMDD.md`, en elke afvinking is daar één regel met de operationele dag, de afspraak en de capture.

### Operationele dag en dagwissel

- **E13** Het systeem onderscheidt kalenderdagen en operationele dagen. Een operationele dag loopt van het ene wisselmoment tot het volgende; een capture hoort bij de operationele dag waarin zijn tijdstip valt. Voorbeeld: vrijdag 00:45 hoort bij donderdag.
- **E14** De lengte van een operationele dag mag variëren (bijvoorbeeld 16 of 32 uur) en het wisselmoment hoeft elke dag niet gelijk te zijn.
- **E15** Het wisselmoment volgt uit de ochtendbriefing (huidige regel: wektijd min 2 uur) en wordt vastgelegd in `changeover-log.md`: één regel per bevestigde briefing. `changeover-log.md` is de bron van waarheid voor de definitieve wisselmomenten.
- **E16** Is er geen bevestigde briefing of is het wisselmoment onduidelijk, dan geldt het fallback-wisselmoment uit `config.md` (`fallback_changeover_time`). Dat kan ik zelf elke dag aanpassen. Een gebruikte fallback wordt ook in `changeover-log.md` vastgelegd, gemarkeerd als fallback, zodat hij vaststaat.
- **E17** Een vastgelegd wisselmoment, bevestigd of fallback, staat vast. Achteraf corrigeren valt buiten scope (zie onderaan).
- **E18** Een operationele dag wordt aangeduid met zijn startmoment (datum + tijd, bijvoorbeeld `2026-11-15_0530`) en hoort bij de kalenderdatum waarop hij begint. Een dag van 32 uur die op 15 november begint, blijft dus 15 november. Zo kunnen twee dagen nooit dezelfde naam krijgen.
- **E19** Een operationele dag is gesloten zodra het volgende wisselmoment bevestigd is. Pas dan worden de captures van die dag ingedeeld (zie E22); captures van de lopende dag staan alleen in de Chain of Events.

### Chain of Events en Journal of Progression

Een MoC (map of content) is een overzichtsnotitie die vooral verwijst naar andere notities. De volgorde zit in de bestandsnamen; de ketting en de koppeling van captures aan een dag lopen via wikilinks.

- **E20** `chain-of-events/` is een platte map met de ruwe captures. Elke kalenderdag heeft één MoC met alleen: een link naar de vorige dag en wikilinks naar de captures van dezelfde kalenderdag. Een link naar de volgende dag is er niet, want die MoC bestaat nog niet. De MoC heet `MoC-YYYYMMDD-<weekday>.md` (Engelse weekdag in kleine letters). Bij de eerste capture op een kalenderdag wordt de MoC meteen aangemaakt, en bij elke capture komt de wikilink er meteen in. Zijn er sinds de laatste MoC dagen zonder captures geweest, dan worden hun MoC's (alleen met de link naar de vorige dag) eerst alsnog aangemaakt, zodat de ketting niet breekt.
- **E21** `journal-of-progression/` is een platte map met per operationele dag een MoC: links naar de vorige en volgende dag, wikilinks naar de captures die bij die dag horen en daarnaast een klein beetje eigen content volgens `templates/MoC-template.md`. De MoC heet `JMoC-YYYYMMDD-HHMM-<weekday>.md`, met de startdatum en starttijd van de operationele dag.
- **E22** Bij het sluiten van een operationele dag worden de bijbehorende captures *gekopieerd* naar `journal-of-progression/`. De kopie is verbatim en krijgt dezelfde bestandsnaam, maar met `j-` (journal) in plaats van `i-` (inbox), zodat wikilinks eenduidig blijven. Het origineel blijft ongewijzigd in `chain-of-events/`.
- **E23** Indelen is herhaalbaar zonder dubbele kopieën. De kopieën en links in de Journal zijn te herleiden uit `chain-of-events/` en `changeover-log.md` (de eigen content in een Journal-MoC niet).

### Algemeen

- **E24** Er zijn geen maximumtijden voor het maken van een capture, de avondreflectie, de ochtendbriefing of een reviewsessie. Een sessie met de agent mag openstaan zolang ik wil, zonder time-out en zonder dat voortgang verloren gaat als ik even wegloop.
- **E25** De systeemtaal (mappen, bestandsnamen, config-sleutels, skills) is Engels. Captures blijven in de taal waarin ik ze maak; tekst die het systeem voor mij schrijft is in de taal uit `config.md`.

### Beslissingen

- **E26** Een openstaande beslissing kan ik op elk moment vastleggen met een capture, of parkeren in de avondreflectie (voor morgen of in de backlog). Zo is hij uit mijn hoofd en weet ik dat hij bewaard is.

### Installatie en rol van de agent

- **E27** De agent vindt zijn rol en algemene regels in `AGENTS.md`. Voor Claude verwijst `CLAUDE.md` naar hetzelfde bestand, zodat er maar één versie is.
- **E28** Bij het eerste gebruik (als `config.md` ontbreekt) begeleidt de skill `setup` de gebruiker stap voor stap: instellingen, mappen, skills beschikbaar maken voor de agent, de `upstream` voor updates en de geplande briefing en debrief. Dit moet te volgen zijn voor iemand die nieuw is in tech en AI.

## Mapstructuur

```
chain-of-events/          ruwe captures + MoC per kalenderdag (plat)
journal-of-progression/   gekopieerde captures + MoC per operationele dag (plat)
intentions/
  daily/                  dagelijkse voornemens, geparkeerde punten
  weekly/                 wekelijkse voornemens, afsprakenpakketten
  backlog/                alles zonder dag
reports/daily/
  daily-briefing/         briefingrapporten (dbf-...) en infographics
  daily-debrief/          debriefrapporten (ddb-...)
changeover-log.md         één regel per bevestigde briefing
config.md                 onder meer language en fallback_changeover_time (alleen in je eigen repo)
config.example.md         voorbeeld voor config.md
AGENTS.md                 rol en regels voor de agent
CLAUDE.md                 verwijst naar AGENTS.md
```

## Buiten scope voor nu

- Een voldaan-score in de avondreflectie.
- Achteraf corrigeren van een bevestigd wisselmoment (bijvoorbeeld dat van 3 dagen geleden) na een vergissing of fout. Daarvoor verzinnen we later een mechanisme.
- Het "waar ik het allemaal voor doe" in de ochtendbriefing. Dat blijft voorlopig leeg.

## Open punten

- Hoe helpt het systeem verder bij beslissingen? Bijvoorbeeld door openstaande beslissingen op een vast moment voor te leggen, of door het aantal keuzes per dag te beperken.