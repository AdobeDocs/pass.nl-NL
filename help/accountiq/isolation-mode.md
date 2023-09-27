---
title: Rapporten weergeven in de isolatiemodus
description: Rapporten weergeven in de isolatiemodus voor Xfinity.
exl-id: e7cf24c5-9bfa-48f6-b5c8-20443a976891
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 0%

---

# Rapporten delen weergeven in de isolatiemodus {#report-isolation-mode}

In de Wijze van de Isolatie, identificeren MVPDs (zoals, Xfinity) constant abonnees over apparaten, maar identificeren hun abonnees verschillend, gebaseerd op de programmeurs die zij met in wisselwerking staan. In de standaardmodus identificeren MVPD&#39;s voortdurend abonnees op verschillende apparaten, ongeacht de programmeurs.

Bijvoorbeeld, in het volgende beeld als abonnee B van een Wijze van de Isolatie MVPD (zoals, Xfinity) tot de inhoud toegang heeft die door twee verschillende programmeurs wordt aangeboden gebruikend het zelfde apparaat, dan zal MVPD verschillende herkenningstekens met de twee verschillende toegangspogingen associëren. Dus voor die programmeurs (L en M in het cijfer) en voor Account IQ, lijkt het dat er twee verschillende abonnees zijn die tot de inhoud toegang hebben. Nochtans, voor Standaard MVPD, als abonnee B tot inhoud toegang heeft die door twee verschillende programmeurs wordt aangeboden, dan zal MVPD één enkel toegangsidentificator voor beide toegangspogingen associëren. MVPDs (zoals, Xfinity) op de Wijze van de Isolatie identificeert niet constant een abonnee zelfs als de abonnee het zelfde apparaat over verschillende programmeurs gebruikt.

![](assets/isolation-diff-new.png)

*Afbeelding: MVPD voor isolatiemodus identificeert vier verschillende abonnees in plaats van twee*

Om de vervorming van gegevens (door het identificeren van de zelfde abonnee als verschillend te beheren gebaseerd op de toegang tot van verschillende programmeurs), beperkt de Wijze van de Isolatie de activiteit die over een programmeur aan de activiteit slechts op de toepassingen van die programmeur wordt gemeld. Voor de isolatiemodus in de bovenstaande afbeelding ziet Programmer L bijvoorbeeld alleen gegevens die zijn gebaseerd op de activiteit van Identities W en Y, waarbij de identiteiten X en Z worden genegeerd.

>[!IMPORTANT]
>
> Het nadeel is dat Programmer L wordt beroofd van het delen van informatie die over Abonnees A en B wordt verzameld wegens activiteit met om het even welke Programmer buiten L.

In de isolatiemodus worden alle berekeningen die zijn gemaakt voor het verkrijgen van de Sharing Scores en alle bijbehorende meetgegevens gemaakt met alleen de activiteit van de apparaten die worden gestreamd vanuit toepassingen die tot de geselecteerde programmeur en kanalen behoren.
De delende scores en de waarschijnlijkheden worden berekend slechts gebruikend de stroom die van de momenteel geselecteerde kanalen begint.

Metrische gegevens weergeven in de isolatiemodus:

1. Selecteren **[!UICONTROL isolation mode]** van de **[!UICONTROL MVPDs in segment]** en selecteert u **[!UICONTROL Apply Selection]**.

   ![](assets/xfinity-in-segment.gif)

   *Afbeelding: MVPD-selectie in de isolatiemodus*

1. Selecteer de gewenste kanalen in het menu **[!UICONTROL Channels in segment]** en selecteert u **[!UICONTROL Apply Selection]**.

   Selecteer ook een [tijdkader](/help/accountiq/product-concepts.md#granularity-def).

   >[!IMPORTANT]
   >
   >Omdat het delen van accounts relevanter is bij het streamen in alle toepassingen van de programmeur, ziet u lagere scores voor delen en enige variatie in de meetwaarden in de isolatiemodus.

   ![](assets/aggregate-sharing-isolation.png)

   *Afbeelding: kansmeters delen in de isolatiemodus*

   Uit bovenstaande cijfers blijkt dat slechts 6% van alle accounts wordt gedeeld en dat slechts 8% van de inhoud wordt verbruikt door die 8%. De kanalen kunnen hun scores in de isolatiemodus vergelijken met die in de andere MVPD&#39;s. Daarom moet de informatie die door het gebruiken van de Wijze van de Isolatie wordt verkregen verschillend van de andere gegevens worden geïnterpreteerd.
