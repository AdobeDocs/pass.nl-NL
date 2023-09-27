---
title: MVPD-verificatie
description: MVPD-verificatie
exl-id: 9ff4a46e-a37b-414c-a163-9e586252a9c3
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1908'
ht-degree: 0%

---

# MVPD-verificatie {#mvpd-authn}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#mvpd-authn-overview}

De daadwerkelijke dienstverlener (SP) rol wordt gehouden door een Programmer, maar de Authentificatie van Adobe Pass dient als volmacht van SP voor die Programmer. Door Adobe Pass Authentication als tussenpersoon te gebruiken, kunnen zowel MVPD&#39;s als Programmers vermijden dat ze hun machtigingsprocessen per geval moeten aanpassen.

De stappen hieronder stellen de opeenvolging van gebeurtenissen voor, gebruikend de Authentificatie van Adobe Pass, wanneer een Programmer authentificatie van MVPD verzoekt die SAML steunt. De component Adobe Pass Authentication Access Enabler is actief op de client van de gebruiker/abonnee. Van daar, vergemakkelijkt de Toegankelijkheid alle stappen van de authentificatiestroom.

1. Wanneer de gebruiker om toegang tot beschermde inhoud verzoekt, stelt de Toegangsmanager authentificatie (AuthN) namens de Programmer (SP) in werking.
1. De app van SP stelt een &quot;Picker MVPD&quot;aan de gebruiker voor om hun leverancier van betaaltelevisie (MVPD) te verkrijgen. SP richt dan browser van de gebruiker aan de geselecteerde dienst van de identiteitsleverancier van MVPD (IdP) opnieuw.  Dit is &quot;**Door programmeur geïnitieerde aanmelding**&quot;.  MVPD verzendt het antwoord van IdP naar de beweringsdienst van SAML van de Adobe, waar het wordt verwerkt.
1. Tot slot richt Toegangsbeheer browser terug naar de plaats van SP, die SP van de status (succes/mislukking) van het verzoek van AuthN op de hoogte brengt.

## De verificatieaanvraag {#authn-req}

Zoals voorgesteld in de stappen hierboven, tijdens de stroom AuthN moet een MVPD zowel een op SAML-Gebaseerd verzoek van AuthN goedkeuren als een reactie van SAML verzenden AuthN.

