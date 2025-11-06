---
title: Integriteitscontrole iOS/tvOS-opslag
description: Integriteitscontrole iOS/tvOS
exl-id: 5d7cdc46-3e51-4e14-9e30-d7f48bc87506
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---

# (Verouderd) Integriteitscontrole iOS/tvOS {#iostvos-sdk-storage-integrity-checks}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

## Inleiding {#Intro}

Vanaf versie 3.8.3 van de iOS/tvOS AccessEnabler SDK is de mogelijkheid om controles van de opslagintegriteit uit te voeren beschikbaar bij de AccessEnabler-initialisatie.

Om dit mechanisme te gebruiken, werd api uitgebreid met een extra initialisatiemethode voor de klasse AccessEnabler.

```
- (nonnull id) initWithStorageCheck:(IntegrityCheckType)performIntegrityCheck softwareStatement:(nonnull NSString *)softwareStatement;
```


## Integriteitscontroles {#Checks}

De controles van de opslagintegriteit zijn nuttig wanneer de corruptie van de opslag AccessEnabler wordt verdacht (zoals wanneer een rasvoorwaarde tijdens een lees-schrijf opslagverrichting gebeurt).

De volgende controles zijn beschikbaar om op initialisering uit te voeren AccessEnabler:
- Opslagoperabiliteit: controles op succes bij lees- en schrijfbewerkingen
- Integriteit van opgeslagen waarden: controleert of alle waarden geldig zijn en in de verwachte indeling

>[!IMPORTANT]
> 
>Als een van de controles mislukt, worden alle waarden in de opslag gewist en wordt de gebruiker afgemeld, wat kan leiden tot een slechte gebruikerservaring. Gebruik controles op de integriteit van de opslag alleen wanneer dit nodig wordt geacht.


## Standaardgedrag {#Default}

De controles van de opslagintegriteit worden uitgezet door gebrek wanneer het initialiseren van AccessEnabled gebruikend de standaardinitialiseringsmethode:

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnaler(softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] init:softwareStatement];
```

Om uitdrukkelijk te specificeren welke controles van de opslagintegriteit die op initialisering moeten worden uitgevoerd AccessEnabler, gebruik de volgende initialisatiemethode:

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnabler(storageCheck: IntegrityCheckType.INTEGRITY_CHECK_ALL, softwareStatement: softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] initWithStorageCheck:INTEGRITY_CHECK_ALL softwareStatement:softwareStatement];
```


## IntegrityCheckType {#Switcher}

Het enum IntegrityCheckType wordt aan de cliënttoepassing blootgesteld en heeft de volgende waarden:

| Waarde | Uitgevoerde controles | Opslag uitgeschakeld | Beschrijving | Aanbevolen gebruiksgeval |
|-----------------------|-----------------------------------------------------|-----------------|------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| INTEGRITY_CHECK_NONE | Geen | Nooit | Er worden geen integriteitscontroles uitgevoerd bij opslaginitialisatie | Wanneer de SDK-stromen werken zoals verwacht |
| INTEGRITY_CHECK_ALL | Opslagoperabiliteit <br/> Geldigheid van opgeslagen waarden | Bij controle mislukt | Alle beschikbare integriteitscontroles worden uitgevoerd bij opslaginitialisatie | Als er een vermoeden van beschadiging van SDK-opslag bestaat. <br/> Als een van de integriteitscontroles mislukt, wordt de gebruiker uitgelogd |
| INTEGRITY_CHECK_CLEAR | Geen | Altijd | De opslag wordt gewist bij de initialisatie van de opslag | Wanneer de SDK-stromen niet naar verwachting kunnen worden voltooid |
