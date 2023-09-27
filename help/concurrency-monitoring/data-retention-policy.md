---
title: Beleid voor gegevensbewaring
description: Beleid voor gegevensbewaring
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---


# Beleid voor gegevensbewaring {#data-retention-policy}

>[!WARNING]
>
>**Opmerking:** De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.


## Inleiding {#introduction}

Adobe moet in zijn rol als gegevensverwerker passende maatregelen nemen om zijn klanten te helpen toegang, schrapping, en andere verzoeken van individuen te vervullen. Het toepassen van een passend, veilig en tijdig verwijderingsbeleid is een belangrijk onderdeel van de naleving van deze verplichting.

## Definities {#definitions}

Een beleid van het Behoud van Gegevens bepaalt hoe lang Adobe de gegevens van de klant opslaat. Het standaardbeleid van het Behouden van Gegevens voor Gelijktijdige Controle is **25 maanden**.

| Bewaarperiode gegevens | De bewaarperiode voor gegevens is de standaardbewaarperiode voor gegevens (25 maanden). |
|---|---|
| **Venster Gegevens bewaren** | Het venster voor gegevensbewaring definieert de parameters waarvoor volledige gegevens kunnen worden weergegeven en gerapporteerd. Het venster voor gegevensbewaring wordt als volgt bepaald:<br/> *Begindatum* = huidige datum - bewaarperiode van gegevens <br/>*Einddatum* = huidige datum |

## Gegevensverzameling {#data-collection}

*Clickstream-gegevens* vertegenwoordigt gegevens die door klanten op de zittingshartslagen (bijvoorbeeld, subjectID, mvpdName, en meta-gegevens) worden gedeeld. Naar alle aangepaste metagegevensvelden wordt verwezen in het dialoogvenster [Standaardmetagegevenskenmerken](/help/concurrency-monitoring/standard-metadata-attributes.md).

## Klanttypen {#customer-types}

### Huidige klanten {#current-customers}

Tenzij de klant extensies voor het bewaren van gegevens koopt, voldoet Gelijktijdige bewaking aan de volgende vereisten voor het bewaren van klantgegevens:

* *Clickstream-gegevens* wordt verzameld door Gelijktijdige Controle moet uiterlijk worden geschrapt **25 maanden** vanaf de datum van verzameling.

### Beëindigde klanten {#terminated-customers}

Een beëindigde klant is een klant die de verhouding met Adobe heeft beëindigd en niet meer Gelijktijdige Controle gebruikt.

* *Clickstream-gegevens* verzameld door Gelijktijdige Controle moet worden geschrapt binnen **6 maanden** vanaf de contractbeëindigingsdatum van de klant.

## Gegevensverwijdering {#data-deletion}

Adobe behoudt het recht om gegevens te verwijderen voor datums na de bewaarperiode van de gegevens, zonder mogelijkheid tot herstel. Gegevens voor huidige klanten moeten maandelijks worden gewist.

