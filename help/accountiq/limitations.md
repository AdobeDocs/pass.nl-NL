---
title: Beperkingen
description: Wis over beperkingen en isolatiemodus MVPD voor programmeurs in Account IQ.
exl-id: 08d65716-8b6a-4300-acda-fec63e1e6815
source-git-commit: 791d661e1495bdb6fe4eb25efbefeecd813f0f3a
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# Beperkingen {#limitations}

De D2C- en TV-versies overal van de account-IQ bieden analyses voor het delen van gebruik en abonnementen voor streamingproviders. De huidige versie 1.3 kent echter bepaalde beperkingen, die in toekomstige versies zullen worden aangepakt.

* De [Algehele score voor delen](/help/accountiq/data-panels.md#overall-sharing-score) momenteel include [Niveau voor delen](/help/accountiq/data-panels.md#sharing-level) en [Gebruik van gedeelde accounts](/help/accountiq/data-panels.md#usage-from-shared-accounts). Toekomstige versies zullen meer metriek omvatten.

* Wanneer u segmenten op het dashboard definieert of gebruikspatronen, kunt u [Videocategorieën in segment](/help/accountiq/data-panels.md#video-categories-segment), [Muziek delen via kanalen en MVPD&#39;s](/help/accountiq/data-panels.md#sharin-score-by-channels-and-mvpds), en [Gebruik van patroondistributie voor videocategorieën](/help/accountiq/usage-patterns.md#usage-pattern-dis-video-categories) rapporten kunnen alleen gegevens weergeven voor maximaal 20 [videocategorieën](product-concepts.md#video-category-def). Segmenten met meer dan 20 videocategorieën bevatten geen gegevens in deze rapporten.

* Momenteel is de mogelijkheid om accountstatistieken te exporteren beperkt tot het exporteren van 1000 accounts.

* Bij het definiëren van bewerkingen kunt u de optie selecteren [segmenttype](/help/accountiq/operations.md#segment) is beperkt tot **Vast aantal accounts**. De **Variabel aantal rekeningen** deze optie zal in een toekomstige versie beschikbaar zijn .

* De **Benchmarking**, **Detectiemodellen**, **Handelingen**, en **Instellingen** secties in de linkernavigatie zijn momenteel uitgeschakeld en zijn in een toekomstige release beschikbaar.

* Bij het maken van bewerkingen kunt u slechts twee soorten bewerkingen toepassen [handelingen](/help/accountiq/operations.md#action) — Gelijktijdige toezichtregels en externe acties.

* Momenteel kunt u alleen [maken](/help/accountiq/operations.md#create-new-operation) en [schema](/help/accountiq/operations.md#schedule) Bewerkingen. De toekomstige versies zullen u toestaan om hen te pauzeren, te hervatten en volledig te leiden.

* U kunt gegevens slechts één week of maand tegelijk analyseren wanneer u Granularity en het Interval van de Tijd selecteert.

* U kunt geen Modus MVPDs van de Isolatie aan de segmentdefinitie met andere MVPDs toevoegen. Sommige MVPDs identificeert uniek geen abonnees over veelvoudige programmeerkanalen. Daarom worden deze MVPD&#39;s afzonderlijk behandeld voor tv-programmeurs overal.



