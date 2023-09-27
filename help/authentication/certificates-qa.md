---
title: Certificaten Vragen en antwoorden
description: Certificaten Vragen en antwoorden
exl-id: d4e493b0-4467-42b1-9758-16c5941d8051
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Certificaten Vragen en antwoorden {#certificates-q}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>

**V1:** Is het mogelijk certificaten te registreren in iOS en Android?

**A:** Het certificaat voor iOS en Android is hetzelfde in de huidige configuratie. Het native certificaat wordt voor beide platforms gebruikt.

</br>

**V2:** Kunnen dezelfde iOS-certificaten worden gebruikt als Primair &amp; Back-upcertificaten in de productie- en faseringsomgeving? Als dit niet wordt aanbevolen, kunt u een verklaring geven?

**A:** Het heeft geen zin hetzelfde certificaat te configureren als het primaire en back-upcertificaat. We hebben het concept van primaire certificaten en back-upcertificaten zodat we meerdere certificaten voor een programmeur kunnen configureren voor het geval het primaire certificaat verloopt of wordt ingetrokken. Een back-upcertificaat geeft programmeurs de tijd om de primaire certificaat te wijzigen zonder dat dit gevolgen heeft voor de releaseomgeving. Maar u kunt dezelfde set primaire en back-upcertificaten gebruiken voor zowel de productieprofielen als de testprofielen.

</br>

**3e kwartaal:** Is een nieuw certificaat nodig voor webpagina&#39;s die de nieuwe flexibele TempPass gebruiken?

**A:** Het certificaat (en alle certificaten) zijn geconfigureerd op het niveau van Media Company &amp; Programmer. FlexibleTempPass is een MVPD, te hoeven u om het even welk certificaat voor het niet te vormen, zodat als u een bestaande Programmer met flexibele TempPass integreert, zal het certificaat dat reeds op het niveau van Programmer/Media Company wordt gevormd worden gebruikt.
