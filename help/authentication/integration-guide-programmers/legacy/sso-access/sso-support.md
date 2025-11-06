---
title: Single Sign-On-ondersteuning
description: Single Sign-On-ondersteuning
exl-id: edc3719e-c627-464c-9b10-367a425698c6
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1167'
ht-degree: 0%

---

# (Verouderd) Single Sign-On-ondersteuning

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [&#x200B; pagina van de Mededelingen van het Product &#x200B;](/help/authentication/product-announcements.md) wordt samengevoegd.

## Overzicht {#overview-sso-support}

In dit document worden de typen Single Sign On beschreven die door Adobe Pass-verificatie op verschillende platforms worden ondersteund en geactiveerd. Het bereik van dit document is om licht te werpen op wat wordt ondersteund en wat niet, wat is de MVPD-dekking voor elke SSO-methode en wat van de programmeurs wordt vereist om te kunnen profiteren van SSO op elk platform.

Nadat een gebruiker zich met hun geloofsbrieven van MVPD heeft aangemeld, produceert de Authentificatie van Adobe Pass een veilig teken dat de zitting van de Authentificatie van MVPD vertegenwoordigt, en bindt dat teken aan het apparaat van de gebruiker gebruikend een identiteitskaart van het Apparaat. Bij Adobe Pass-verificatie wordt het token/apparaat-id opgeslagen op een server of op het apparaat. Dit staat gebruikers toe om hun geloofsbrieven minder vaak in te gaan terwijl het houden van transacties veilig.

>[!NOTE]
>
>SSO-workflows maken deel uit van het Premium Workflow-pakket. Neem contact op met je Adobe Pass-verkoper als je deze functie wilt gebruiken.

## Huidige status van SSO op verschillende platforms {#current-sso-status-platforms}

| Platform/apparaat | SSO-ondersteuning | Type SSO | MVPD-dekking | Notities |
|:-------------------:|:-----------:|:---------------------------------------:|-----------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| Web (JavaScript) | Ja | Gedeeld verificatietoken (Adobe SSO) | Alles | Geen SSO voor meerdere browsers Volg de instructies in de Programmer Integration Guide voor JavaScript. Na het volgen van de instructies is SSO standaard ingeschakeld.  Verificatie per aanvrager wordt ingeschakeld en de SSO wordt verbroken |
| iOS | Ja | Platform SSO - tokenuitwisseling | Afhankelijk van Apple-ondersteuning, is de lijst hier | In iOS 10 introduceerden Apple en Adobe SSO-functionaliteit voor deelnemende programmeurs en MVPD&#39;s. Door de nieuwste Adobe iOS SDK te gebruiken of door de Adobe Clientless REST API te gebruiken en de Apple SSO-functionaliteit te implementeren, kunt u profiteren van SSO op iOS-apparaten. Meer informatie over SDK-implementatie hier en meer informatie over Clientless-implementatie. Extra opmerkingen: - Als u Apple SSO niet wilt gebruiken, kunt u nog steeds een beperkte SSO hebben tussen apps van dezelfde leverancier (dezelfde bundle ID) die opslag en een ID (IDFV) kunnen delen. SSO is dus beperkt tot de apps van dezelfde leverancier. |
| Android | Ja | Gedeeld verificatietoken (Adobe SSO) | Alles | Als de gebruiker niet het de toestemmingsverzoek van WRITE_EXTERNAL_STORAGE goedkeurt, zal de bibliotheek een lokale zandbak gebruiken. De implicatie in dit geval is dat er geen SSO tussen verschillende toepassingen zal zijn wanneer het gebruiken van de lokale opslag. |
| tvOS - nieuwe Apple TV | Ja | Platform SSO - tokenuitwisseling | Afhankelijk van Apple-ondersteuning, is de lijst hier | Vanaf tvOS 10 introduceerde Apple &amp; Adobe SSO-functionaliteit voor deelnemende programmeurs en MVPD&#39;s. Door de nieuwste Adobe tvOS SDK te gebruiken of door de Adobe Clientless REST API te gebruiken en de Apple SSO-functionaliteit te implementeren, kunt u profiteren van SSO op tvOS-apparaten. Meer informatie over tvOS SDK: hier en hier vindt u meer informatie over Clientless-implementatie. |
| Roku | Ja | Gedeeld verificatietoken (Adobe SSO) | Belangrijke volledige lijst voor dekking die binnenkort zal worden verstrekt. | Roku SSO werkt uit de doos met Clientless API voor alle klanten die de richtlijnen van Roku respecteren, geen speciale implementatie wordt vereist. SSO is gebaseerd op apparaatidentificatiegegevens die Roku veilig naar Adobe verzendt. |
| Amazon FireTV | Ja | Gedeeld verificatietoken (Adobe SSO) | Belangrijke volledige lijst voor dekking die binnenkort zal worden verstrekt. | FireTV SDK biedt ondersteuning voor Single Sign On op basis van Android-mogelijkheden. De SSO op dit platform is alleen mogelijk tussen apps die momenteel Adobe FireTV SDK gebruiken. Meer informatie over de nieuwe FireTV SDK hier. FireTV-toepassingen die worden geïmplementeerd boven op de client-API kunnen gebruikmaken van SSO tegen EOY 2018. |
| Xbox 360 | Nee |                                         |                                                     | Er is geen apparaat-id die we kunnen gebruiken. Er is een toepassings-id, zodat gebruikers deze niet telkens opnieuw hoeven te verifiëren. |
| Xbox One | Nee |                                         |                                                     | Er is geen apparaat-id die we kunnen gebruiken. Er is een toepassings-id, zodat gebruikers deze niet telkens opnieuw hoeven te verifiëren. |
| Windows 8/10 | Nee |                                         |                                                     | Er is geen apparaat-id die we kunnen gebruiken. Er is een toepassings-id, zodat gebruikers deze niet telkens opnieuw hoeven te verifiëren. |
| Samsung TV&#39;s | Nee |                                         |                                                     | Er is geen apparaat-id die we kunnen gebruiken. Er is een toepassings-id, zodat gebruikers deze niet telkens opnieuw hoeven te verifiëren. |

### Opmerkingen over Xbox 360 en Xbox One {#notes-xbox-360}

* **Xbox 360** - Xbox 360 baseert zich op de Levende Dienst om het teken te verstrekken dat deviceID inbedt. De lagen van de Levende Dienst in de appID-waarde voor deviceID, die het werkingsgebied maken slechts aan app. Voor Xbox 360 heeft Microsoft Adobe een Java-bibliotheek verschaft om het token te parseren.

* **Xbox One** - Een JSON Webteken zal worden uitgegeven dat met het cert/de sleutel van de uitgever wordt gecodeerd en door Microsoft wordt ondertekend. Adobe extraheert de deviceID uit een parameter met de naam DPI (Device Pairwise ID), anders dan de Xbox 360-parameter PDID (Partner Device ID). PDID bestaat ook in Xbox One, maar moet worden vervangen door deze nieuwe parameter &quot;Device Pairwise ID&quot; (DPI).


### SSO uitschakelen {#disable-sso}

In bepaalde situaties zullen sommige apps of sites SSO willen uitschakelen om aan geavanceerde bedrijfssituaties te voldoen.

* **voor JS en inheemse SDKs** - het de steunteam van de Authentificatie van Adobe Pass kan SSO voor een paar van identiteitskaart van de Aanvrager/van MVPD onbruikbaar maken. U hoeft niet te werken op sites of in native apps.  Zodra SSO door het team van de Steun van de Authentificatie van Adobe Pass onbruikbaar wordt gemaakt, zullen de authentificatie die met het gespecificeerde paar wordt uitgevoerd RequestorId/MVPD niet met plaatsen of apps worden gedeeld gebruikend verschillende NotaIDs. Bovendien zijn bestaande verificaties met verschillende aanvragers-id&#39;s niet geldig voor de combinatie van aanvrager-id / MVPD waarin SSO is uitgeschakeld. Technisch, wordt SSO het onbruikbaar maken verwezenlijkt door het teken AuthN aan de specifieke combinatie van identiteitskaart/MVPD van de Aanvrager te binden.
* **voor Clientless API** - u kunt SSO in de Klantloze authentificatiestroom onbruikbaar maken door een niet lege appId parameter in de vraag van REST te specificeren. U kunt elke tekenreeks als waarde gebruiken, zolang die tekenreeks uniek is voor de aanvrager-id. Voor de API zonder client moet de programmeur/implementator de site of de app wijzigen om deze parameter voor de aanvrager toe te voegen.

>[!IMPORTANT]
>
>BELANGRIJKE NOTA VOOR CLIENTLESS API SSO: Sommige MVPDs vereisen dat elk netwerk (aanvrager identiteitskaart) zijn eigen authentificatiestroom uitvoert. Voor de op SDK gebaseerde stromen (iOS enz.) wordt dit automatisch door de SDK afgehandeld. Nochtans, voor Clientless APIs moet dit door Programmer worden behandeld. We raden programmeurs ten zeerste aan om SSO-stromen op dit moment niet in te schakelen voor client-API&#39;s en in plaats daarvan een combinatie van apparaat-id en toepassings-id te gebruiken voor apparaat-id. Adobe zal ook werken aan het verbeteren van de Clientless API stromen zodat juiste SSO kan worden gevestigd.

### Afmelden {#logout-sso-support}

Programmeurs moeten zich ervan bewust zijn dat de actie &quot;Afmelden&quot; in de context van Single Sign-On, wanneer uitgevoerd in één app/op één site, alle tokens op het apparaat verwijdert en de gebruiker wordt afgemeld voor apps/sites.

Als aan SSO-voorwaarden wordt voldaan (ongeacht of SSO is ingeschakeld of uitgeschakeld), wordt Afmelden uitgevoerd en worden alle verificatie- en autorisatiegegevens verwijderd.
