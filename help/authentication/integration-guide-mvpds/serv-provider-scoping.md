---
title: Scoping serviceprovider
description: Scoping serviceprovider
exl-id: 730c43e1-46c0-4eec-b562-b1ad93cce6d3
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# Scoping serviceprovider {#service-provoider-scoping}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Overzicht {#overview}

De standaardimplementatie van een integratie van de Authentificatie van Adobe Pass met MVPD is gebaseerd op de **Specificatie OLCA**. In het gedeelte Verificatievereisten van de specificatie OLCA (6.5, Subject Identifier) wordt aangegeven dat het mogelijk is het bereik van de Serviceverlener (SP) voor de Subject-id aan te geven. (Het onderwerpherkenningsteken is verduisterde Gebruiker - identiteitskaart MVPD keert aan SP terug.)  In een integratie van de Authentificatie van Adobe Pass, wordt het vereist dat MVPDs scoping van de verzoeken van de Authentificatie van SP toelaat.

Met de Authentificatie die van Adobe Pass de rol van SP voor Programmer neemt, is het noodzakelijk om een aanpassing uit te voeren die SP scoping van het verzoek van de Authentificatie toelaat.  Dit moet worden gedaan zodat MVPD het netwerkmerk kan identificeren dat in de bevestiging SAML aan de Identiteitsprovider van MVPD (IdP) wordt overgegaan.  Scoping kan op een van de twee manieren worden uitgevoerd die in de volgende sectie worden beschreven.

## Scoping serviceprovider {#service-provider-scoping}

De Authentificatie van Adobe Pass steunt de volgende twee manieren om SP scoping van de Verzoeken van de Authentificatie toe te laten:

* **de methode van de Uitgever van SAML.** In deze aanpak wordt de &quot;Request or ID&quot; toegevoegd aan de SAML Issuer-tekenreeks in het SAML-verificatieverzoek.

* **de Aangepaste het Scoping benadering van het Bezit.** In deze aanpak wordt de &#39;Verzoeker-id&#39; expliciet opgenomen als een aangepaste eigenschap &#39;Scoping&#39; in de SAML-verificatieaanvraag.

>[!NOTE]
>
>De &quot;aanvrager-id&quot; is hoe Adobe Pass-verificatie verwijst naar het netwerkmerk van de programmeur (bijvoorbeeld: &quot;CNN&quot; is een van de merken van het Turner-netwerk).

### SAML-uitgiftebenadering {#saml-issuer-approach}

Deze benadering gebruikt het element SAML `<Issuer>` in het verzoek van de Authentificatie van SAML, zoals aangetoond in dit fragment:

```xml
...
<saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
    http://saml.sp.adobe.adobe.com/on-behalf-of/requestorID
</saml:Issuer>
...
```

### Aangepaste benadering van bereikeigenschappen {#custom-scoping-property-approach}

Deze benadering gebruikt een douanebezit genoemd &quot;Scoping&quot;, zoals aangetoond in dit fragment van een SAML authentificatieverzoek:

```xml
...
<samlp:Scoping xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <samlp:RequesterID xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">requestorID</samlp:RequesterID>
</samlp:Scoping>
...
```

<!--
>[!RELATEDINFORMATION]
>* [MVPD Authentication](/help/authentication/authn-usecase.md)
>* **OLCA Specification**
-->
