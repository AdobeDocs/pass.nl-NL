---
title: Proxy MVPD Web Service
description: Proxy MVPD Web Service
exl-id: f75cbc4d-4132-4ce8-a81c-1561a69d1d3a
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '955'
ht-degree: 0%

---

# Proxy MVPD-webservice {#proxy-mvpd-wbservice}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#overview-proxy-mvpd-webserv}

Een &quot;Volmacht MVPD&quot;is een MVPD die, naast het beheren van zijn eigen integratie met de Authentificatie van Adobe Pass, ook het machtigingsproces namens een groep bijbehorende &quot;Proxied MVPDs&quot; beheert. Deze regeling is transparant voor programmeurs.

Om de eigenschap ProxyMVPD uit te voeren, verstrekt de Authentificatie van Adobe Pass de Webdiensten RESTful, waarmee ProxyMVPDs lijsten van ProxiedMVPDs kan voorleggen en terugwinnen. Het protocol dat voor deze openbare API wordt gebruikt is REST HTTP, met de volgende veronderstellingen:

* De Volmacht MVPD gebruikt de methode van de GET van HTTP om de lijst van huidige geïntegreerde MVPDs terug te winnen.
* De Volmacht MVPD gebruikt de methode van de POST van HTTP om de lijst van gesteunde MVPDs bij te werken.

## Proxy MVPD-services {#proxy-mvpd-services}

