---
title: Isolatiemodus MVPD's
description: Meer informatie over de isolatiemodus MVPD's voor tv-programmeurs overal
source-git-commit: 5639319ce8915f0c33d927ca9554c405b3b2e87d
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 0%

---


# Isolatiemodus MVPD&#39;s voor tv-programmeurs overal {#isolation-mode-tve}

>[!IMPORTANT]
>
> Beperking van de Isolatiemodus voor MVPD&#39;s geldt alleen voor programmeurs op tv overal.

In de Wijze van de Isolatie, identificeren MVPDs (zoals, Xfinity) constant abonnees over apparaten die op hun interactie met specifieke programmeurs worden gebaseerd. In de standaardwijze, identificeren de MVPDs constant abonnees over apparaten ongeacht de betrokken programmeurs.

Hier volgt een voorbeeld:

![](assets/isolation-diff-new.png)

*De Wijze van de isolatie MVPDs identificeert vier verschillende abonnees in plaats van twee*

* Als abonnee B van een MVPD van de Wijze van de Isolatie (zoals, Xfinity) tot de inhoud toegang heeft die door twee verschillende programmeurs wordt aangeboden gebruikend het zelfde apparaat, dan zal MVPD verschillende herkenningstekens met de twee verschillende toegangspogingen associëren. Het lijkt dat er twee verschillende abonnees zijn die tot de inhoud voor de programmeurs (L en M in het cijfer) toegang hebben.

* Voor Standaard MVPDs, als abonnee B tot inhoud toegang heeft die door twee verschillende programmeurs wordt aangeboden, dan zal MVPD één enkel toegangsidentificator voor beide toegangspogingen associëren.

* MVPDs (zoals, Xfinity) op de Wijze van de Isolatie identificeert constant geen abonnee, zelfs als de abonnee het zelfde apparaat over verschillende programmeurs gebruikt.

Om gegevensvervorming te verhinderen door één enkele abonnee als veelvoudige abonnees te tellen toe te schrijven aan de toegang tot van verschillende programmeurs, beperkt de Wijze van de Isolatie gemelde activiteit over een programmeur tot slechts hun toepassingen.

Programmer L kan bijvoorbeeld gegevens alleen weergeven op basis van de activiteit van Identities W en Y, waarbij de identiteiten X en Z in de vorige afbeelding worden genegeerd.

>[!IMPORTANT]
>
> Het nadeel is dat Programmer L wordt beroofd van het delen van informatie die over Abonnees A en B wordt verzameld wegens activiteit met om het even welke Programmer buiten L.

In de isolatiemodus worden het delen van scores en de bijbehorende meetgegevens uitsluitend berekend op basis van de activiteit van apparaten die vanuit de toepassingen van de geselecteerde programmeur en het geselecteerde kanaal worden gestreamd. De delende scores en de waarschijnlijkheden worden berekend vanaf de stroom begint op de momenteel geselecteerde kanalen.

Het systeem werkt automatisch in de Isolatiemodus wanneer het geselecteerde segment een Isolatiemodus MVPD bevat die afzonderlijke abonnees bij het streamen vanaf verschillende programmeurs identificeert als meerdere abonnees. Alle grafieken en grafieken voor deze segmenten geven de resultaten van dit gewijzigde gedrag weer.

>[!IMPORTANT]
>
> Het gedrag in de isolatiemodus is niet compatibel met de standaardmodus, Isolatiemodus MVPD kan niet worden gemengd met andere MVPD&#39;s en omgekeerd.

Als u een segment wilt maken dat wordt geanalyseerd in de isolatiemodus, sleept u de Isolatiemodus MVPD, zoals **Xfinity**, aan de MVPDs sectie van de segmentdefinitie.

>[!NOTE]
>
> Aangezien MVPD&#39;s in de isolatiemodus niet met andere MVPD&#39;s kunnen worden gemengd, staat het MVPD-gedeelte van de segmentdefinitie niet toe dat daar een andere MVPD wordt gesleept.

![](assets/xfinity-in-segment.png)

*Xfinity-selectie in de isolatiemodus*

>[!IMPORTANT]
>
> Het delen van accounts is relevanter wanneer dit wordt gemeten voor streaming in alle toepassingen van de programmeur. Minder verwacht **Scores delen** en enige variatie in de meetwaarden in de isolatiemodus.

![](assets/aggregate-sharing-isolation.png)

*Het delen van kansmeters in de Wijze van de Isolatie*

Uit de bovenstaande cijfers blijkt dat slechts 9% van alle accounts wordt gedeeld en dat slechts 11% van de inhoud wordt verbruikt. Vanwege de natuurlijk lagere scores moeten de resultaten in de isolatiemodus anders worden geïnterpreteerd dan de resultaten in de standaardmodus.
