---
title: Verificatie op thuisbasis voor tv overal
description: Verificatie op thuisbasis voor tv overal
exl-id: abdc7724-4290-404a-8f93-953662cdc2bc
source-git-commit: 7107d4a915113fb237602143aafc350b776c55d6
workflow-type: tm+mt
source-wordcount: '1638'
ht-degree: 0%

---

# Verificatie op thuisbasis voor tv overal

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Wat is verificatie op thuisbasis? {#whatis-home-based-authn}

HBA (Home Based Authentication) is een tv-functie overal waarmee betaaltv-abonnees tv-inhoud online kunnen bekijken zonder MVPD-referenties in te voeren als ze thuis zijn. Hierdoor wordt de gebruikerservaring van de verificatiestroom aanzienlijk verbeterd.

Thuisgebaseerde verificatie-definitie door het Open Authentication Technology Committee (OATC): &quot;In-home automatische verificatie is het proces waarbij een MVPD/OVD gebruikmaakt van kenmerken van het thuisnetwerk (of id&#39;s die automatisch toegankelijk zijn tussen apparaten op het thuisnetwerk) om te verifiëren welke abonneeaccount aan dat thuisnetwerk is gekoppeld, zodat gebruikers niet handmatig aanmeldingsgegevens hoeven in te voeren bij het instellen van een TVE-sessie voor toegang tot beveiligde inhoud.&quot;



Voor meer informatie over HBA en de industriestandaarden, lees de [ Gevallen en Vereisten van het Gebruik OATC ](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/Defining%20TVE%20Home-Based%20Authentication%20(HBA)%20%20Use%20Cases%20and%20Requirements%20Recommended%20Practice%20Version%201_0%20FINAL%20DRAFT%20FOR%20BOARD%20APPROVAL.pdf) {target=_blank} documentatie en **OATC de Richtlijnen van de Ervaring van de Gebruiker voor HBA**.

>[!NOTE]
>
>Sommige HBA-stromen maken deel uit van het Premium Workflow-pakket. Neem contact op met je Adobe Pass-verkoper als je deze functie wilt gebruiken.

## Waarom HBA belangrijk is voor u {#why-hba}

HBA is belangrijk omdat hierdoor praktisch de aanmeldingsbarrière voor uw kijkers die thuis zijn en al een kabelabonnement hebben, wordt verwijderd. Bovendien kan verificatie op thuisbasis de betrokkenheid van uw kijkers aanzienlijk vergroten en een betere gebruikerservaring bieden voor uw tv-inhoud overal.

Momenteel is bijna de helft van de aanmeldingspogingen niet geslaagd.

Zodra HBA door één van Hoogste 5 MVPDs werd geactiveerd, verhoogde zijn tarief van de authentificatieomzetting **met 40%** (van 45% tot 63%)

![](assets/authn-conv-pre-post.png)

Hieronder ziet u ook de aanmeldingsconversiesnelheid voor een kanaal dat is geïntegreerd met verschillende MVPD&#39;s: voor kanalen die HBA hebben ingeschakeld en voor kanalen die geen HBA hebben. De omrekeningskoers voor HBA-patiënten is aanzienlijk hoger dan die zonder HBA.

![](assets/hba-vs-non-hba.png)

Zes maanden nadat HBA voor de meeste kanalen die met dit MVPD worden geïntegreerd, werd toegelaten, merkten wij een verhoging van 82% in unieke gebruikers (het aantal gebruikers die tot TV overal door dit MVPD toegang hebben bijna verdubbelde).

2w3In tegenstelling, zoals u in de grafiek kunt zien hieronder, hadden andere MVPDs die HBA niet hadden toegelaten slechts een 26% toename in de unieke gebruikers in de afgelopen zes maanden.

![](assets/unique-visitors-incr.png)

Uit onze gegevens, die 6 maanden voor en 6 maanden na het inschakelen van HBA werden verzameld, zagen we een aanzienlijke toename in de betrokkenheid van kijkers voor de kanalen die HBA ingeschakeld waren. Praktisch gebruikers van MVPDs die HBA hebben toegelaten neigen om gemiddeld 30% meer inhoud te letten dan gebruikers van MVPDs die geen toegelaten HBA hebben.

![](assets/user-engagement-increase.png)

## Ondersteuning voor Adobe Pass Authentication HBA {#auth-hba-support}

In deze sectie wordt de HBA-ondersteuning beschreven die wordt geboden door Adobe Pass Authentication, het gedrag van Adobe Pass Authentication-platforms in HBA-flows en worden ook technische details beschreven die nuttig zijn voor de implementatie van HBA.

Adobe Pass-verificatiefuncties die HBA ondersteunen

* Mogelijkheid om verschillende verificatie-TTL&#39;s in te stellen voor HBA- en niet-HBA-verificatie (ook ondersteuning voor MVPD vereist)
* Mogelijkheid om automatisch een MVPD (de plukker van MVPD overslaan) te selecteren als de authentificatie verliep. Dit is nuttig, vooral wanneer HBA TTLs klein is.
* Mogelijkheid om de programmeurs te informeren als de verificatie HBA was of niet (ook ondersteuning van MVPD vereist)

### HBA-gebruikerservaring op Adobe Pass-verificatieplatforms {#hba-user-exp}

De volgende tabellen bevatten informatie over de gebruikerservaring voor de ondersteunde platforms wanneer HBA is ingeschakeld en wanneer HBA niet is ingeschakeld:

| Gebruikersstroom - Type platform | SWF, iOS, Android |
|---|---|
| Met HBA ingeschakeld | Wanneer gebruikers thuis zijn, worden ze automatisch geverifieerd. Nadat het token HBA AuthN verloopt, worden gebruikers automatisch opnieuw geverifieerd. |
| Zonder HBA | De gebruikers worden gevraagd om hun MVPD te selecteren en hun geloofsbrieven in te gaan, zelfs als zij thuis zijn.Nadat het teken AuthN verloopt, moeten de gebruikers hun geloofsbrieven opnieuw ingaan. |

| Gebruikersstroom - Type platform | js, Windows (native) |
|---|---|
| Met HBA ingeschakeld | Wanneer gebruikers thuis zijn, worden ze automatisch geverifieerd. Nadat het teken van AuthN HBA verloopt, moeten de gebruikers hun MVPD van de plukker opnieuw selecteren en zullen automatisch voor authentiek verklaard worden. |
| Zonder HBA | De gebruikers worden gevraagd om hun MVPD te selecteren en hun geloofsbrieven in te gaan, zelfs als zij thuis zijn. Nadat het teken AuthN verloopt, moeten de gebruikers hun geloofsbrieven opnieuw ingaan. |

| Gebruikersstroom - Type platform | REST-API zonder client (tweede schermverificatie) |
|---|---|
| Met HBA ingeschakeld | Wanneer gebruikers thuis zijn en een Clientless REST API-toepassing gebruiken, worden ze automatisch geverifieerd op het tweede schermapparaat nadat ze de registratiecode hebben ingevoerd en hun MVPD hebben geselecteerd. Nadat het token HBA AuthN is verlopen, worden gebruikers automatisch opnieuw geverifieerd (op het tweede schermapparaat). |
| Zonder HBA | De gebruikers worden gevraagd om hun MVPD te selecteren en hun geloofsbrieven in te gaan, zelfs als zij thuis zijn. Nadat het teken AuthN verloopt, moeten de gebruikers hun geloofsbrieven opnieuw ingaan. |

### Technische details van de implementatie van HBA {#tech-details-hba}

#### Protocol OAuth 2.0 {#oauth-2-protocol}

In de stroom HBA voor MVPDs die met het OAuth 2.0 authentificatieprotocol wordt geïntegreerd, geeft MVPD een vernieuwingstoken uit en de Adobe geeft een token van de authentificatie van HBA uit:

* Vernieuwingstoken heeft TTL die door de bedrijfsvereisten van MVPD wordt bepaald.
* Het de authentificatietoken TTL van HBA **moet minder dan of gelijk aan** zijn verfrist teken TTL.


*Beschrijving van de de authentificatiestroom van HBA voor het protocol OAuth 2.0*


| Handelingen van gebruikers | Systeemhandelingen |
|---|---|
| De gebruiker navigeert naar de site van de programmeur. Wanneer u probeert een video af te spelen, wordt de MVPD-kiezer weergegeven. De gebruiker selecteert hun MVPD en klikt login. | Er wordt een achtergrondcontrole uitgevoerd. MVPD past hun reeks regels voor gebruikersopsporing toe (bijvoorbeeld, kaart het IP van de gebruiker adres met het adres van MAC van verdeler-provisioned modems of breedband-verbonden reeks-top dozen). |
| Er wordt een scherm weergegeven dat ongeveer 3 seconden aanhoudt. Er kan een interstitiële pagina worden weergegeven om de gebruiker te laten weten dat hij/zij automatisch wordt aangemeld via hun MVPD-account. | <ol><li>AccessEnabler, die aan de kant van de programmeur geïnstalleerd is, verzendt een authentificatieverzoek (als HTTP- verzoek) naar het eindpunt van de Authentificatie van Adobe Pass.</li><li>Het eindpunt van de Authentificatie van Adobe Pass richt het verzoek aan het MVPD authentificatieeindpunt opnieuw. <br />**Nota:** het verzoek bevat de `hba_flag` parameter (poging HBA = waar) die signaleert dat MVPD authentificatie HBA zou moeten proberen.</li><li>Het MVPD authentificatieeindpunt verzendt een vergunningscode naar het eindpunt van de Authentificatie van Adobe Pass.</li><li>De Authentificatie van Adobe Pass gebruikt de vergunningscode om te verzoeken verfrist token en een toegangstoken van het symbolische eindpunt van MVPD.</li><li>De MVPD verzendt een verificatiebeslissing en de parameter `hba_status` (true/false) in de `id_token` .</li><li>Een vraag aan het MVPD eindpunt van het gebruikersprofiel wordt verzonden om de [ hba_status sleutel in gebruikersmeta-gegevens ](/help/authentication/user-metadata-feature.md#obtaining) bloot te stellen.</li><li>MVPD plaatst vernieuwt teken TTL aan een MVPD-Goedgekeurde waarde en de Adobe plaatst het teken AuthN TTL aan een waarde minder of gelijk aan de waarde van het vernieuwingstoken.</li></ol> |
| De gebruiker is geverifieerd en kan nu bladeren naar tv-inhoud overal. | Het verificatietoken wordt doorgegeven aan de gebruiker die nu met succes door de site van de programmeur kan bladeren. |

#### SAML-protocol {#saml-protocol}

Beschrijving van de HBA-verificatiestroom voor het SAML-verificatieprotocol

| Handelingen van gebruikers | Systeemhandelingen |
|---|---|
| De gebruiker navigeert naar de site van de programmeur. Wanneer u probeert een video af te spelen, wordt de MVPD-kiezer weergegeven. De gebruiker selecteert hun MVPD en klikt login. | Er wordt een achtergrondcontrole uitgevoerd. MVPD past hun reeks regels voor gebruikersopsporing toe (bijvoorbeeld, kaart het IP van de gebruiker adres met het adres van MAC van verdeler-provisioned modems of breedband-verbonden reeks-top dozen). |
| Er wordt een scherm weergegeven dat ongeveer 3 seconden aanhoudt. Er kan een interstitiële pagina worden weergegeven om de gebruiker te laten weten dat hij/zij automatisch wordt aangemeld via hun MVPD-account. | <ol><li>AccessEnabler, die aan de kant van de programmeur geïnstalleerd is, verzendt een authentificatieverzoek (als HTTP- verzoek) naar het eindpunt van de Authentificatie van Adobe Pass.</li><li>Het eindpunt van de Authentificatie van Adobe Pass richt het verzoek aan het MVPD authentificatieeindpunt opnieuw.</li><li>De MVPD moet een authenticatiebesluit verzenden in de vorm van een SAML-reactie die de HBA-markering moet bevatten: hba_status (true/false).</li><li>Een vraag aan het MVPD eindpunt van het gebruikersprofiel wordt verzonden om de [ hba_status sleutel in gebruikersmeta-gegevens ](/help/authentication/user-metadata-feature.md#obtaining) bloot te stellen.</li></ol> |
| De gebruiker is geverifieerd en kan nu bladeren naar tv-inhoud overal. | Het verificatietoken wordt doorgegeven aan de gebruiker die nu met succes door de site van de programmeur kan bladeren. |


## HBA activeren {#how-to-activate-hba}

* **protocol OAuth:**
   * Voor het toelaten van HBA zie, [ de Gids van de Gebruiker van het Dashboard van Adobe Pass TVE ](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-overview.md)
* **SAML protocol:** Op huis gebaseerde Authentificatie wordt geactiveerd op de kant MVPD. De programmeur of Adobe heeft geen actie nodig.
Voor meer informatie over MVPDs die huis Gebaseerde Authentificatie steunen, zie [ status HBA voor MVPDs ](/help/authentication/hba-status-mvpds.md).

## Veelgestelde vragen {#faqs}


**Vraag:** Waarom de scheiding tussen Huis Gebaseerde Authentificatie met SAML en protocollen OAuth2?

**Antwoord:** de stroom HBA is verschillend voor de twee protocollen. Vanuit het perspectief van de programmeur is er geen behoefte aan actie om ervoor te zorgen dat HBA voor SAML MVPDs wordt toegelaten, terwijl voor OAuth2 MVPDs, HBA in of uit in het Dashboard van Adobe Pass TVE kan worden in- en uitgeschakeld.



**Vraag:** zijn de gebruikers vereist om een gebruikersbenaming en een wachtwoord in te vullen de eerste keer zij voor authentiek verklaren wanneer HBA wordt toegelaten?

**Antwoord:** Nr, gebruikersbenaming en wachtwoord worden niet vereist.



**Vraag:** hoe u ouderlijke controles afdwingt?

**Antwoord 1:** de Adobe kan HBA voor integratie met kanalen onbruikbaar maken die ouderlijke controlegoedkeuring vereisen.

**Antwoord 2:** de Adobe werkt met OATC op een document van UX dat adviseert hoe te opstelling de ervaring van HBA met ouderlijke controles.



**Vraag:** hebben de leveranciers die HBA steunen kortere vensters van TTL voor HBA dan zij voor regelmatige authentificatie doen?

**Antwoord:** het plaatsen van TTL is configureerbaar. We raden u aan een kortere TTL in te stellen voor HBA-verificatietokens om foutafhandeling te voorkomen.


## Nuttige informatie {#useful-info}

* [ Onmiddellijke Toegang (HBA) Recommendations ](http://www.ctamtve.com/instantaccess) {target=_blank} - door CTAM
* [ de implementatie van de Steekproef van HBA op de app van de Programmer ](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/HBA_Flow_Sample.pdf?dc=201604222139-1346) {target=_blank} - door Adobe
  <!--* [Home Based Authentication User Experience Guidelines for TV Everywhere](http://oatc.us/Standards/DownloadRecommendedPractices.aspx){target=_blank} - by OATC-->
* ](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/Defining%20TVE%20Home-Based%20Authentication%20(HBA)%20%20Use%20Cases%20and%20Requirements%20Recommended%20Practice%20Version%201_0%20FINAL%20DRAFT%20FOR%20BOARD%20APPROVAL.pdf) {target=_blank} de Gevallen en Vereisten van het Gebruik van de Authentificatie van het Huis Gebaseerde [ door OATC
* [ Op huis gebaseerde Authentificatie Infographic ](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AdobeNewsletterHBA.pdf?dc=201604260953-2640) {target=_blank} - door Adobe
* [Verificatie met het OAuth 2.0-protocol](/help/authentication/authn-oauth2-protocol.md)
* [Verificatie met SAML MVPDs](/help/authentication/authn-usecase.md)
* [Gebruikershandleiding voor Adobe Pass TVE-dashboard](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-overview.md)
* [hba_status-gebruikersmetagegevens](/help/authentication/user-metadata-feature.md#obtaining)
