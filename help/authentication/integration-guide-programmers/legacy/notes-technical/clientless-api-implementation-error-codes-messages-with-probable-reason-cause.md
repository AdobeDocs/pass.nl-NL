---
title: Implementatie van de API zonder client - foutcodes / berichten met mogelijke reden / oorzaak
description: Implementatie van de API zonder client - foutcodes / berichten met mogelijke reden / oorzaak
exl-id: 616e35fc-9b72-422b-9a05-e6248bd52490
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# (Verouderd) Clientless API-implementatie - foutcodes / berichten met vermoedelijke oorzaak / oorzaak {#clientless-api-implementation--error-codes-messages-with-probable-reason-cause}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

</br>


## Fout: niet geautoriseerd

### Oorzaken:

1. Ontbrekende machtigingheader in de POST
1. Uitgave met machtigingsheader - controleer of de aanvraagtijd in milliseconden is.

## Fout: SC 400 tijdens verifiëren

### Oorzaken:

1. De server heeft de registratiecode, die voor een specifieke aanvrager en omgeving is gemaakt, niet gevonden.
1. Mogelijk gaat u naar problemen met interdomeinscripts
1. Correcte spoofing moet worden toegevoegd aan het bestand /etc/hosts

## Fout: 400 onjuist verzoek

### Oorzaken:

1. Onjuiste URL voor POST/GET
1. SAMLAssertionParserException - de gecodeerde bevestiging van SAML kon niet bij het eind van Adobe worden gedecrypteerd

## Fout: 403 Verboden

### Oorzaken:

1. Te veel snelle aanvragen - een functie van het API-beheer om DoS-aanvallen te voorkomen.
2. Als het gebruiken van prequizmilieu dan spoofing toevoegt, anders zorg ervoor spoofing is verwijderd uit /etc/hosts- dossier

## Fout: kan zich niet aanmelden bij MVPD-pagina

### Oorzaken:

1. Gebruikersnaam en wachtwoord komen niet overeen
2. Aanmelding is mogelijk uitgeschakeld
3. Controleren of de aanmelding is bedoeld voor productie of staging


<!--

## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)

-->
