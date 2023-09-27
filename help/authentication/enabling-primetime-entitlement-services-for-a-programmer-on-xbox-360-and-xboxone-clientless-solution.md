---
title: Adobe Pass Entitlement Services inschakelen voor een programmeur op Xbox 360 en XboxOne-client
description: Adobe Pass Entitlement Services inschakelen voor een programmeur op Xbox 360 en XboxOne-client
exl-id: ff7254de-9ea4-4c27-a186-d1c2eea12222
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '444'
ht-degree: 0%

---

# Adobe Pass Entitlement Services inschakelen voor een programmeur op Xbox 360 en XboxOne-client {#enabling-primetime-entitlement-services-for-a-programer-on-xbox-360-and-xboxone-clientless}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.




1. De programmeur maakt een Zendesk-ticket waarmee de Xbox 360/One for Adobe Pass Authentication Client less-oplossing kan worden ingeschakeld door de volgende informatie op te geven:

   1. Platform: bv. Xbox 360, Xbox One

   1. Id aanvrager: bv. netgeo, CNN enz.

1. De Adobe zal X509 Certificaten tot stand brengen en zal de privé sleutel en het wachtwoord aan zijn eind vormen.

1. De Adobe zal het Openbare certificaat (van X509 cert) aan Programmeur in het kaartje of via e-mail verstrekken.

1. De programmeur moet dat openbare certificaat dan installeren op de GDNP-portal voor de toepassing die bij Microsoft is geregistreerd.

1. De programmeur zal dan JWT (Java Web Token) of STS Token voor XboxOne of 360 van de dienst van de Xbox Live van Microsoft verzoeken, die zou worden gecodeerd gebruikend het openbare certificaat X509 dat in stap 3 wordt verstrekt.

1. Dit zijn de tokens die unieke deviceId voor Xbox-apparaten bevatten. Neem het token (JWT of STS) op in de machtigingheader met behulp van de parameter &#39;x&#39;, zoals hieronder:

   1. Voor Xbox 360 moet het XSTS-token Base64-gecodeerd zijn voordat het naar Adobe Pass pay-TV-verificatie wordt verzonden.
   1. Voor Xbox One is de JWT al correct gecodeerd, zodat er geen extra codering moet plaatsvinden.

1. Alle API-aanroepen van het Xbox-apparaat moeten de machtigingsheader met het bovenstaande token in de parameter x bevatten.



>[!NOTE]
>
>Met name de Xbox heeft een aantal unieke vereisten met betrekking tot digitale ondertekening. De apparaat-id van de XBox-console is opgenomen in het XSTS-token.  Voor Xbox 360 is dit een gecodeerde SAML-bewering; voor Xbox One is dit een gecodeerde JWT. De XBox-console-app verzendt het volledige XSTS-token naar Adobe Pass pay-TV-verificatie. Adobe Pass pay-TV-verificatie decodeert het token met behulp van de openbare sleutel, parseert het token en extraheert de deviceId uit het token.

>[!NOTE]
>
>Wegens de grote lengte van het teken van XSTS, heeft de console XBox een technische beperking: het kan niet het teken als parameter van HTTP naar Adobe Pass betaal-TV authentificatie APIs verzenden. Met Adobe Pass pay-TV-verificatie kan de XSTS-token worden verzonden als onderdeel van de HTTP-header &quot;Authorization&quot; wanneer de API&#39;s worden aangeroepen. De XSTS-token moet worden versleuteld met de openbare sleutel uit het X.509-certificaat dat aan de programmeur is uitgegeven via betaaltelevisie-verificatie van Adobe Pass. Bij Adobe Pass pay-TV-verificatie wordt de bijbehorende persoonlijke sleutel opgeslagen en gebruikt om het XSTS-token te decoderen en de deviceId ervan te extraheren.
