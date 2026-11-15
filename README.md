# Persoonlijk Progressiesysteem


## Product core requirements

### Moet bieden
Dit systeem moet het volgende aan de gebruiker bieden:
- Overzicht creëren
- Regelmaat bieden
- Gevoel van continuïteit bieden
- Meer tijd vrijmaken
- Voldoening bieden
- Discipline mogelijk maken
- Voorspelbaarheid creëren
- Vooruitgang zichtbaar maken
- Flexibiliteit behouden
- Doelgericht blijven
- Laagdrempelige manier bieden om notities te maken


## Core Features
Dit zijn de features van het systeem. Captures, de Chain of Events, de debrief en de briefing zijn gebouwd. De Journal of Progression en het afvinken van afsprakenpakketten zijn nog niet gebouwd. De volledige eisen staan in `REQUIREMENTS.md`.


### Captures
Makkelijke manier om via een kort berichtje of voicememo een notitie vast te leggen. Een capture komt direct in `chain-of-events/` en wordt meteen gelinkt in de MoC van die kalenderdag.

### Chain of Events
Chronologische ketting van kalenderdagen (MoC's) met de ruwe captures van elke dag. Elke MoC linkt terug naar de vorige dag.

### Afsprakenpakketten
Periodiek "need-to-address-taken" kunnen afspreken die gedurende de dagen terugkeren tot het einde van de vooraf bepaalde periode. Tijdens de periode wordt er bijgehouden (afgevinkt) aan welke taken voldaan worden. Ze staan in `intentions/weekly/`.

Voorbeeld: elke week een vast afsprakenpakket van 3 tot 6 afspraken die je in je dagelijks leven wilt inbouwen, zoals een vaste tijd om naar bed te gaan, een vast moment om te stoppen met werken of een korte dagelijkse gewoonte.

### Debrief / reflection
's Avonds een overzicht van wat je die dag hebt bereikt. Met mogelijkheid om nog dingen aan te passen, toe te voegen en opmerkingen toe te voegen. Wat je voor morgen wilt doen, parkeer je in `intentions/daily/`; dingen voor later gaan naar `intentions/backlog/`.

### Briefing
's Ochtends een overzicht van wat je je voor die dag hebt voorgenomen: de geparkeerde punten van gisteravond en je afsprakenpakketten. De briefing legt ook het wisselmoment van de operationele dag vast in `changeover-log.md`.

### Journal of Progression
Chronologische ketting van operationele dagen (MoC's) en daaraan hangende notities; dagboekvorm; "wat is er gelukt en wat niet? en waarom?"; persoonlijke inzichten en beslissingen. Dit is een aparte laag bovenop de Chain of Events.
