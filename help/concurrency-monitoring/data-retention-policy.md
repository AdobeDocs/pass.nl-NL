---
title: Beleid voor gegevensbewaring
description: Beleid voor gegevensbewaring
exl-id: aa7d2d5e-9a8b-404b-874c-9e5923417784
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 0%

---

# Beleid voor gegevensbewaring {#data-retention-policy}

>[!WARNING]
>
>**Bericht:** de inhoud op deze pagina wordt verstrekt voor informatiedoeleinden slechts. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.


## Inleiding {#introduction}

Adobe moet in zijn rol als gegevensverwerker passende maatregelen nemen om zijn klanten te helpen toegang, schrapping, en andere verzoeken van individuen te vervullen. Het toepassen van een passend, veilig en tijdig verwijderingsbeleid is een belangrijk onderdeel van de naleving van deze verplichting.

## Definities {#definitions}

Een gegevensbewaarbeleid bepaalt hoe lang Adobe de gegevens van de klant opslaat. Het beleid van het Behoud van standaardGegevens voor Gelijktijdige Controle is **25 maanden**.

| Bewaarperiode gegevens | De bewaarperiode voor gegevens is de standaardbewaarperiode voor gegevens (25 maanden). |
|---|---|
| **Venster van het Behoud van Gegevens** | Het venster voor gegevensbewaring definieert de parameters waarvoor volledige gegevens kunnen worden weergegeven en gerapporteerd. Het venster van het gegevensbehoud wordt bepaald als volgt:<br/> *begindatum* = huidige datum - de periode van het gegevensbehoud <br/>*einddatum* = huidige datum |

## Gegevensverzameling {#data-collection}

*Clickstream gegevens* vertegenwoordigt gegevens die door klanten op de zittingshartslagen (bijvoorbeeld, subjectID, mvpdName, en meta-gegevens) worden gedeeld. Alle gebieden van douanemetagegevens worden van verwijzingen voorzien in de [ Standaard meta-gegevensattributen ](/help/concurrency-monitoring/standard-metadata-attributes.md).

## Klanttypen {#customer-types}

### Huidige klanten {#current-customers}

Tenzij de klant extensies voor het bewaren van gegevens koopt, voldoet Gelijktijdige bewaking aan de volgende vereisten voor het bewaren van klantgegevens:

* *Clickstream gegevens* die door de Controle van de Valuta worden verzameld moeten niet later dan **25 maanden** van de datum van inzameling worden geschrapt.

### Beëindigde klanten {#terminated-customers}

Een beëindigde klant is een klant die de verhouding met Adobe heeft beëindigd en niet meer Gelijktijdige Controle gebruikt.

* *Clickstream gegevens* die door Gelijktijdige Controle worden verzameld moeten binnen **6 maanden** van de het contractbeëindigingsdatum van de klant worden geschrapt.

## Gegevensverwijdering {#data-deletion}

Adobe behoudt het recht om gegevens te verwijderen voor datums na de bewaarperiode van de gegevens, zonder mogelijkheid tot herstel. Gegevens voor huidige klanten moeten maandelijks worden gewist.
