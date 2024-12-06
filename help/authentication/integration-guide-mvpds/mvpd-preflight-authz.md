---
title: Preflight-machtiging voor MVPD
description: Preflight-machtiging voor MVPD
exl-id: da2e7150-b6a8-42f3-9930-4bc846c7eee9
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '750'
ht-degree: 0%

---

# Preflight-machtiging voor MVPD

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Inleiding {#mvpd-preflight-authz-intro}

&quot;Preflight-autorisatie&quot; is een eenvoudige machtigingscontrole voor meerdere bronnen. Programmeurs gebruiken het hoofdzakelijk om hun UIs te versieren (bijvoorbeeld, die op toegangsstatus met slot en ontgrendelingspictogrammen wijzen).

De Authentificatie van Adobe Pass kan Preflight Authorization momenteel op twee manieren voor MVPDs, of via AuthN reactiekenmerken of via een multichannel verzoek AuthZ steunen.  In de volgende scenario&#39;s worden de kosten/baten beschreven van de verschillende manieren waarop u Preflight-autorisatie kunt implementeren:

* **het Hoogste scenario van het Geval** - MVPD verstrekt de lijst van vooraf gemachtigde middelen tijdens de vergunningsfase (Multikanaal AuthZ).
* **het slechtste scenario van het Geval** - als een MVPD geen vorm van veelvoudige middelenvergunning steunt, voert de server van de Authentificatie van Adobe Pass een vergunningsvraag aan MVPD voor elk middel in de middelenlijst uit. Dit scenario heeft een effect (in verhouding tot het aantal middelen) op de reactietijd voor het Preflight vergunningsverzoek. Hierdoor kan de belasting van zowel Adobe- als MVPD-servers toenemen, wat prestatieproblemen veroorzaakt. Ook zal het vergunningsverzoeken/reactiegebeurtenissen zonder de daadwerkelijke behoefte aan een spel produceren.
* **Vervangen** - MVPD verstrekt de lijst van vooraf gemachtigde middelen tijdens de authentificatiefase, zodat zal er geen netwerkvraag nodig zijn, zelfs niet de preflight verzoek, aangezien de lijst op de cliënt in het voorgeheugen ondergebracht is.

Hoewel MVPDs geen Preflight vergunning moet steunen, beschrijven de volgende secties sommige Preflight vergunningsmethodes die de Authentificatie van Adobe Pass kan steunen, alvorens terug te vallen naar het worstcasescenario hierboven.

## Preflight in AuthN {#preflight-authn}

Dit Preflight-scenario is compatibel met OLCA (Cableabs). De authentificatie en de Authorization Interface 1.0 sectie 7.5.2 van de Specificatie getiteld &quot;De Verklaring van Attributen binnen de Bevestiging van de Authentificatie&quot;, beschrijft hoe een authentificatiereactie SAML een lijst van vooraf erkende middelen kan bevatten. Als een IdP dit steunt, zal de server van de Authentificatie van Adobe Pass de Preflight lijst van middelen bij authentificatietijd kunnen produceren en het op de cliënt samen met de Token van de Authentificatie in het voorgeheugen onderbrengen. Deze methode bereikt ook het beste casescenario, en geen netwerkvraag zal worden uitgevoerd wanneer de programmeur checkPreauthorisedResources () roept, aangezien alles reeds op de cliënt is.

### Lijst met aangepaste bronnen in SAML-kenmerkinstructie {#custom-res-saml-attr}

De de authentificatiereactie van SAML van IdP omvat een AttributeStatement die middelnamen bevat die AdobePass zou moeten machtigen.  Sommige MVPD&#39;s verstrekken dit in het volgende formaat:

```XML
<saml:AttributeStatement>
  <saml:Attribute Name="authorized_resources">
    <saml:AttributeValue>MMOD</saml:AttributeValue>
    <saml:AttributeValue>Olympics2012</saml:AttributeValue>
  </saml:Attribute>
</saml:AttributeStatement>
```

Het bovenstaande voorbeeld bevat een lijst met twee vooraf geautoriseerde bronnen: &quot;MMOD&quot; en &quot;Olympische Spelen 2012&quot;.

Dit bereikt effectief het beste casescenario, en geen netwerkvraag zal worden uitgevoerd wanneer de programmeur checkPreauthorisedResources () roept, aangezien alles reeds op de cliënt is.

## Preflight van meerdere kanalen in AuthZ {#preflight-multich-authz}

Deze Preflight-implementatie is ook compatibel met OLCA (Cablelabs).  De Authentificatie en van de Authorization Interface 1.0 Specificatie (secties 7.5.3 en 7.5.4) beschrijft methodes om de informatie van de Vergunning van een MVPD te verzoeken gebruikend of SAML Assertions of XACML. Dit is de geadviseerde manier om vergunningsstatus voor MVPDs te vragen die dit als deel van de stroom van de Authentificatie niet steunen. De Authentificatie van Adobe Pass geeft één enkele netwerkvraag aan MVPD uit om de lijst van erkende middelen terug te winnen.


De Authentificatie van Adobe Pass ontvangt de lijst van middelen van de toepassing van de Programmer. De integratie van MVPD van de Authentificatie van Adobe Pass kan één vraag AuthZ met al die middelen dan maken, en dan de reactie ontleden en de veelvoudige vergunning/ontkennen besluiten halen.  De stroom voor de preflight met multi-channel scenario AuthZ werkt als volgt:

1. De app van de programmeur verzendt een komma gescheiden lijst van middelen door de Preflight cliënt API, bijvoorbeeld: &quot;TestChannel1,TestChannel2,TestChannel3&quot;.
1. De Preflight AuthZ- verzoekvraag MVPD bevat de veelvoudige middelen, en heeft de volgende structuur:

```XML
<?xml version="1.0" encoding="UTF-8"?><soap11:Envelope xmlns:soap11="http://schemas.xmlsoap.org/soap/envelope/"> 
<soap11:Header/> 
<soap11:Body> 
  <xacml-samlp:XACMLAuthzDecisionQuery xmlns:xacml-samlp="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:protocol" 
                                       CombinePolicies="false" Destination="https://login.idpexmaple.net/" ID="_3576604f382455d6495f342d9e07b69c" 
                                       IssueInstant="2013-02-07T10:31:40.333Z" Version="2.0"> 
  <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion">https://saml.sp.auth-staging.adobe.com/on-behalf-of/TestDistributors</saml2:Issuer> 
  <xacml-context:Request xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os"> 
  <xacml-context:Subject SubjectCategory="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject"> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-id" DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">VFZTAQEAABQCe[...]</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Subject> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">TestChannel1</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">TestChannel2</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                                xsi:type="xacml-context:AttributeValueType">TestChannel3</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Action> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">VIEW</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Action> 
  <xacml-context:Environment> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:subject:authn-locality:ip-address" 
                           DataType="urn:oasis:names:tc:xacml:2.0:data-type:ipAddress"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">127.0.0.1</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Environment> 
  </xacml-context:Request> 
  </xacml-samlp:XACMLAuthzDecisionQuery> 
</soap11:Body> 
</soap11:Envelope>
```

## Aangepaste autorisatie voor meerdere bronnen {#custom-authz}

Sommige MVPDs hebben toestemmingseindpunten die vergunning voor veelvoudige middelen in één verzoek steunen, maar zij vallen niet onder het scenario dat in Multikanaal AuthZ wordt beschreven. Deze specifieke MVPDs vereist douanewerk.

De Adobe kan veelvoudige kanaalvergunning zonder veranderingen in de bestaande implementatie ook steunen.  Deze aanpak moet tussen Adobe en het technische team van de MVPD worden herzien om ervoor te zorgen dat het naar behoren functioneert.

## MVPD&#39;s die Preflight-autorisatie ondersteunen {#mvpds-supp-preflight-authz}

De volgende lijst maakt een lijst van MVPDs die Preflight Vergunning, samen met het type van preflight steunen zij en bekende beperkingen:

| Preflight-benadering | MVPD | Notities |
|:-------------------------------:|:--------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------:|
| AuthZ met meerdere kanalen | AT&amp;T Proxy Clearleap Charter_Direct Proxy GLDS Rogers Verizon OSN Bell Sasktel Optimum AlticeOne comprimeren |                                                                    |
| Kanaalverbinding in gebruikersmetagegevens | Suddenlink HTC | Alle directe integraties van de Synacor kunnen deze benadering eveneens steunen. |
| Vork en verbinding | Alle andere niet hierboven vermelde | Het standaard maximum aantal gecontroleerde middelen = 5. |

<!--
![RelatedInformation]
>* [Logout](/help/authentication/usecase-mvpd-logout.md)
>* [Authorization](/help/authentication/authz-usecase.md)
>* [MVPD Integration Features](/help/authentication/mvpd-integr-features.md)
>* [MVPD User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Preflight Authorization - Programmer Integration Guide](/help/authentication/preflight-authz.md)
>* [AuthN and AuthZ Interface 1.0 Specification](https://www.cablelabs.com/specifications/CL-SP-AUTH1.0-I04-120621.pdf){target=_blank} 
-->
