---
title: Clientloze API-implementatie - foutcodes / Berichten met waarschijnlijke reden / oorzaak
description: Clientloze API-implementatie - foutcodes / Berichten met waarschijnlijke reden / oorzaak
exl-id: 616e35fc-9b72-422b-9a05-e6248bd52490
source-git-commit: 2dbb45aebb1a00863a9344114963f6df95763dfc
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 0%

---

# Clientloze API-implementatie - foutcodes / Berichten met waarschijnlijke reden / oorzaak {#clientless-api-implementation--error-codes-messages-with-probable-reason-cause}

>[!NOTE]
>
>De inhoud op deze pagina is uitsluitend ter informatie beschikbaar. Voor het gebruik van deze API is een actuele licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>


## Fout: niet geautoriseerd

### Oorzaken:

1. Ontbrekende autorisatiekop in POST
1. Probleem met autorisatiekop - controleer of de aanvraagtijd zich in milliseconden bevindt.

## Fout: SC 400 tijdens verificatie

### Oorzaken:

1. De server heeft de registratiecode niet gevonden, die voor een specifieke aanvrager en een bepaalde omgeving is gemaakt.
1. Er kunnen problemen optreden met andere domeinen met scripts
1. De juiste spoofing moet worden toegevoegd aan het /etc/hosts-bestand

## Fout: 400 Ongeldig verzoek

### Oorzaken:

1. Verkeerde URL voor POST/GET
1. SAMLAssertionParserException – De gecodeerde SAML-bevestiging kon niet worden ontsleuteld aan het einde van Adobe

## Fout: 403 Verboden

### Oorzaken:

1. Te veel snelle verzoeken - een functie van het API-beheer om DoS-aanvallen te voorkomen.
2. Als u een prequal-omgeving gebruikt, voegt u spoofing toe, anders moet u ervoor zorgen dat spoofing is verwijderd uit het /etc/hosts-bestand

## Fout: kan zich niet aanmelden bij de MVPD-pagina

### Oorzaken:

1. Gebruikersnaam en wachtwoord komen niet overeen
2. Aanmelden is mogelijk uitgeschakeld
3. Controleren of de aanmelding is bedoeld voor productie of staging


<!--

## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)

-->
