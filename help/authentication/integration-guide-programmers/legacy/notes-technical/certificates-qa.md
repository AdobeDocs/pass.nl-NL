---
title: Certificaten Vragen en antwoorden
description: Certificaten Vragen en antwoorden
exl-id: d4e493b0-4467-42b1-9758-16c5941d8051
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# (Verouderd) Certificaten Vragen en antwoorden {#certificates-q}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [ pagina van de Mededelingen van het Product ](/help/authentication/product-announcements.md) wordt samengevoegd.

</br>

**Q1:** Is het mogelijk om certificaten over iOS en Android te registreren?

**A:** het certificaat voor iOS en Android is het zelfde in de huidige configuratie. Het native certificaat wordt voor beide platforms gebruikt.

</br>

**Q2:** Kunnen de zelfde certificaten van iOS als Primair &amp; Reserve in de milieu&#39;s van de Productie en van het Staging worden gebruikt? Als dit niet wordt aanbevolen, kunt u een verklaring geven?

**A:** Er is geen punt in het vormen van het zelfde certificaat zoals zowel Primair als Reserve certificaat. We hebben het concept van primaire certificaten en back-upcertificaten zodat we meerdere certificaten voor een programmeur kunnen configureren voor het geval het primaire certificaat verloopt of wordt ingetrokken. Een back-upcertificaat geeft programmeurs de tijd om de primaire certificaat te wijzigen zonder dat dit gevolgen heeft voor de releaseomgeving. Maar u kunt dezelfde set primaire en back-upcertificaten gebruiken voor zowel de productieprofielen als de testprofielen.

</br>

**Q3:** is een nieuw certificaat nodig voor Web-pagina&#39;s die nieuwe flexibele TempPass zullen gebruiken?

**A:** het certificaat (en om het even welk certificaat inderdaad) wordt gevormd op het niveau van het Bedrijf &amp; van de Programmer van Media. FlexibleTempPass is een MVPD, u te hoeven om geen certificaat voor het te vormen, zodat als u een bestaande Programmer met flexibele TempPass integreert, zal het certificaat dat reeds op het niveau van Programmer/Media Company wordt gevormd worden gebruikt.
