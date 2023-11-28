---
title: Integriteitscontrole iOS/tvOS-opslag
description: Integriteitscontrole iOS/tvOS
source-git-commit: 444a81ad18afcb26dcf3ae8b41f4d02c465f4655
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 0%

---

# Integriteitscontrole iOS/tvOS {#iostvos-sdk-storage-integrity-checks}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Inleiding {#Intro}

Vanaf versie 3.8.3 van de iOS/tvOS AccessEnabler SDK is de mogelijkheid om controles op de integriteit van de opslag uit te voeren beschikbaar bij de AccessEnabler-initialisatie.

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
| INTEGRITY_CHECK_ALL | Opslagfunctionaliteit <br/> Geldigheid van opgeslagen waarden | Bij controle mislukt | Alle beschikbare integriteitscontroles worden uitgevoerd bij opslaginitialisatie | Wanneer beschadiging van SDK-opslag wordt vermoed. <br/> Als om het even welke integriteitscontroles ontbreken, zal de gebruiker worden geregistreerd |
| INTEGRITY_CHECK_CLEAR | Geen | Altijd | De opslag wordt gewist bij de initialisatie van de opslag | Wanneer de SDK-stromen niet naar verwachting kunnen worden voltooid |
