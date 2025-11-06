---
title: Voordelen van het gebruik van de parameter Clientless deviceType in Adobe Pass Authentication-meetgegevens
description: Voordelen van het gebruik van de parameter Clientless deviceType in Adobe Pass Authentication-meetgegevens
exl-id: a5004887-d5fa-468e-971b-10806519175b
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '372'
ht-degree: 0%

---

# (Verouderd) Voordelen van het gebruik van de parameter Clientless deviceType in Adobe Pass Authentication-meetgegevens {#benefits-of-using-the-clientless-devicetype-parameter-in-primetime-authentication-metrics}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [&#x200B; pagina van de Mededelingen van het Product &#x200B;](/help/authentication/product-announcements.md) wordt samengevoegd.

</br>

## Context

Hoewel facultatief, wordt de parameter `deviceType` van Clientless API, wanneer huidig, gebruikt in de metriek van de Authentificatie van Adobe Pass die door [&#x200B; de Controle van de Dienst van de Entitlement &#x200B;](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md) wordt blootgesteld.

Aangezien de verbinding tussen de `deviceType` parameter en zijn **voordelen** op de metriek van de Authentificatie van Adobe Pass niet aanvankelijk werd verklaard, moet het werkingsgebied van deze technische nota meer informatie over hen toevoegen.

## Toelichting

De parameter `deviceType` was aanwezig in de Clientless API sinds de eerste versie, maar de implicaties ervan op de metriek van de Authentificatie van Adobe Pass werden toegevoegd in een recentere versie.



>[!IMPORTANT]
>
>Als de parameter `deviceType` correct wordt geplaatst dan heeft het het volgende **voordeel** in de Controle van de Dienst van het Entitlement: het biedt metriek aan die [&#x200B; uitgesplitst per apparatentype &#x200B;](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) wanneer het gebruiken van Clientless zijn, zodat de verschillende types van analyse voor bijvoorbeeld Roku, AppleTV, Xbox enz. kunnen worden uitgevoerd.


Voor meer informatie over de Controle API van de Dienst van de Entitlement, gelieve te verwijzen naar de [&#x200B; boor-benedenboom, &#x200B;](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md#drill-down_tree) die de [&#x200B; dimensies &#x200B;](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#esm_dimensions) (middelen) toont beschikbaar in ESM 2.0.

>[!NOTE]
>
>De inhoud van dit technische nota werd ook toegevoegd aan [&#x200B; Clientless API &#x200B;](#clientless_device_type).




## Implementatie

Om volledig van de metriek van de Authentificatie van Adobe Pass te profiteren, zijn er 2 types van [&#x200B; Clientless APIs &#x200B;](#web_srvs_summary) die momenteel worden gebruikt en die de correcte `deviceType` moeten hebben reeks:

1. API&#39;s die `regcode` als een vereiste parameter hebben en die de parameter `deviceType` gebruiken die is ingesteld bij het maken van de `regcode` , met de volgende API-aanroep:
   - [\&lt;REGGIE\_FQDN\>/reggie/v1/](#reg_serv)

1. API&#39;s met de `deviceType` als optionele parameter:
   - [\&lt;SP\_FQDN\>/api/v1/checkauthoring](#check_authn_token)
   - [&lt;span class=&quot;s1&quot;>](#retrieve_authn_token)
   - [\&lt;SP\_FQDN\>/api/v1/authorize](#init_authz)
   - [\&lt;SP\_FQDN\>/api/v1/tokens/authz](#retrieve_authz_token)
   - [\&lt;SP\_FQDN\>/api/v1/tokens/media](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/mediatoken](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/preauthorize](#PreAuthZ_Resources)
   - [\&lt;SP\_FQDN\>/api/v1/logout](#init_logout)

Onze aanbeveling is om de parameter `deviceType` te gebruiken en het juiste apparaattype zonder Clientless voor alle API&#39;s door te geven.
