---
title: Clientless API-implementatie - Foutcodes / berichten met waarschijnlijke reden / oorzaak
description: Clientless API-implementatie - Foutcodes / berichten met waarschijnlijke reden / oorzaak
exl-id: 616e35fc-9b72-422b-9a05-e6248bd52490
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# (Nalatenschap) Clientless API-implementatie - Foutcodes / berichten met waarschijnlijke reden / oorzaak {#clientless-api-implementation--error-codes-messages-with-probable-reason-cause}

>[!NOTE]
>
>De inhoud van deze pagina is uitsluitend bedoeld voor informatieve doeleinden. Voor het gebruik van deze API is een actuele licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor dat u op de hoogte blijft van de meest recente productaankondigingen voor Adobe Pass-verificatie en tijdlijnen voor buitengebruikstelling die zijn verzameld op de [pagina Productaankondigingen](/help/authentication/product-announcements.md) .

</br>


## Fout: Niet geautoriseerd

### Oorzaken:

1. Ontbrekende autorisatiekop in de POST
1. Probleem met autorisatieheader - controleer of de aanvraagtijd in milliseconden is.

## Fout: SC 400 tijdens authenticatie

### Oorzaken:

1. De server heeft de registratiecode niet gevonden, die is aangemaakt voor een specifieke aanvrager en omgeving.
1. Het kan zijn dat je problemen ondervindt met cross-domain scripting
1. De juiste spoofing moet worden toegevoegd aan het bestand /etc/hosts

## Fout: 400 onjuist verzoek

### Oorzaken:

1. Verkeerd ingedeelde url voor POST/GET
1. SAMLAssertionParserException – De versleutelde SAML-bewering kon aan het einde van Adobe niet worden gedecodeerd

## Fout: 403 Verboden

### Oorzaken:

1. Te veel snelle verzoeken - een functie van het API-beheer om DoS-aanvallen te voorkomen.
2. Als je een prequal-omgeving gebruikt, voeg dan spoofing toe, zorg er anders voor dat spoofing is verwijderd uit het bestand /etc/hosts

## Fout: Kan niet inloggen op de MVPD-pagina

### Oorzaken:

1. Gebruikersnaam en wachtwoord komen niet overeen
2. Aanmelding is mogelijk uitgeschakeld
3. Controleer of de login voor productie of enscenering is


<!--

## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)

-->
