---
title: Upgradepad voor iOS/tvOS 3.7.0 inschakelen
description: Upgradepad voor iOS/tvOS 3.7.0 inschakelen
exl-id: f15c7414-ec9b-4e21-b457-1ecf59f47441
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---

# Upgradepad voor iOS/tvOS 3.7.0 inschakelen {#accessenabler-iostvos-370-upgrade-path}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>

Wijzigingen in sleutelketenopslag vanaf [new AccessEnabler versie 3.7.0](/help/authentication/authn-rn-ios-tvos-370.md) zijn onverenigbaar met de implementatie van de Keychain opslag van versie AccessEnabler lager dan 3.7.0.

Het verbeteringspad voor één toepassing die nieuwe AccessEnabler versie 3.7.0 goedkeurt zal alle tekenen van vorige versie/s van de opslag van de Keychain migreren. Daarom eindgebruikers **geen verlies van verificatie-/autorisatiesessies ervaren** tijdens het updateproces van het AccessEnabler-framework.

## Bekende beperkingen

Sommige beperkingen, die hieronder worden beschreven, kunnen door implementatoren worden aangetroffen.


1. Reguliere (Adobe) SSO werkt niet tussen één toepassing die AccessEnabler versie 3.7.0 gebruikt en één toepassing die versie(s) gebruikt van AccessEnabler lager dan 3.7.0, zelfs niet voor toepassingen die door dezelfde leverancier zijn ontwikkeld.

   >[!IMPORTANT]
   >
   >* SSO op systeemniveau (Apple) wordt niet beïnvloed!
   >
   >* De regelmatige (Adobe) SSO zal blijven werken als beide toepassingen door de zelfde verkoper worden ontwikkeld en AccessEnabler versie/s gebruiken lager dan 3.7.0!
   >
   >* De regelmatige (Adobe) SSO zal werken als beide toepassingen door de zelfde verkoper worden ontwikkeld en AccessEnabler versie 3.7.0 gebruiken!


1. In de situatie van het degraderen van één toepassing gebruikend AccessEnabler versie 3.7.0 aan een lagere versie van AccessEnabler, dan zullen de nieuwe geproduceerde tokens niet worden gemigreerd. Daarom zouden de eindgebruikers het verlies van authentificatie/vergunningszittingen kunnen ervaren, zonder het te verwachten.

   >[!IMPORTANT]
   >
   >* Eindgebruikers die zijn geverifieerd via SSO op systeemniveau (Apple) worden niet beïnvloed!
   >* Eindgebruikers die al waren geverifieerd voordat ze met AccessEnabler versie 3.7.0 naar de nieuwe toepassing werden bijgewerkt, worden hierdoor niet beïnvloed!

1. In de situatie van het degraderen van één toepassing gebruikend AccessEnabler versie 3.7.0 aan een lagere versie van AccessEnabler, dan zullen de geschrapte tekenen niet worden erkend. Daarom zouden de eindgebruikers de aanwezigheid van authentificatie/vergunningszittingen kunnen ervaren, zonder het te verwachten.
