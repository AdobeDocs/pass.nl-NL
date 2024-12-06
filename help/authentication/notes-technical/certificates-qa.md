---
title: Certificaten Vragen en antwoorden
description: Certificaten Vragen en antwoorden
exl-id: d4e493b0-4467-42b1-9758-16c5941d8051
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 0%

---

# Certificaten Vragen en antwoorden {#certificates-q}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>

**Q1:** Is het mogelijk om certificaten over iOS en Android te registreren?

**A:** het certificaat voor iOS en Android is het zelfde in de huidige configuratie. Het native certificaat wordt voor beide platforms gebruikt.

</br>

**Q2:** Kunnen de zelfde certificaten van iOS als Primair &amp; Reserve in de milieu&#39;s van de Productie en van het Staging worden gebruikt? Als dit niet wordt aanbevolen, kunt u een verklaring geven?

**A:** Er is geen punt in het vormen van het zelfde certificaat zoals zowel Primair als Reserve certificaat. We hebben het concept van primaire certificaten en back-upcertificaten zodat we meerdere certificaten voor een programmeur kunnen configureren voor het geval het primaire certificaat verloopt of wordt ingetrokken. Een back-upcertificaat geeft programmeurs de tijd om de primaire certificaat te wijzigen zonder dat dit gevolgen heeft voor de releaseomgeving. Maar u kunt dezelfde set primaire en back-upcertificaten gebruiken voor zowel de productieprofielen als de testprofielen.

</br>

**Q3:** is een nieuw certificaat nodig voor Web-pagina&#39;s die nieuwe flexibele TempPass zullen gebruiken?

**A:** het certificaat (en om het even welk certificaat inderdaad) wordt gevormd op het niveau van het Bedrijf &amp; van de Programmer van Media. FlexibleTempPass is een MVPD, te hoeven u om het even welk certificaat voor het niet te vormen, zodat als u een bestaande Programmer met flexibele TempPass integreert, zal het certificaat dat reeds op het niveau van Programmer/Media Company wordt gevormd worden gebruikt.
