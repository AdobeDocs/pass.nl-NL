---
title: Voordelen van het gebruik van de parameter Clientless deviceType in Adobe Pass Authentication-meetgegevens
description: Voordelen van het gebruik van de parameter Clientless deviceType in Adobe Pass Authentication-meetgegevens
exl-id: a5004887-d5fa-468e-971b-10806519175b
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 0%

---

# Voordelen van het gebruik van de parameter Clientless deviceType in Adobe Pass Authentication-meetgegevens {#benefits-of-using-the-clientless-devicetype-parameter-in-primetime-authentication-metrics}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>

## Context

Hoewel optioneel, wordt de parameter `deviceType` van de Clientless-API, indien aanwezig, wordt gebruikt in Adobe Pass-verificatiegegevens die beschikbaar worden gemaakt via [Entitlement Service Monitoring](/help/authentication/entitlement-service-monitoring-overview.md).

Overwegende dat de verbinding tussen `deviceType` en de **uitkeringen** op het gebied van Adobe Pass-verificatiemetriek werd in eerste instantie niet vermeld dat het toepassingsgebied van deze technische notitie bestaat uit het toevoegen van meer informatie over deze gegevens.

## Toelichting

De `deviceType` Deze parameter was aanwezig in de Clientless API sinds de eerste versie, maar de implicaties ervan op de metriek van de Authentificatie van Adobe Pass werden toegevoegd in een recentere versie.



>[!IMPORTANT]
>
>Als de parameter `deviceType` correct is ingesteld, heeft het volgende **voordeel** in Entitlement Service Monitoring: het biedt metriek aan die [uitgesplitst per apparaattype](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) bij gebruik van Clientless, zodat verschillende soorten analyses kunnen worden uitgevoerd voor bijvoorbeeld Roku, AppleTV, Xbox enz.


Raadpleeg voor meer informatie over de API voor de bewaking van de machtigingsservice de [boor-down boom,](/help/authentication/entitlement-service-monitoring-api.md#drill-down_tree) die de [afmetingen](/help/authentication/entitlement-service-monitoring-overview.md#esm_dimensions) (middelen) beschikbaar in ESM 2.0.

>[!NOTE]
>
>Inhoud van deze technische notitie is ook toegevoegd aan de [Clientloze API](#clientless_device_type).




## Implementatie

Om volledig van de metriek van de Authentificatie van Adobe Pass te profiteren, zijn er twee soorten [Clientless API&#39;s](#web_srvs_summary) die momenteel worden gebruikt en die de juiste `deviceType` set:

1. API&#39;s met `regcode` als een vereiste parameter en zal de `deviceType` parameter die is ingesteld bij het maken van de `regcode`, met de volgende API-aanroep:
   - [\&lt;reggie _fqdn=&quot;&quot;>/reggie/v1/{requestorId}/regcode](#reg_serv)

1. API&#39;s met de `deviceType` als optionele parameter:
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/checkauthing](#check_authn_token)
   - [&lt;span class=&quot;s1&quot;>](#retrieve_authn_token)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/authorize](#init_authz)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/tokens/authz](#retrieve_authz_token)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/tokens/media](#short_media)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/mediatoken](#short_media)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/preAuthze](#PreAuthZ_Resources)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/logout](#init_logout)

Onze aanbeveling is om de `deviceType` en geeft het juiste apparaattype zonder Clientless voor alle API&#39;s door.
