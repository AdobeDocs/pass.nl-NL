---
title: MVPD-autorisatie
description: MVPD-autorisatie
exl-id: 215780e4-12b6-4ba6-8377-4d21b63b6975
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 0%

---

# MVPD-autorisatie

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#mvpd-authz-overview}

De vergunning (AuthZ) wordt uitgevoerd via backchannel (server-aan-server) mededelingen tussen een Adobe-ontvangen backend server en het eindpunt van AuthZ van MVPD.

Voor verzoeken AuthZ, zou het vergunningseindpunt minstens de volgende parameters moeten kunnen verwerken:

* **Uid**. De gebruikers-id die is ontvangen van de verificatiestap.

* **identiteitskaart van het Middel**. Een tekenreeks die een bepaalde inhoudsbron identificeert. Deze resource-id wordt opgegeven door de programmeur en de MVPD moet de bedrijfsregels voor deze bronnen versterken (bijvoorbeeld door te controleren of de gebruiker is geabonneerd op een bepaald kanaal).

Naast het bepalen of de gebruiker een vergunning heeft, moet het antwoord de tijd-aan-levende (TTL) van deze vergunning omvatten, d.w.z. wanneer de vergunning verloopt. Als TTL niet wordt geplaatst, zal het verzoek AuthZ ontbreken.  Om deze reden, **TTL is een verplichte configuratie die op de kant van de Authentificatie van Adobe Pass** plaatst, om het geval te behandelen wanneer een MVPD TTL niet in hun verzoek omvat.

## De vergunningsaanvraag {#authz-req}

Een AuthZ-verzoek moet een onderwerp bevatten namens wie het verzoek wordt gedaan, de bron(nen) waartoe het onderwerp toegang probeert te krijgen, de actie die het onderwerp probeert uit te voeren op de bron en de omgeving waarin de bewerking op het punt staat plaats te vinden. In het specifieke geval van Adobe Pass Authentication, komen deze elementen overeen met:

| XACML-element | Komt overeen met |
|---------------|--------------------------------------------------------------------------------------------------------------------------------|
| Onderwerp | Het hoofd dat door de Voor authentiek verklaarde Zitting wordt geïdentificeerd, die door &quot;subject-token&quot;AttributeValue van de bevestiging van SAML wordt van verwijzingen voorzien. |
| Bron | A URI for the Protected Resource. |
| Handeling | WEERGEVEN. |
| Omgeving | Omvat IP adres van de vragende cliënt, zoals die door SP wordt gezien. |



SP op dit punt moet een XACML Authorization DecisionQuery voorbereiden en het verzenden (via HTTP POST) naar het (eerder-overeengekomen) Punt van het Besluit van het Beleid (PDP) voor IdP. Hieronder ziet u een voorbeeld van een eenvoudig XACML-verzoek (zie de XACML-kernspecificatie):

```XML
POST https://authz.site.com/XACML_endpoint
<Request  xmlns="urn:oasis:names:tc:xacm:2.0:context:schema:os"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="urn:oasis:names:tc:xacml:2.0:context:schema:os
http://docs.oasis-open.org/xacml/access_control-xacml-2.0-context-schema-os.xsd">
<Subject>
   <Attribute
        AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-token"
        DataType="http://www.w3.org/2001/XMLSchema#base64Binary">
      <AttributeValue>{Base64 Data}</AttributeValue>
   </Attribute>
</Subject>
<Resource>
   <Attribute
        AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id"
        DataType="http://www.w3.org/2001/XMLSchema#anyURI">
<AttributeValue>urn:tve:tms:1234</AttributeValue>
   </Attribute>
</Resource>
<Action>
   <Attribute
        AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id"
        DataType="http://www.w3.org/2001/XMLSchema#string">
       <AttributeValue>VIEW</AttributeValue>
   </Attribute>
</Action>
<Environment>
   <Attribute
       AttributeId="urn:oasis:names:tc:xacml:1.0:subject:authn-locality:ip-address"
       DataType="http://www.w3.org/2001/XMLSchema#string">
      <AttributeValue>1.2.3.4</AttributeValue>
   </Attribute>
</Environment>
</Request>
```


Na het ontvangen van het verzoek AuthZ, evalueert PDP van MVPD het verzoek en bepaalt of het onderwerp de gevraagde actie op het middel zou moeten mogen uitvoeren. De MVPD geeft vervolgens een antwoord met een beslissing, statuscode en bericht, zoals hieronder beschreven in de machtigingsreactie.

## De machtigingsreactie {#authz-response}

De reactie op het verzoek AuthZ komt nadat MVPD het verzoek evalueert en past de gevraagde bedrijfsregels toe om te bepalen als het onderwerp de gevraagde actie op het middel mag uitvoeren. De teruggekeerde Reactie op de Authentificatie van Adobe Pass wordt opnieuw uitgedrukt na de XACML kernspecificatie met een Besluit, een code van de Status, een bericht, en de Verplichtingen die SP als het Punt van de Handhaving van het Beleid (PEP) heeft. Hier volgt een voorbeeld van een reactie:

```XML
<Response xmlns="urn:oasis:names:tc:xacml:2.0:context:schema:os">
  <Result>
  <Decision>Permit</Decision>
  <Status>
     <StatusCode Value="urn:oasis:names:tc:xacml:1.0:status:ok"/>
     <StatusMessage>ok</StatusMessage>
  </Status>
  <xacml:Obligations     
          xmlns:xacml="urn:oasis:names:tc:xacml:2.0:policy:schema:os">
     <xacml:Obligation    
              ObligationId="urn:cablelabs:olca:1.0:obligations:log"
              FulfillOn="Permit" />
  </xacml:Obligations>
 </Result>
</Response>
```

Hieronder volgt een lijst met DENY-verplichtingen die door Adobe Pass Authentication worden ondersteund en waarmee programmeurs kunnen voldoen:

* **urn :tve: xacml:2.0 :obligations: beperking-pc** - de abonnee heeft een ouderlijke controlecontrole ontbroken en SP moet passende maatregelen nemen om toegang tot deze inhoud te beperken.

* **urn :tve: xacml:2.0 :obligations: verbetering** - de abonnee heeft geen aangewezen abonnementsniveau.  U moet een upgrade uitvoeren op uw abonnement om toegang te krijgen tot inhoud.

De Authentificatie van Adobe Pass steunt de volgende **Verplichtingen van de VERGUNNING** en laat programmeurs toe om hen te vervullen:

* **urn :cablelabs: olca:1.0 :obligations: logboek** - Adobe Pass registreert de transactie en kan beschikbaar maken via het overeengekomen rapporteringsmechanisme.

* **urn :cablelabs: olca:1.0 :obligations: re-authz** - de Authentificatie van Adobe Pass verfrist opnieuw de vergunning in n seconden (als argument aan de Verplichting via een XACML AttributeAssignment wordt gespecificeerd - zie XACML kernspecificatie, Sectie 5.46).

<!--
>![RelatedInformation]
>* [Preflight Authorization](/help/authentication/preflight-authz.md)
>* [Authentication](/help/authentication/authn-usecase.md)
-->
