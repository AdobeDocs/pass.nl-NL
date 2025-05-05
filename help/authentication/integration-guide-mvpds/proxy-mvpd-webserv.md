---
title: Proxy MVPD Web Service
description: Proxy MVPD Web Service
exl-id: f75cbc4d-4132-4ce8-a81c-1561a69d1d3a
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1027'
ht-degree: 0%

---


# Proxy MVPD-webservice {#proxy-mvpd-wbservice}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Voordat u de Proxy MVPD-webservice gebruikt, moet u controleren of aan de volgende voorwaarden is voldaan:
>
> * Haal de cliëntgeloofsbrieven zoals die in [ worden beschreven terug cliëntgeloofsbrieven ](../integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API documentatie.
> * Haal het toegangstoken zoals die in [ wordt beschreven terug toegangstoken ](../integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) API documentatie.
>
> Verwijs naar de [ Dynamische documentatie van het Overzicht van de Registratie van de Cliënt ](../integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) voor meer informatie over hoe te om een geregistreerde toepassing tot stand te brengen en de softwareverklaring te downloaden.

## Overzicht {#overview-proxy-mvpd-webserv}

Een &quot;Volmacht MVPD&quot;is een MVPD die, naast het beheren van zijn eigen integratie met de Authentificatie van Adobe Pass, ook het machtigingsproces namens een groep bijbehorende &quot;Proxied MVPDs&quot; beheert. Deze regeling is transparant voor programmeurs.

Om de eigenschap ProxyMVPD uit te voeren, verstrekt de Authentificatie van Adobe Pass de Webdiensten RESTful, waarmee ProxyMVPDs lijsten van ProxiedMVPDs kan voorleggen en terugwinnen. Het protocol dat voor deze openbare API wordt gebruikt is REST HTTP, met de volgende veronderstellingen:

&#x200B;- Proxy MVPD gebruikt de methode van de GET van HTTP om de lijst van huidige geïntegreerde MVPDs terug te winnen.
&#x200B;- Proxy MVPD gebruikt de methode van de POST van HTTP om de lijst van gesteunde MVPDs bij te werken.

## Proxy MVPD-services {#proxy-mvpd-services}

&#x200B;- [ wint proxy MVPDs ](#retriev-proxied-mvpds) terug
&#x200B;- [ voorlegt proxy MVPDs ](#submit-proxied-mvpds)

### Geavanceerde MVPD&#39;s ophalen {#retriev-proxied-mvpds}

Haalt de huidige lijst van Proxied MVPDs terug die met de geïdentificeerde Volmacht MVPD wordt geïntegreerd.

| Endpoint | Geroepen door | Parameters aanvragen | Aanvraagkoppen | HTTP-methode | HTTP-respons |
|--------------------------------------------------------------------------|-----------|-----------------------|---------------------------|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| &lt;FQDN>/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds | ProxyMVPD | proxy-mvpd-identifier | Vergunning (verplicht) | GET | <ul><li> 200 (ok) - De aanvraag is verwerkt en de reactie bevat een lijst met ProxiedMVPD&#39;s in XML-indeling</li><li>401 (niet-geautoriseerd) - Geeft een van de volgende gegevens aan:<ul><li>De cliënt MOET om een nieuw access_token verzoeken</li><li>Het verzoek is afkomstig van een IP-adres dat niet aanwezig is in de lijst van gewenste personen</li><li>Het token is niet geldig</li></ul></li><li>403 (niet toegestaan) - Geeft aan dat de bewerking niet wordt ondersteund voor de opgegeven parameters of dat de proxy-MVPD niet is ingesteld als een proxy of ontbreekt</li><li>405 (methode niet toegestaan) - Er is een andere HTTP-methode dan GET of POST gebruikt. De HTTP-methode wordt over het algemeen niet ondersteund of wordt niet ondersteund voor dit specifieke eindpunt.</li><li>500 (interne serverfout) - Er is een fout opgetreden aan de serverzijde tijdens het aanvraagproces.</li></ul> |

Voorbeeld van krullen:

`curl -X GET -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/ProxyMVPD_Adobe/mvpds"`


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

Past een serie van MVPDs die met geïdentificeerde Volmacht MVPD wordt geïntegreerd.

| Endpoint | Geroepen door | Parameters aanvragen | Aanvraagkoppen | HTTP-methode | HTTP-respons |
|:------------------------------------------------------------------------:|:---------:|-----------------------|:---------------------------------------------------:|:-----------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| &lt;FQDN>/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds | ProxyMVPD | proxy-mvpd-identifier | Vergunning (Verplicht) proxied-mvpds (Verplicht) | POST | <ul><li>201 (gemaakt) - De push is verwerkt</li><li>400 (ongeldig verzoek) - De server weet niet hoe te om het verzoek te verwerken:<ul><li>Binnenkomende XML voldoet niet aan schema dat in deze specificatie wordt gepubliceerd</li><li>De proxy-mvpds hebben geen unieke id&#39;s</li><li>De geduwde aanvragerIds bestaat niet Andere de containerreden van Servlet voor 400 reactiecode</li></ul><li>401 (niet-geautoriseerd) - Geeft een van de volgende gegevens aan:<ul><li>De cliënt MOET om een nieuw access_token verzoeken</li><li>Het verzoek is afkomstig van een IP-adres dat niet aanwezig is in de lijst van gewenste personen</li><li>Het token is niet geldig</li></ul></li><li>403 (niet toegestaan) - Geeft aan dat de bewerking niet wordt ondersteund voor de opgegeven parameters of dat de proxy-MVPD niet is ingesteld als een proxy of ontbreekt</li><li>405 (methode niet toegestaan) - Er is een andere HTTP-methode dan GET of POST gebruikt. De HTTP-methode wordt over het algemeen niet ondersteund of wordt niet ondersteund voor dit specifieke eindpunt.</li><li>500 (interne serverfout) - Er is een fout opgetreden aan de serverzijde tijdens het aanvraagproces.</li></ul> |

Voorbeeld van krullen:

`curl -X POST -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth.adobe.com/control/v3/mvpd-proxies/ProxyMVPD_Adobe/mvpds" -d "proxied-mvpds=%3CproxiedMvpds%3E%3CproxiedMvpd%3E%3CdisplayName%3EFirst%20MVPD%20Name%3C%2FdisplayName%3E%3Cid%3EfirstMVPDId%3C%2Fid%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3C%2FproxiedMvpd%3E%3CproxiedMvpd%3E%3Cid%20ProviderID%3D%22ProviderID_Value_Sent_On_IdPEntry%22%3EmvpdPickerId%3C%2Fid%3E%3CdisplayName%3EMVPD%20Name%20Two%3C%2FdisplayName%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3CrequestorIds%3E%3CrequestorId%3ETHE_REQUESTOR_ID%3C%2FrequestorId%3E%3C%2FrequestorIds%3E%3C%2FproxiedMvpd%3E%3C%2FproxiedMvpds%3E"`



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

**Nota&#39;s op elementen:**

&#x200B;- `id` (verplicht) - De proxy-id voor MVPD moet een tekenreeks zijn die relevant is voor de naam van de MVPD, waarbij een van de volgende tekens wordt gebruikt (aangezien deze voor traceringsdoeleinden aan programmeurs wordt getoond):
&#x200B;- Alfanumerieke tekens, onderstrepingsteken (&quot;_&quot;) en koppelteken (&quot;-&quot;).
&#x200B;- De idID moet voldoen aan de volgende reguliere expressie:
`(a-zA-Z0-9((-)|_)*)`

     Aldus moet het minstens één karakter hebben, met een brief beginnen, en met om het even welke brief, cijfer, streepje, of onderstrepingsteken verdergaan.

&#x200B;- `iframeSize` (optioneel) - Het element iframeSize is optioneel en definieert de grootte van het iFrame als de MVPD-verificatiepagina zich in een iFrame moet bevinden. Anders, als het iframeSize element niet aanwezig is, zal de authentificatie in volledige browser opnieuw richten pagina gebeuren.
&#x200B;- `requestorIds` (optioneel) - De waarden requestIds worden opgegeven door Adobe. Een vereiste is dat een proxy MVPD met minstens één aanvragerId moet worden geïntegreerd. Als de tag &quot;requestIds&quot; niet aanwezig is op het proxy-element MVPD, wordt die proxy MVPD geïntegreerd met alle beschikbare aanvragers die zijn geïntegreerd onder de proxy MVPD.
&#x200B;- `ProviderID` (facultatief) - wanneer het attribuut ProviderID op het id element aanwezig is, zal de waarde van ProviderID op het SAML authentificatieverzoek naar Volmacht MVPD als Proxied MVPD/SubMVPD ID (in plaats van de id waarde) worden verzonden. In dit geval, zal de waarde van identiteitskaart slechts in de plukker worden gebruikt MVPD die op de pagina van de Programmer wordt voorgesteld, en intern door de Authentificatie van Adobe Pass. De lengte van het attribuut ProviderID moet tussen 1 en 128 karakters zijn.

## Beveiliging {#security}

Een verzoek kan alleen als geldig worden beschouwd als het aan de volgende regels voldoet:

&#x200B;- De verzoekkopbal moet het veiligheids Oauth2 toegangstoken bevatten die zoals in [ wordt beschreven wordt verkregen terugwinnen toegangstoken ](../integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) API documentatie.
&#x200B;- Het verzoek moet van een specifiek IP adres komen dat is toegestaan.
&#x200B;- De aanvraag moet via het SSL-protocol worden verzonden.

Alle parameters in de aanvraagkoptekst die hierboven niet worden vermeld, worden genegeerd.

Voorbeeld van krullen:

`curl -X GET -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/<proxy-mvpd-identifier>/mvpds"`

## De Eindpunten van de Dienst van het Web van de Volmacht MVPD voor de Milieu&#39;s van de Authentificatie van Adobe Pass {#proxy-mvpd-wevserv-endpoints}

&#x200B;- **Productie URL:** https://mgmt.auth.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds
&#x200B;- **het Opvoeren URL:** https://mgmt.auth-staging.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds
&#x200B;- **pre-Qual-Production URL:** https://mgmt-prequal.auth.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds
&#x200B;- **Pre-Qual-Staging URL:** https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds

<!--
>[!RELATEDINFORMATION]
>* [Proxy MVPD SAML integration](/help/authentication/proxy-mvpd-saml-int.md)
>* [User metadata exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Technical paper](/help/authentication/technical-paper.md)
>* [Adobe Pass Authentication glossary](/help/authentication/glossary.md)
-->