* [Geavanceerde MVPD&#39;s ophalen](#retriev-proxied-mvpds)
* [Geavanceerde MVPD&#39;s verzenden](#submit-proxied-mvpds)

### Geavanceerde MVPD&#39;s ophalen {#retriev-proxied-mvpds}

Haalt de huidige lijst van Proxied MVPDs voor ProxyMVPD terug die door de apikey parameter wordt geïdentificeerd.

| Endpoint | Geroepen door | Aanvraagkoppen | HTTP-methode | HTTP-respons |
|---|---|---|---|---|
| &lt;fqdn>/control/v1/proxiedMvpds | ProxyMVPD | apikey (verplicht) | GET | <ul><li> 200 (ok) - De aanvraag is verwerkt en de reactie bevat een lijst met ProxiedMVPD&#39;s in XML-indeling</li><li>401 (niet-geautoriseerd) - Gebruikersverificatie vereist of autorisatie niet verleend voor opgegeven referenties.  Geeft een van de volgende opties aan:<ul><li>De apikey-token is niet aanwezig in de aanvraagkoptekst</li><li>Het verzoek is afkomstig van een IP-adres dat niet aanwezig is in de lijst van gewenste personen</li><li>Het token is niet geldig</li></ul></li><li>403 (niet toegestaan) - Geeft aan dat de bewerking niet wordt ondersteund voor de opgegeven parameters of dat de proxy-MVPD niet is ingesteld als een proxy of ontbreekt</li><li>405 (methode niet toegestaan) - Er is een andere HTTP-methode dan GET of POST gebruikt. De HTTP-methode wordt over het algemeen niet ondersteund of wordt niet ondersteund voor dit specifieke eindpunt.</li><li>500 (interne serverfout) - Er is een fout opgetreden aan de serverzijde tijdens het aanvraagproces.</li></ul> |

Voorbeeld van krullen:

`curl -X GET -H "apikey: ???provided-by-adobe???" "https://mgmt-prequal.auth-staging.adobe.com/control/v1/proxiedMvpds"`


Voorbeeld van XML-reactie:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxiedMvpds>
    <proxiedMvpd>
        <id>oneMvpdId</id>
        <displayName>MVPD Name</displayName>
        <logoURL></logoURL>
    </proxiedMvpd>
    <proxiedMvpd>
        <id ProviderID="ProviderID_Value_Sent_On_IdPEntry">mvpdPickerId</id>
        <displayName>MVPD Name Two</displayName>
        <logoURL></logoURL>
        <requestorIds>
            <requestorId>TheRequestorId_IntegratedWith</requestorId>
        </requestorIds>
    </proxiedMvpd>
    <proxiedMvpd>
        <id>anotherMvpdId</id>
        <displayName>Another MVPD</displayName>
        <logoURL></logoURL>
        <iframeSize>
            <iframeHeight>400</iframeHeight>
            <iframeWidth>340</iframeWidth>
        </iframeSize>
        <requestorIds>
            <requestorId>FirstIntegratedRequestorId</requestorId>
            <requestorId>SecondIntegratedRequestorId</requestorId>
        </requestorIds>
    </proxiedMvpd>
</proxiedMvpds>
```

### Geavanceerde MVPD&#39;s verzenden {#submit-proxied-mvpds}

Past een serie van MVPDs die met de Volmacht MVPD wordt geïntegreerd door de parameter apikey wordt geïdentificeerd.

| Endpoint | Geroepen door | Aanvraagkoppen | HTTP-methode | HTTP-respons |
|:------------------------------:|:---------:|:--------------------------------------------:|:-----------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| &lt;fqdn>/control/v1/proxiedMvpds | ProxyMVPD | apikey (Mandatory) proxied-mvpds (Mandatory) | POST | <ul><li>201 (gemaakt) - De push is verwerkt</li><li>400 (ongeldig verzoek) - De server weet niet hoe te om het verzoek te verwerken:<ul><li>Binnenkomende XML voldoet niet aan schema dat in deze specificatie wordt gepubliceerd</li><li>De proxy-mvpds hebben geen unieke id&#39;s</li><li>De geduwde aanvragerIds bestaat niet Andere de containerreden van Servlet voor 400 reactiecode</li></ul><li>401 (onbevoegd) - de apikey is ongeldig of bezoeker IP is niet op de lijst van gewenste personen</li><li>403 (niet toegestaan) - Geeft aan dat de bewerking niet wordt ondersteund voor de opgegeven parameters of dat de proxy-MVPD niet is ingesteld als een proxy of ontbreekt</li><li>405 (methode niet toegestaan) - Er is een andere HTTP-methode dan GET of POST gebruikt. De HTTP-methode wordt over het algemeen niet ondersteund of wordt niet ondersteund voor dit specifieke eindpunt.</li><li>500 (interne serverfout) - Er is een fout opgetreden aan de serverzijde tijdens het aanvraagproces.</li></ul> |

Voorbeeld van krullen:

`curl -X POST -H "apikey: <API_KEY>" "https://mgmt-prequal.auth.adobe.com/control/v1/proxiedMvpds" -d "proxied-mvpds=%3CproxiedMvpds%3E%3CproxiedMvpd%3E%3CdisplayName%3EFirst%20MVPD%20Name%3C%2FdisplayName%3E%3Cid%3EfirstMVPDId%3C%2Fid%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3C%2FproxiedMvpd%3E%3CproxiedMvpd%3E%3Cid%20ProviderID%3D%22ProviderID_Value_Sent_On_IdPEntry%22%3EmvpdPickerId%3C%2Fid%3E%3CdisplayName%3EMVPD%20Name%20Two%3C%2FdisplayName%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3CrequestorIds%3E%3CrequestorId%3ETHE_REQUESTOR_ID%3C%2FrequestorId%3E%3C%2FrequestorIds%3E%3C%2FproxiedMvpd%3E%3C%2FproxiedMvpds%3E"`



XML-voorbeeld:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxiedMvpds>
    <proxiedMvpd>
        <id>oneMvpdId</id>
        <displayName>MVPD Name</displayName>
        <logoURL></logoURL>
    </proxiedMvpd>
    <proxiedMvpd>
        <id ProviderID="ProviderID_Value_Sent_On_IdPEntry">mvpdPickerId</id>
        <displayName>MVPD Name Two</displayName>
        <logoURL></logoURL>
        <requestorIds>
            <requestorId>TheRequestorId_IntegratedWith</requestorId>
        </requestorIds>
    </proxiedMvpd>
    <proxiedMvpd>
        <id>anotherMvpdId</id>
        <displayName>Another MVPD</displayName>
        <logoURL></logoURL>
        <iframeSize>
            <iframeHeight>400</iframeHeight>
            <iframeWidth>340</iframeWidth>
        </iframeSize>
        <requestorIds>
            <requestorId>FirstIntegratedRequestorId</requestorId>
            <requestorId>SecondIntegratedRequestorId</requestorId>
        </requestorIds>
    </proxiedMvpd>
</proxiedMvpds>
```


### Posteringsfrequentie {#posting-frequency}

Adobe Pass Authentication adviseert dat ProxyMVPDs hun lijst van ProxiedMVPDs slechts zou moeten duwen wanneer er een verandering van de vorige duw is.

### Proxied MVPD&#39;s verwijderen {#delete-proxied-freqency}

Als ProxyMVPD een verslag van XML met een lege lijst ProxiedMVPDs duwt, zal die lege lijst in ons systeem enkel als om het even welke lijst worden opgeslagen, waarbij effectief het schrappen van de vorige lijst.



## XSD-indeling {#xsd-format}

De Adobe heeft het volgende toegelaten formaat voor het posten/het terugwinnen van proxyMVPDs van/aan onze openbare Webdienst bepaald:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
           xmlns:pxm="http://tve.adobe.com/data/proxiedmvpd"
           targetNamespace="http://tve.adobe.com/data/proxiedmvpd"
           elementFormDefault="qualified"
           version="1.0">
    <xs:complexType name="iframeSize">
        <xs:all>
            <xs:element name="iframeHeight" type="xs:int" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="iframeWidth" type="xs:int" minOccurs="1" maxOccurs="1" nillable="false"/>
        </xs:all>
    </xs:complexType>
    <xs:complexType name="requestorIds">
        <xs:annotation>
            <xs:documentation>List of requestors/programmers integrated with the proxied MVPD</xs:documentation>
        </xs:annotation>
        <xs:sequence>
            <xs:element name="requestorId" type="xs:string" minOccurs="1" maxOccurs="unbounded" nillable="false">
                <xs:annotation>
                    <xs:documentation>The requestor/programmer identifier recognized by Adobe</xs:documentation>
                </xs:annotation>
            </xs:element>
        </xs:sequence>
    </xs:complexType>
    <xs:complexType name="proxiedMvpd">
        <xs:all>
            <xs:element name="id" minOccurs="1" maxOccurs="1" nillable="false">
                <xs:annotation>
                    <xs:documentation>The id must conform to the regular expression: ([a-zA-Z0-9]+((\-)|[_])*)</xs:documentation>
                </xs:annotation>
                <xs:complexType>
                    <xs:simpleContent>
                        <xs:extension base="xs:string">
                            <xs:attribute name="ProviderID">
                                <xs:simpleType>
                                    <xs:restriction base="xs:string">
                                        <xs:minLength value="1"/>
                                        <xs:maxLength value="128"/>
                                    </xs:restriction>
                                </xs:simpleType>
                            </xs:attribute>
                        </xs:extension>
                    </xs:simpleContent>
                </xs:complexType>
            </xs:element>
            <xs:element name="displayName" type="xs:string" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="logoURL" type="xs:anyURI" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="iframeSize" type="pxm:iframeSize" minOccurs="0" maxOccurs="1"/>
            <xs:element name="requestorIds" type="pxm:requestorIds" minOccurs="0" maxOccurs="1"/>
        </xs:all>
    </xs:complexType>
    <xs:element name="proxiedMvpds">
        <xs:annotation>
            <xs:documentation>List of Proxied MVPD</xs:documentation>
        </xs:annotation>
        <xs:complexType>
            <xs:sequence>
                <xs:element name="proxiedMvpd" type="pxm:proxiedMvpd" minOccurs="0" maxOccurs="unbounded"/>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

**Opmerkingen over elementen:**

* `id` (verplicht) - De Proxied MVPD ID moet een koord relevant voor de naam van MVPD zijn, gebruikend om het even welke volgende karakters (aangezien het aan Programmers voor het volgen van doeleinden zal worden blootgesteld):
   * Alfanumerieke tekens, onderstrepingsteken (&quot;_&quot;) en koppelteken (&quot;-&quot;).
   * De idID moet voldoen aan de volgende reguliere expressie:
     `(a-zA-Z0-9((-)|_)*)`

     Het moet dus ten minste één teken hebben, beginnen met een letter en doorgaan met een letter, cijfer, streepje of onderstrepingsteken.

* `iframeSize` (optioneel) - Het iframeSize-element is optioneel en definieert de grootte van het iFrame als de MVPD-verificatiepagina zich in een iFrame moet bevinden. Anders, als het iframeSize element niet aanwezig is, zal de authentificatie in volledige browser opnieuw richten pagina gebeuren.
* `requestorIds` (optioneel) - De waarden voor requestIds worden opgegeven door Adobe. Een vereiste is dat een proxy MVPD met minstens één aanvragerId moet worden geïntegreerd. Als de tag &quot;requestIds&quot; niet aanwezig is op het proxy-element MVPD, wordt die proxy MVPD geïntegreerd met alle beschikbare aanvragers die zijn geïntegreerd onder de proxy MVPD.
* `ProviderID` (facultatief) - wanneer het attribuut ProviderID op het id element aanwezig is, zal de waarde van ProviderID op het SAML authentificatieverzoek naar Volmacht MVPD als Proxied MVPD/SubMVPD ID (in plaats van de id waarde) worden verzonden. In dit geval, zal de waarde van identiteitskaart slechts in de plukker worden gebruikt MVPD die op de pagina van de Programmer wordt voorgesteld, en intern door de Authentificatie van Adobe Pass. De lengte van het attribuut ProviderID moet tussen 1 en 128 karakters zijn.

## Beveiliging {#security}

Een verzoek kan alleen als geldig worden beschouwd als het aan de volgende regels voldoet:

* De aanvraagheader moet de beveiligingstoepassingsparameter bevatten. (Dit is een toepassingssleutel die de vraag van de Volmacht MVPD uniek zal identificeren.)
* Het verzoek moet van een specifiek IP adres komen dat is toegestaan.
* De aanvraag moet via het SSL-protocol worden verzonden.

Adobe geeft de (statische) waarde van het token op. Deze waarde wordt gebruikt in het verificatie- en autorisatieproces.  Alle parameters in de aanvraagkoptekst die hierboven niet worden vermeld, worden genegeerd.

Voorbeeld van krullen:

`curl -X GET -H "apikey: ???provided-by-adobe???" "https://mgmt-prequal.auth-staging.adobe.com/control/v1/proxiedMvpds"`

## De Eindpunten van de Dienst van het Web van de Volmacht MVPD voor de Milieu&#39;s van de Authentificatie van Adobe Pass {#proxy-mvpd-wevserv-endpoints}

* **URL productie:** https://mgmt.auth.adobe.com/control/v1/proxiedMvpds
* **Staging-URL:** https://mgmt.auth-staging.adobe.com/control/v1/proxiedMvpds
* **URL voor preQual-Production:** https://mgmt-prequal.auth.adobe.com/control/v1/proxiedMvpds
* **PreQual-Staging URL:** https://mgmt-prequal.auth-staging.adobe.com/control/v1/proxiedMvpds

<!--
>[!RELATEDINFORMATION]
>* [Proxy MVPD SAML integration](/help/authentication/proxy-mvpd-saml-int.md)
>* [User metadata exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Technical paper](/help/authentication/technical-paper.md)
>* [Adobe Pass Authentication glossary](/help/authentication/glossary.md)
-->
