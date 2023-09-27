---
title: MVPD Content Metadata Exchange
description: MVPD Content Metadata Exchange
exl-id: d17e60dc-6c61-4ca2-bad8-1840c95261e0
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '404'
ht-degree: 0%

---

# MVPD Content Metadata Exchange

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#content-metadat-exchange-overview}

Deze pagina beschrijft twee standaardimplementaties die de Authentificatie van Adobe Pass gebruikt om gestructureerde gegevens naar MVPDs op het vergunningsverzoek te verzenden.  De gestructureerde gegevens vertegenwoordigen de bron (de programmeur) die het verzoek indient en eventueel aanvullende gegevens zoals de classificatie van inhoud.

Aan de programmeerzijde ondersteunt Adobe Pass Authentication gestructureerde MRSS-gegevensbronnen als volgt:

1. De programmeur verzendt het Middel als koord MRSS. Adobe Pass-verificatie codeert deze niet op de client voor web- of native apparaten. De MRSS wordt als een normale tekenreeks verzonden naar de Adobe Pass-verificatieserver.
1. Aan de serverzijde wordt MRSS gevalideerd tegen het vooraf gedefinieerde schema (http://search.yahoo.com/mrss/).  Als de validatie slaagt, haalt Adobe Pass Authentication de informatie uit de MRSS-velden, waaronder:
   * kanaaltitel
   * objecttitel
   * resource-id
   * ratingwaarde en -type
1. De waarden die uit de MRSS worden geëxtraheerd, worden gebruikt om de vergunningsaanvraag op te bouwen die aan de MVPD wordt doorgegeven.

De Authentificatie van Adobe Pass steunt twee benaderingen voor het omzetten van MRSS in formaten die door MVPDs worden gesteund:

* **XACML**.  De eerste aanpak wordt afgestemd op de OLCA-standaard.  Het gebruikt XACML, waarin de waarden MRSS worden gehaald om een XACMLResource met attributen te bouwen die aan de elementen MRSS in kaart brengen.  Dit wordt dan overgegaan tot MVPD.
* **REST**.  De tweede aanpak is REST-gebaseerd.  MRSS is base64 gecodeerd en overgegaan als parameter URL op de vraag REST.

In beide benaderingen verwerkt het MVPD het vergunningsverzoek door de geëxtraheerde waarden in zijn eigen logische stroom op te nemen en een reactie op de toelating te retourneren.

## Integratiedetails {#integration-details}

* Op OLCA gebaseerde XACML gestructureerde bron
* Op REST gebaseerde gestructureerde bron

### Op OLCA gebaseerde XACML gestructureerde bron {#olca-based-xacml-struc-resource}

De meeste kabel-georiënteerde MVPDs gebruiken de op XACML-Gebaseerde benadering, maar steunen nog niet de volledige gestructureerde gegevensbenadering.  Andere MVPDs die XACML steunen neemt de Titel van het Kanaal en keurt dat voor de attributen ResourceID goed. In het onderstaande voorbeeld ziet u de volledige gestructureerde XACML-benadering. Het team van de Authentificatie van Adobe Pass adviseert dat voor MVPDs die XACML gebruiken, maar nog geen eigenschappen zoals ouderlijke controles steunen, zij hun integratie XACML aan het volgende voorbeeld zouden moeten aanpassen:

```XML
<?xml version="1.0" encoding="UTF-8"?>
<soap11:Envelope xmlns:soap11=">
    <soap11:Header/>
    <soap11:Body>
        <xacml-samlp:XACMLAuthzDecisionQuery
                xmlns:xacml-samlp="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:protocol"
                Destination="
                ID="_f1dd34469c5aeac016760e51dbba007d" IssueInstant="2012-06-26T16:30:24.879Z" Version="2.0">
            <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
                https://saml.sp.auth.adobe.com/
            </saml:Issuer>
            <ds:Signature xmlns:ds=">.......</ds:Signature>
            <xacml-context:Request xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os">
                [....info skipped for brevity....]
                <xacml-context:Resource>
 
        // The MRSS item GUID is passed as the XACML Resource resource-id
                    <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id">
                        <xacml-context:AttributeValue>DISNEY_GUID_12345</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
        // The MRSS channel title is passed as the XACML Resource tv-network
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:tv-network">
                        <xacml-context:AttributeValue>Disney</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
        // Adobe doesn't yet support an explicit namespace for the GUID, so we reuse the channel title as the GUID.  
        // We expect to add an explicit namespace later next year pulling it from the GUID scheme attribute.
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:id:namespace">
                        <xacml-context:AttributeValue>Disney</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
        // The MRSS item title is passed as the XACML Resource content title
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:title">
                        <xacml-context:AttributeValue>Disney Program X</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
        // The MRSS media rating is passed as the XACML Resource content rating 
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:rating:vchip">
                        <xacml-context:AttributeValue>TV-Y</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
                </xacml-context:Resource>
 
                <xacml-context:Action>
                    <xacml-context:Attribute>
                        <xacml-context:AttributeValue>VIEW</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
                </xacml-context:Action>
 
                [.....info skipped for brevity....]
            </xacml-context:Request>
        </xacml-samlp:XACMLAuthzDecisionQuery>
    </soap11:Body>
</soap11:Envelope>
 
//formatted for readability
```

### Op REST gebaseerde gestructureerde bron {#rest-based-struct-resource}

Sommige MVPDs hebben op het volgende REST-Gebaseerde protocol voor vergunning gestandaardiseerd. Deze benadering is zo volledig gespiegeld als de benadering XACML, maar verstrekt een &quot;lichtere gewicht&quot;implementatie.

`// The MRSS is base64 encoded by Adobe Pass Authentication, and passed in that format to the REST-based Authorization endpoint.`

`https://auth.somedomain.net/mediation/1/rest/client/authz?uuID=AC82CE4&mrss=base64encodedstring&IPAddress=123.456.78.901`

<!--
>[!RELATEDINFORMATION]
>* [User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Logout](/help/authentication/usecase-mvpd-logout.md)
>* [Programmer Integration Guide: Identifying Protected Resources](/help/authentication/identify-protected-resources.md)
>* [Programmer Integration Guide: User Metadata Exchange](/help/authentication/user-metadata.md)
-->
