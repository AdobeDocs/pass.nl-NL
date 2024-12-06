---
title: SSO via passieve verificatie
description: SSO via passieve verificatie
exl-id: ce45899f-6e94-4bb0-a2c1-51f03bd66d8d
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '778'
ht-degree: 0%

---

# SSO via passieve verificatie

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.


## Inleiding

Het werkingsgebied van dit document moet de implementatie van de passieve authentificatiestroom beschrijven en hoe dit met onze standaard enige sign-on benadering werkt.

## Gebruiksmogelijkheden

Met Adobe Pass Authentication schakelt u Single Sign-On (SSO) in tussen apps/sites. Nadat een gebruiker met hun geloofsbrieven MVPD het programma opent, produceert de Authentificatie van Adobe Pass een veilig teken dat de de authentificatiesessie van MVPD vertegenwoordigt, en bindt dat teken aan het apparaat van de gebruiker gebruikend een identiteitskaart van het Apparaat. Bij Adobe Pass-verificatie wordt het token/apparaat-id opgeslagen op een server of op het apparaat.

Zolang het token nog geldig is, worden de gebruikers direct weergegeven als zijnde geverifieerd. Dit staat gebruikers toe om hun geloofsbrieven minder vaak in te gaan terwijl het houden van transacties veilig.



Het hier beschreven geval voor zakelijk gebruik is een zeer specifieke vereiste: dat de gebruiker ten minste één keer voor elke bezochte site moet worden geverifieerd. Dit laat MVPD toe om bedrijfsregels toe te passen verbonden aan de authN zitting die per netwerk kan variëren. Het is in strijd met de huidige TVE-belofte dat een gebruiker zich slechts eenmaal moet aanmelden en dat de verificatie zal worden uitgevoerd op alle sites die deel uitmaken van het ecosysteem voor Adobe Pass-verificatie.



Om de bedrijfsregel te handhaven maar ook een goede gebruikerservaring te houden, vereist MVPD NIET een gebruiker om referentie informatie manueel te verstrekken. Wij kunnen van het eerder vastgestelde zittingskoekje profiteren en proberen om een automatische re-authentificatie te doen gebruikend de passieve stroom; van het gebruikersperspectief zal hij automatisch het programma worden geopend.



Om deze op te lossen, hebben wij 2 verschillende eigenschappen uitgevoerd: authentificatie per netwerk en passieve authentificatiesteun. MVPDs zal SAML passieve authN steunen waar zij eenvoudig een gebruiker opnieuw voor authentiek verklaren als een authN zitting op IdP bestaat, ongeacht op welke plaats die zitting werd gecreeerd.



## Verificatie per netwerk

Deze eigenschap zorgt ervoor dat MVPD een authentificatieverzoek eens voor elke bezochte plaats ontvangt. Deze functionaliteit betekent dat een de authentificatietoken van de Authentificatie van Adobe Pass aan requestID zal worden beperkt, zo geldig slechts voor het netwerk zal zijn die het opvroegen. Als de gebruiker op de site &quot;A&quot; verifieert en vervolgens de site &quot;B&quot; bezoekt, moet de gebruiker daarom worden geverifieerd.



Merk op dat wegens het feit dat op MVPD IdP de gebruiker reeds voor authentiek wordt verklaard, hij niet zal worden vereist om login informatie te verstrekken, maar in plaats daarvan zal browser eenvoudig van plaats &quot;B&quot;aan MVPD IdP en dan terug worden opnieuw gericht. Dezelfde gebruiker wordt nog steeds geverifieerd op site &quot;A&quot; als hij terugkeert.



De volgende stroom toont de basisauthentificatie per de eigenschap van het Netwerk:





## Passieve verificatie

Het doel voor dit is om het herauthentificatieproces op de achtergrond te maken zonder de behoefte van volledige browser omleiding en de plukker die wordt getoond. Als gevolg hiervan wordt een gebruiker die van site A naar site B gaat, automatisch geverifieerd.



Vanuit UX-perspectief is er geen verschil tussen deze flow en een flow die wordt uitgevoerd met een gewone MVPD. Wat de gebruiker ziet is dat het na het ingaan van geloofsbrieven als resultaat van het bezoeken van plaats A, automatisch zal op plaats B voor authentiek worden verklaard.



Om deze stroom levensvatbaar te maken zal MVPD passieve authentificatie moeten steunen zodat verborgen iframe niet &quot;geplakt&quot;op de login pagina zal zijn in het geval MVPD geen zitting meer heeft. Dit wordt gedaan via het standaard &quot;isPassive&quot;attribuut.



Het volgende diagram toont de verbeterde stroom en de &quot;achter-de-scènes&quot;passieve authentificatie:





SAML-aanvraagvoorbeeld
Hier is een SAML- verzoeksteekproef voor de passieve authN stroom:


```xml
<saml2p:AuthnRequest xmlns:saml2p="urn:oasis:names:tc:SAML:2.0:protocol"
                     AssertionConsumerServiceURL="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"
                     Destination="https://mvpd_idp_url"
                     ForceAuthn="false"
                     ID="_15056686-399c-4528-b21a-4a9542cfc8ec"
                     IsPassive="true"
                     IssueInstant="2014-11-03T14:18:12.394Z"
                     ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
                     Version="2.0"
                     >
    <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion">https://saml.sp.auth.adobe.com </saml2:Issuer>
    <saml2p:Extensions>
        <thrpty:RespondTo xmlns:thrpty="urn:oasis:names:tc:SAML:protocol:ext:third-party">https://saml.sp.auth.adobe.com</thrpty:RespondTo>
    </saml2p:Extensions>
    <saml2p:NameIDPolicy AllowCreate="true"
                         Format="urn:oasis:names:tc:SAML:2.0:nameid-format:transient"
                         SPNameQualifier="https://saml.sp.auth.adobe.com"
                         />
</saml2p:AuthnRequest>
```

## Bedrijfsvoorschriften

MVPDs heeft specifieke SSO scoping domeinbeperkingen. Bijvoorbeeld, slechts konden sommige domeinen door sommige MVPDs (SSO voor het zelfde media bedrijf maar niet over bedrijven worden toegestaan).
Sommige MVPDs zou verschillende authentificatieregels kunnen vereisen om worden afgedwongen. Bijvoorbeeld, zou MVPDs verschillende authentificatie TTLs per verschillende netwerken kunnen hebben. Ook, zou MVPDs op huis gebaseerde authentificatie voor sommige netwerken maar niet voor anderen kunnen toelaten (de ouderlijke zaken van het controlegebruik worden sterk vertegenwoordigd hier).


Deze bedrijfsvereisten zouden moeten in mening zijn dat het belangrijkste gebruik-geval is dat de gebruiker niet zou moeten worden vereist opnieuw binnen te ondertekenen na met succes het ondertekenen met hun MVPD.

Dit kan worden verwezenlijkt door authentificatie per netwerk met passieve authN vlag te gebruiken.



## Bekende beperkingen

iOS - vanwege de aard van de lokale opslag in iOS werken SSO-stromen niet op iOS voor toepassingen die door verschillende leveranciers zijn ontwikkeld. Raadpleeg deze technische notitie voor meer informatie over SSO in iOS 8 en hoger.


<!--
>[!RELATEDINFORMATION]
>* Single Sign-On on iOS
>* SSO on iOS when using the Adobe Pass Authentication Access Enabler
-->