De [OLCA (Online Content Access)-verificatie en verificatie-interfacespecificatie](https://www.cablelabs.com/specifications/search?query=&amp;category=&amp;subcat=&amp;doctype=&amp;content=false&amp;archives=false){target=_blanck} presenteert een standaardverzoek en een antwoord van AuthN. Hoewel de Authentificatie van Adobe Pass MVPDs niet vereist om hun machtigingsoverseinen op deze norm te baseren, kan het bekijken van de specificatie inzicht in de belangrijkste attributen verstrekken die voor een transactie AuthN worden vereist.

>[!NOTE]
>
>Het verzoek AuthN een MVPD met de Authentificatie van Adobe Pass ontvangt bevat geen digitale handtekening. In het onderstaande voorbeeld ziet u echter geen handtekening, vanwege de beknoptheid. Zie het voorbeeld in [de verificatierespons](#authn-response) in de volgende secties.

Voorbeeld van SAML-verificatieverzoek:

```XML
<?xml version="1.0" encoding="UTF-8"?>
<samlp:AuthnRequest  
    AssertionConsumerServiceURL=http://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer          
    Destination=http://idp.com/SSOService
    ForceAuthn="false"
    ID="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
    IsPassive="false"
    IssueInstant="2010-08-03T14:14:54.372Z"
    ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
    Version="2.0"
    xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
        http://saml.sp.adobe.adobe.com
    </saml:Issuer>
    <ds:Signature xmlns:ds=_signature_block_goes_here_
    </ds:Signature>
    <samlp:NameIDPolicy
        AllowCreate="true"
        Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
        SPNameQualifier="http://saml.sp.adobe.adobe.com"/>
</samlp:AuthnRequest> 
```

In de onderstaande tabel worden de kenmerken en tags beschreven die in een verificatieaanvraag moeten worden opgenomen, met de verwachte standaardwaarden.

**Gegevens SAML-verificatieverzoek**

| voorbeeld:AuthnRequest | &lt;authnrequest> uitgegeven door de Serviceverlener aan de Identiteitsprovider. |
|-----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AssertionConsumerServiceURL | Dit is het eindpunt van de Adobe dat moet worden gebruikt in de volgende respons. Standaardwaarde: **http://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer** |
| Doel | Een URI-verwijzing die het adres aangeeft waarnaar deze aanvraag is verzonden. Dit is nuttig om kwaadwillig door:sturen van verzoeken aan onbedoelde ontvangers, een bescherming te verhinderen die door sommige protocolbanden wordt vereist. Als het aanwezig is, MOET de daadwerkelijke ontvanger controleren dat de verwijzing van URI de plaats identificeert waarop het bericht werd ontvangen. Als dit niet het geval is, MOET het verzoek worden afgewezen. Sommige protocolbanden kunnen het gebruik van dit attribuut vereisen. |
| ForceAuthn | Het attribuut ForceAuthn, indien aanwezig met de waarde waar, verplicht de identiteitsleverancier om deze identiteit nieuw te vestigen eerder dan het steunen van een bestaande zitting het met het hoofd kan hebben. |
| ID | Een id voor de aanvraag. Zie [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} punt 1.3.4 voor nadere bijzonderheden. |
| IsPassive | Een Booleaanse waarde. Indien &quot;true&quot;, MOGEN de identiteitsprovider en de gebruikersagent zelf NIET zichtbaar de controle over de gebruikersinterface van de aanvrager overnemen en met de presentator communiceren op een merkbare manier. Als geen waarde wordt opgegeven, is de standaardwaarde &quot;false&quot;. |
| IssueInstant | Het tijdstip waarop het antwoord wordt gegeven. De tijdwaarde wordt gecodeerd in UTC zoals beschreven in [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} Punt 1.3.3. |
| ProtocolBinding | Een verwijzing van URI die een het protocolband identificeert van SAML die moet worden gebruikt wanneer het terugkeren van &lt;response> bericht. Zie [SAMLBind] voor meer informatie over protocol banden en de verwijzingen van URI die voor hen worden bepaald. Standaardwaarde: urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST |
| Versie | De versie van deze aanvraag. |
| voorbeeld:uitgever | Identificeert de entiteit die het antwoordbericht heeft gegenereerd. (Voor meer informatie over dit element, zie SAML kern 2.0-os Sectie 2.2.5) |
| ds:handtekening | Een handtekening van XML die de integriteit van beschermt en uitgever van de bewering voor authentiek verklaart, zoals die Sectie 5 in SAML kern 2.0-os wordt beschreven |
| voorbeeld:NameIDPopolicy | Geeft beperkingen op aan de naam-id die moet worden gebruikt om het gevraagde onderwerp weer te geven. |
| AllowCreate | Een Booleaanse waarde die wordt gebruikt om aan te geven of de identiteitsprovider tijdens het uitvoeren van het verzoek een nieuwe id mag maken die de principal vertegenwoordigt. Standaard: true |
| Indeling | Hiermee wordt de URI-referentie opgegeven die overeenkomt met de naam-id-indeling Default: urn:oasis:names:tc:SAML:2.0:nameid-format:transient Adobe adviseert: urn:oasis:names:tc:SAML:2.0:nameid-format:blijvend |
| SPNameQualifier | Optioneel geeft u op dat de id van het assertieonderwerp moet worden geretourneerd (of gemaakt) in de naamruimte van een andere serviceprovider dan de aanvrager. Standaard: http://saml.sp.adobe.adobe.com |

## De verificatiereactie {#authn-response}

Nadat het authentificatieverzoek is ontvangen en behandeld, moet MVPD een authentificatiereactie nu verzenden.

**Voorbeeld van SAML-verificatierespons**

```XML
<?xml version="1.0" encoding="UTF-8"?> 
<samlp:Response Destination="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"
                ID="_0ac3a9dd5dae0ce05de20912af6f4f83a00ce19587"                             
                InResponseTo="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
                IssueInstant="2010-08-17T11:17:50Z" Version="2.0"              
                xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion"
                xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol"
                xmlns:xs="http://www.w3.org/2001/XMLSchema"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
             http://idp.com/SSOService
    </saml:Issuer>
    <samlp:Status>
       <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
    </samlp:Status>
    <saml:Assertion ID="pfxb0662d76-17a2-a7bd-375f-c11046a86742"
                   IssueInstant="2010-08-17T11:17:50Z"
                   Version="2.0">
        <saml:Issuer>http://idp.com/SSOService</saml:Issuer>
        <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
          <ds:SignedInfo>
            <ds:CanonicalizationMethod
                     Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
            <ds:SignatureMethod
                     Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>
            <ds:Reference URI="#pfxb0662d76-17a2-a7bd-375f-c11046a86742">
              <ds:Transforms>
                 <ds:Transform
                    Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>        
                 <ds:Transform
                            Algorithm=http://www.w3.org/2001/10/xml-exc-c14n#"/>
              </ds:Transforms>
              <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
              <ds:DigestValue>LgaPI2ASx/fHsoq0rB15Zk+CRQ0=</ds:DigestValue>
            </ds:Reference>
          </ds:SignedInfo>
          <ds:SignatureValue>
                POw/mCKF__shortened_for_brevity__9xdktDu+iiQqmnTs/NIjV5dw==
          </ds:SignatureValue>
          <ds:KeyInfo>
            <ds:X509Data>
                <ds:X509Certificate>
                 MIIDVDCCAjygAwIBA__shortened_for_brevity_utQ==
                </ds:X509Certificate>
            </ds:X509Data>
          </ds:KeyInfo>
      </ds:Signature>
      <saml:Subject>
        <saml:NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
                     SPNameQualifier="https://saml.sp.auth.adobe.com">
            _5afe9a437203354aa8480ce772acb703e6bbb8a3ad
        </saml:NameID>
        <saml:SubjectConfirmation
                     Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
            <saml:SubjectConfirmationData
                  InResponseTo="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
                  NotOnOrAfter="2010-08-17T11:22:50Z"                                          
                  Recipient="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"/>
           </saml:SubjectConfirmation>
       </saml:Subject>
       <saml:Conditions NotBefore="2010-08-17T11:17:20Z"
                        NotOnOrAfter="2010-08-17T19:17:50Z">
           <saml:AudienceRestriction>
              <saml:Audience>https://saml.sp.auth.adobe.com</saml:Audience>
           </saml:AudienceRestriction>
       </saml:Conditions>
       <saml:AuthnStatement AuthnInstant="2010-08-17T11:17:50Z"
                   SessionIndex="_1adc7692e0fffbb1f9b944aeafce62aaa7d770cd9e">
        <saml:AuthnContext>
            <saml:AuthnContextClassRef>
                   urn:oasis:names:tc:SAML:2.0:ac:classes:Password
            </saml:AuthnContextClassRef>
        </saml:AuthnContext>
    </saml:AuthnStatement>
  </saml:Assertion>
</samlp:Response>
```


In de steekproef hierboven, verwacht SP van de Adobe om gebruikersidentiteitskaart uit Onderwerp/NameId te halen. SP van de Adobe kan worden gevormd om gebruiker te krijgen - identiteitskaart van een douane bepaald attribuut; de reactie zou een element zoals het volgende moeten bevatten:

```XML
<saml:AttributeStatement>
     <saml:Attribute Name="guid" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
         <saml:AttributeValue xsi:type="xs:string">
               71C69B91-F327-F185-F29E-2CE20DC560F5
         </saml:AttributeValue>
    </saml:Attribute>
</saml:AttributeStatement>
```

**SAML-verificatieresponsdetails**
| samlp:Response | Reactie ontvangen door Adobe Pass-verificatie.                                                                                                                                                                                                                                                                                                                                                                                                                       | |—|—| | Bestemming | Een URI-referentie die het adres aangeeft waarnaar dit verzoek is verzonden. Dit is nuttig om kwaadwillig door:sturen van verzoeken aan onbedoelde ontvangers, een bescherming te verhinderen die door sommige protocolbanden wordt vereist. Als het aanwezig is, MOET de daadwerkelijke ontvanger controleren dat de verwijzing van URI de plaats identificeert waarop het bericht werd ontvangen. Als dit niet het geval is, MOET het verzoek worden afgewezen. Sommige protocolbanden kunnen het gebruik van dit attribuut vereisen. | | ID | Een identificatiecode voor het verzoek. Het is van het type xs:ID en MOET voldoen aan de voorschriften van punt 1.3.4 van [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} voor unieke identificatiecode. De waarden van het attribuut ID in een verzoek en het attribuut InResponseTo in de overeenkomstige reactie MOETEN aanpassen.                                                                                                                                                                                         | | InResponseTo | De id van een SAML-protocolbericht waarop een certificerende entiteit de bewering kan indienen. De waarde moet gelijk zijn aan die in het ID-kenmerk dat in de verificatieaanvraag is verzonden. Zie SAML core 2.0-os | | IssueInstant | Tijdstip van afgifte van het verzoek.                                                                                                                                                                                                                                                                                                                                                                                                                                  | | Versie | De versie van het verzoek.                                                                                                                                                                                                                                                                                                                                                                                                                                                | | saml.Issuer | Identificeert de entiteit die het aanvraagbericht heeft gegenereerd. (Zie punt 2.2.5 voor meer informatie over dit element. SAML core 2.0-os) | | samlp:Status | Een code die de status van het overeenkomstige verzoek weergeeft.                                                                                                                                                                                                                                                                                                                                                                                                               | | sampleP:StatusCode | Een code die de status van de activiteit weergeeft die naar aanleiding van het desbetreffende verzoek is uitgevoerd.                                                                                                                                                                                                                                                                                                                                                                       | | saml:Bevestiging | Met dit type wordt de basisinformatie beschreven die alle beweringen gemeen hebben.                                                                                                                                                                                                                                                                                                                                                                                                        | | ID | De identificatiecode voor deze bewering.                                                                                                                                                                                                                                                                                                                                                                                                                                         | | Versie | De versie van deze bewering.                                                                                                                                                                                                                                                                                                                                                                                                                                             | | IssueInstant | Tijdstip van afgifte van het verzoek.                                                                                                                                                                                                                                                                                                                                                                                                                                  | | ds:Handtekening | Een XML-handtekening die de integriteit van de uitgever van de bewering beschermt en deze authenticeert, zoals beschreven in sectie 5 in SAML core 2.0-os | | ds:SignedInfo | De structuur van SignedInfo omvat het kanonicalisatiealgoritme, een handtekeningalgoritme, en één of meerdere verwijzingen. Het SignedInfo-element kan een optioneel ID-kenmerk bevatten waarmee door andere handtekeningen en objecten naar dit element kan worden verwezen. Zie Syntaxis en verwerking XML-handtekening | | ds:CanonicalizationMethod | CanonicalizationMethod is een vereist element dat het canonicalization algoritme specificeert dat op het element SignedInfo wordt toegepast alvorens handtekeningsberekeningen uit te voeren. Zie Syntaxis en verwerking XML-handtekening | | ds:SignatureMethod | SignatureMethod is a required element that specifies the algorithm used for signature generation and validation. Dit algoritme identificeert alle cryptografische functies betrokken bij de handtekeningsverrichting (b.v. het hakken, openbare zeer belangrijke algoritmen, MACs, het opvullen, enz.) Zie Syntaxis en verwerking XML-handtekening | | ds:Reference | Verwijzing is een element dat een of meer keren kan voorkomen. Hiermee worden een overzichtsalgoritme en een samenvattingswaarde opgegeven, en eventueel een id van het object dat wordt ondertekend, het type van het object en/of een lijst met transformaties die moeten worden toegepast voordat het object wordt getransformeerd. Zie Syntaxis en verwerking XML-handtekening | | ds:Transformaties | Het optionele element Transforms bevat een geordende lijst met Transform-elementen. Deze beschrijven hoe de ondertekenaar het gesplitste gegevensobject heeft verkregen. De uitvoer van elke Transformatie fungeert als invoer voor de volgende Transformatie. De invoer voor de eerste transformatie is het resultaat van het verwijderen van het URI-kenmerk van het Reference-element. De uitvoer van de laatste Transform is de invoer voor het DigestMethod-algoritme. Zie Syntaxis en verwerking XML-handtekening | | ds:DigestMethod | DigestMethod is een vereist element dat het digest-algoritme identificeert dat op het ondertekende object moet worden toegepast. Zie Syntaxis en verwerking XML-handtekening | | ds:DigestValue | DigestValue is een element dat de gecodeerde waarde van de digest bevat. De samenvatting wordt altijd gecodeerd met base64. Zie Syntaxis en verwerking XML-handtekening | | ds:SignatureValue | Het SignatureValue-element bevat de werkelijke waarde van de digitale handtekening; het wordt altijd gecodeerd met base64. Zie Syntaxis en verwerking XML-handtekening | | ds:KeyInfo | KeyInfo is een optioneel element waarmee de ontvanger(s) de sleutel kunnen verkrijgen die nodig is om de handtekening te valideren. Zie Syntaxis en verwerking XML-handtekening | | ds:X509Data | Een X509Data-element in KeyInfo bevat een of meer id&#39;s van sleutels of X509-certificaten. Zie Syntaxis en verwerking XML-handtekening | | ds: X509Certificate | Het X509Certificate-element, dat een base64-gecodeerde code bevat [X509v3] certificaat | | saml.:Onderwerp | Het onderwerp van de verklaring(en) in de bewering.                                                                                                                                                                                                                                                                                                                                                                                                                          | | saml:NameID | De &lt;nameid> element is van type NameIDType (zie Sectie 2.2.2 in SAML kern 2.0-os), en wordt gebruikt in diverse de assertieconstructies van SAML zoals &lt;subject> en &lt;subjectconfirmation> elementen, en in diverse protocolberichten.                                                                                                                                                                                                                                           | | Formaat | A URI reference representing the Classification of string-based identifier information.                                                                                                                                                                                                                                                                                                                                                                                    | | SPNameQualifier | Voorts komt een naam in aanmerking met de naam van een dienstverlener of aansluiting van dienstverrichters. Deze eigenschap verstrekt een extra middel om namen op basis van de het steunen partij of partijen te federeren.                                                                                                                                                                                                                                                                      | | saml:Onderwerpbevestiging | Informatie aan de hand waarvan het onderwerp kan worden bevestigd. Indien meer dan één bevestiging van het onderwerp wordt verstrekt, volstaat het aan een van de onderwerpen te voldoen om het onderwerp voor de toepassing van de bewering te bevestigen.                                                                                                                                                                                                                                                    | | saml:SubjectConfirmationData | Aanvullende bevestigingsinformatie die door een specifieke bevestigingsmethode moet worden gebruikt. De typische inhoud van dit element kan bijvoorbeeld een <!--<ds:KeyInfo>--> element zoals gedefinieerd in de specificatie XML Signature Syntax and Processing | | NotOnOrAfter | Een tijdstip waarop het onderwerp niet langer kan worden bevestigd.                                                                                                                                                                                                                                                                                                                                                                                                            | | Ontvanger | A URI specifying the entiteit or location to which an attesting entity can present the assertion. Bijvoorbeeld, zou deze eigenschap erop kunnen wijzen dat de bewering aan een bepaald netwerkeindpunt moet worden geleverd om een intermediair te verhinderen het elders opnieuw te richten.                                                                                                                                                                                   | | saml.voorwaarden | De &lt;condition> element fungeert als extensiepunt voor nieuwe voorwaarden.                                                                                                                                                                                                                                                                                                                                                                                                   | | NotBefore | Het kenmerk NotBefore geeft het tijdmoment aan waarop het geldigheidinterval begint.                                                                                                                                                                                                                                                                                                                                                                                  | | saml:AudienceRestriction | De &lt;audiencerestriction> element specificeert dat de bewering aan één of meerdere specifieke publiek wordt gericht die door &lt;audience> elementen.                                                                                                                                                                                                                                                                                                                           | | saml:Publiek | Een URI-referentie die een beoogd publiek identificeert.                                                                                                                                                                                                                                                                                                                                                                                                                      | | saml:AuthnStatement | Een verificatieverklaring.                                                                                                                                                                                                                                                                                                                                                                                                                                               | | AuthnInstant | Geeft het tijdstip aan waarop de verificatie heeft plaatsgevonden.                                                                                                                                                                                                                                                                                                                                                                                                                 | | SessionIndex | Geeft de index aan van een bepaalde zitting tussen de aangever die door het onderwerp wordt geïdentificeerd en de authenticerende autoriteit.                                                                                                                                                                                                                                                                                                                                              | | saml:AuthnContext | De context die door de authenticerende autoriteit wordt gebruikt tot en met de authenticatiegebeurtenis die deze verklaring heeft opgeleverd.                                                                                                                                                                                                                                                                                                                                                 | | voorbeeld:AuthnContextClassRef | Een verwijzing van URI die een klasse van de authentificatiecontext identificeert die de verklaring van de authentificatiecontext beschrijft die volgt. |
