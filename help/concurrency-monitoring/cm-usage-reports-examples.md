---
title: Voorbeelden van gebruiksrapporten voor gelijktijdige bewaking
description: Voorbeelden van gebruiksrapporten voor gelijktijdige bewaking
source-git-commit: 1ee6ba156364b183e7b5271e38af2c34687fca65
workflow-type: tm+mt
source-wordcount: '2400'
ht-degree: 0%

---

# Voorbeelden van gebruiksrapporten voor gelijktijdige bewaking{#cm-usage-reports-examples}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Voorbeelden van maandelijkse rapporten {#monthly-reports-examples}

| Naam | Dimensionen | URL | Metrisch |
|--------------------------------------------------------------------------------|----------------------------------------------------------------------------------|----------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Maandelijkse cross-MVPD-statistieken | &quot;year&quot;, &quot;month&quot;, &quot;mvpd&quot; | cmu/v2/jaar/maand/mvpd | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Maandelijkse staat van meerdere VPD-huurders | &quot;year&quot;, &quot;month&quot;, &quot;mvpd&quot;, &quot;owner | cmu/v2/jaar/maand/mvpd/huurder | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Maandelijkse staten van Cross-MVPD-huurder/toepassing | &quot;year&quot;, &quot;month&quot;, &quot;mvpd&quot;, &quot;huurder&quot;, &quot;application-id&quot; | cmu/v2/jaar/maand/mvpd/huurder/application-id | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Maandelijkse statussen van Cross-MVPD-gebruikers/toepassingen met toepassingsnaam en -platform | &quot;year&quot;, &quot;month&quot;, &quot;mvpd&quot;, &quot;huurder&quot;, &quot;application-id&quot;, &quot;application&quot;, &quot;platform&quot; | cmu/v2/year/month/mvpd/huurder/application-id/application/platform | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Maandelijks rapport per huurder | &quot;huurder&quot;, &quot;jaar&quot;, &quot;maand&quot; | cmu/v2/huurder/jaar/maand | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Maandelijkse statussen per huurder | &quot;huurder&quot;, &quot;year&quot;, &quot;month&quot;, &quot;application-id&quot; | cmu/v2/huurder/jaar/maand/application-id | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Maandelijkse statussen per huurder met toepassingsnaam | &quot;huurder&quot;, &quot;year&quot;, &quot;month&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/huurder/jaar/maand/application-id/application | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Maandelijkse statussen van het platformkanaal per huurder | &quot;huurder&quot;, &quot;year&quot;, &quot;month&quot;, &quot;mvpd&quot;, &quot;platform&quot;, &quot;application-id&quot; | cmu/v2/huurder/year/month/mvpd/platform/application-id | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Maandelijkse statussen per-huurder mvpd-platformkanaal met toepassingsnaam | &quot;huurder&quot;, &quot;year&quot;, &quot;month&quot;, &quot;mvpd&quot;, &quot;platform&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/huurder/year/month/mvpd/platform/application-id/application | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Maandelijkse statussen per kanaal/platform | &quot;huurder&quot;, &quot;year&quot;, &quot;month&quot;, &quot;channel&quot;, &quot;platform&quot;, &quot;application-id&quot; | cmu/v2/huurder/jaar/maand/kanaal/platform/application-id | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Maandelijkse statussen per kanaal/platform met toepassingsnaam | &quot;huurder&quot;, &quot;year&quot;, &quot;month&quot;, &quot;channel&quot;, &quot;platform&quot;, &quot;application-id&quot;,&quot;application&quot; | cmu/v2/huurder/jaar/maand/kanaal/platform/application-id/application | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Maandelijkse statistieken per mvpd | &quot;mvpd&quot;, &quot;year&quot;, &quot;month&quot; | cmu/v2/mvpd/jaar/maand | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Maandelijkse statussen per mvpd-huurder | &quot;mvpd&quot;, &quot;year&quot;, &quot;month&quot;, &quot;owner | cmu/v2/mvpd/jaar/maand/huurder | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Maandbericht op jaarbasis | &quot;year&quot;, &quot;month&quot;, &quot;gelijktijdige&quot; | cmu/v2/jaar/maand/gelijktijdig | &quot;gelijktijdig&quot;, &quot;gebruikers&quot; |
| Maandelijks rapport op valutaniveau per huurder | &quot;year&quot;, &quot;month&quot;, &quot;gelijktijdige&quot;, &quot;huurder&quot; | cmu/v2/jaar/maand/gelijktijdig-niveau/huurder | &quot;gelijktijdige&quot;, &quot;huurder&quot;, &quot;gebruikers&quot; |
| Maandelijks rapport op valutaniveau per huurder mvpd | &quot;year&quot;, &quot;month&quot;, &quot;gelijktijdige uitvoering&quot;, &quot;huurder&quot;, &quot;mvpd&quot; | cmu/v2/jaar/maand/gelijktijdig-niveau/huurder/mvpd | &quot;gelijktijdig-niveau&quot;, &quot;huurder&quot;, &quot;mvpd&quot;, &quot;gebruikers&quot; |
| Maandelijks activiteitenverslag | &quot;year&quot;, &quot;month&quot;, &quot;activity-level&quot; | cmu/v2/jaar/maand/activiteit | &quot;activity-level&quot;, &quot;users&quot; |
| Maandelijks activiteitenniveau per huurder | &quot;year&quot;, &quot;month&quot;, &quot;activity-level&quot;, &quot;huurder&quot; | cmu/v2/jaar/maand/activiteit-niveau/huurder | &quot;activity-level&quot;, &quot;huurder&quot;, &quot;gebruikers&quot; |
| Maandelijks activiteitenrapport per huurder mvpd | &quot;year&quot;, &quot;month&quot;, &quot;activity-level&quot;, &quot;huurder&quot;, &quot;mvpd&quot; | cmu/v2/jaar/maand/activiteit-niveau/huurder/mvpd | &quot;activity-level&quot;, &quot;huurder&quot;, &quot;mvpd&quot;, &quot;gebruikers&quot; |

*TODO:controleer met BG als de gelijktijdige en activiteitenniveaurapporten correct zijn *

## Voorbeelden van dagelijkse rapporten {#daily-reports-examples}

| Naam | Dimensionen | URL | Metrisch |
|------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|--------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Dagelijkse staten van mvpd/platform voor meerdere gebruikers | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;huurder&quot;, &quot;mvpd&quot;, &quot;platform&quot;, &quot;application-id&quot; | cmu/v2/jaar/maand/dag/huurder/mvpd/platform/application-id | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Dagelijkse statussen mvpd/platform voor meerdere paden met toepassingsnaam | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;huurder&quot;, &quot;mvpd&quot;, &quot;platform&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/jaar/maand/dag/huurder/mvpd/platform/application-id/application | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Dagelijkse statistieken van het platform voor meerdere gebruikers | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;huurder&quot;, &quot;platform&quot;, &quot;application-id&quot; | cmu/v2/jaar/maand/dag/huurder/platform/application-id | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Dagelijkse statussen van platformen voor meerdere gebruikers met toepassingsnaam | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;huurder&quot;, &quot;platform&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/year/month/day/huurder/platform/application-id/application | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Dagelijkse status van kanalen/platformen voor meerdere gebruikers | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;huurder&quot;, &quot;channel&quot;, &quot;platform&quot;, &quot;application-id&quot; | cmu/v2/jaar/maand/dag/huurder/kanaal/platform/application-id | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Dagelijkse statussen van kanalen/platformen voor meerdere paden met toepassingsnaam | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;huurder&quot;, &quot;channel&quot;, &quot;platform&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/year/month/day/huurder/channel/platform/application-id/application | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Dagelijkse statistieken van Cross-MVPD | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, mvpd&quot; | cmu/v2/jaar/maand/dag/mvpd | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Dagstaat van meerdere VPD-huurders | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;mvpd&quot;, &quot;owner | cmu/v2/jaar/maand/dag/mvpd/huurder | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Dagelijkse staat van de pachter/toepassing van de dwars-MVPD | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;mvpd&quot;, &quot;leasing&quot; | cmu/v2/jaar/maand/dag/mvpd/huurder/application-id | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Dagelijkse statistieken van meerdere VPD-gebruikers/toepassingen met toepassingsnaam en -platform | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, mvpd&quot;, &quot;huurder&quot;, &quot;application-id&quot;, &quot;application&quot;, &quot;platform&quot; | cmu/v2/year/month/day/mvpd/huurder/application-id/application/platform | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Dagverslag per huurder | &quot;huurder&quot;, &quot;jaar&quot;, &quot;maand&quot;, &quot;dag&quot; | cmu/v2/huurder/jaar/maand/dag | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Dagelijkse staten per huurderstoepassing | &quot;huurder&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;application-id&quot; | cmu/v2/huurder/jaar/maand/dag/applicatie-id | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Dagelijkse statussen per huurderstoepassing met toepassingsnaam | &quot;huurder&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/huurder/jaar/maand/dag/toepassings-id/toepassing | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Dagelijkse statistieken per huurder van mvpd | &quot;huurder&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;mvpd&quot;, &quot;platform&quot;, &quot;application-id&quot; | cmu/v2/huurder/jaar/maand/dag/mvpd/platform/application-id | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Dagelijkse statussen per huurder mvpd met toepassingsnaam | &quot;huurder&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;mvpd&quot;, &quot;platform&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/huurder/year/month/day/mvpd/platform/application-id/application | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Dagelijkse status per kanaal/platform | &quot;huurder&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;channel&quot;, &quot;platform&quot;, &quot;application-id&quot; | cmu/v2/huurder/jaar/maand/dag/kanaal/platform/application-id | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Dagelijkse statussen Per-huurderskanaal/platform met toepassingsnaam | &quot;huurder&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;channel&quot;, &quot;platform&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/huurder/year/month/day/channel/platform/application-id/application | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Dagelijkse statistieken per MVPD | &quot;mvpd&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot; | cmu/v2/mvpd/jaar/maand/dag | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Dagstatistieken per mvpd-huurder | &quot;mvpd&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hero&quot; | cmu/v2/mvpd/jaar/maand/dag/huurder | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Dagverslag op jaarniveau | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;gelijktijdige&quot; | cmu/v2/jaar/maand/dag/valutaniveau | &quot;gelijktijdig&quot;, &quot;gebruikers&quot; |
| Dagverslag op valutaniveau per huurder | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;gelijktijdige uitvoering&quot;, &quot;huurder&quot; | cmu/v2/jaar/maand/dag/valutaniveau/huurder | &quot;gelijktijdige&quot;, &quot;huurder&quot;, &quot;gebruikers&quot; |
| Dagverslag op valutaniveau per huurder mvpd | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;gelijktijdige uitvoering&quot;, &quot;huurder&quot;, &quot;mvpd&quot; | cmu/v2/jaar/maand/dag/valutaniveau/huurder/mvpd | &quot;gelijktijdig-niveau&quot;, &quot;huurder&quot;, &quot;mvpd&quot;, &quot;gebruikers&quot; |
| Dagelijks activiteitenverslag | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;activity-level&quot; | cmu/v2/jaar/maand/dag/activiteit-niveau | &quot;activity-level&quot;, &quot;users&quot; |
| Dagelijks activiteitenniveau per huurder | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;activity-level&quot;, &quot;huurder&quot; | cmu/v2/jaar/maand/dag/activiteit-niveau/huurder | &quot;activity-level&quot;, &quot;huurder&quot;, &quot;gebruikers&quot; |
| Dagelijks activiteitenniveau per huurder mvpd | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;activity-level&quot;, &quot;huurder&quot;, &quot;mvpd&quot; | cmu/v2/jaar/maand/dag/activiteit-niveau/huurder/mvpd | &quot;activity-level&quot;, &quot;huurder&quot;, &quot;mvpd&quot;, &quot;gebruikers&quot; |

*TODO:controleer met BG als de gelijktijdige en activiteitenniveaurapporten correct zijn *

## Voorbeelden van Uurrapporten {#hourly-reports-examples}

| Naam | Dimensionen | URL | Metrisch |
|-------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Uurstatus van toepassing in een andere gebruiker | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;huurder&quot;, &quot;application-id&quot; | cmu/v2/year/month/day/hour/huurder/application-id | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Toepassing die door meerdere gebruikers wordt gebruikt, begint met naam en platform van de toepassing | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;huurder&quot;, &quot;application-id&quot;, &quot;application&quot;, &quot;platform&quot; | cmu/v2/year/month/day/hour/huurder/application-id/application/platform | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Uurstatus mvpd/platform voor meerdere gebruikers | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;huurder&quot;, &quot;mvpd&quot;, &quot;platform&quot;, &quot;application-id&quot; | cmu/v2/year/month/day/hour/huurder/mvpd/platform/application-id | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Mvpd/platform-uurs statussen voor meerdere gebruikers met toepassingsnaam | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;huurder&quot;, &quot;mvpd&quot;, &quot;platform&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/year/month/day/hour/huurder/platform/application-id/application | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Uurstaten van het platform voor meerdere gebruikers | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;huurder&quot;, &quot;platform&quot;, &quot;application-id&quot; | cmu/v2/year/month/day/hour/huurder/platform/application-id | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Platformstatus van meerdere gebruikers per uur met toepassingsnaam | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;huurder&quot;, &quot;platform&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/year/month/day/hour/huurder/platform/application-id/application | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Kanaal-/platformstatus van meerdere gebruikers | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;huurder&quot;, &quot;channel&quot;, &quot;platform&quot;, &quot;application-id&quot; | cmu/v2/year/month/day/hour/huurder/channel/platform/application-id | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Kanaal-/platformstatus voor meerdere gebruikers per uur met toepassingsnaam | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;huurder&quot;, &quot;channel&quot;, &quot;platform&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/year/month/day/hour/huurder/channel/platform/application-id/application | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Cross-MVPD-uurs statussen | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;mvpd&quot; | cmu/v2/jaar/maand/dag/uur/mvpd/ | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Uurstaten van Cross-MVPD-huurder | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;mvpd&quot;, &quot;owner | cmu/v2/jaar/maand/dag/uur/mvpd/huurder | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Uurstatus van Cross-MVPD-huurder/toepassing | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;mvpd&quot;, &quot;huurder&quot;, &quot;application-id&quot; | cmu/v2/year/month/day/hour/mvpd/huurder/application-id | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Cross-MVPD-huurder/toepassing begint per uur met toepassingsnaam en -platform | &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;mvpd&quot;, &quot;huurder&quot;, &quot;application-id&quot; , &quot;application&quot;, &quot;platform | cmu/v2/year/month/day/hour/mvpd/huurder/application-id/application/platform | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-essies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;kill-essies&quot; |
| Uurstatus per huurder | &quot;huurder&quot;, &quot;jaar&quot;, &quot;maand&quot;, &quot;dag&quot;, &quot;uur&quot; | cmu/v2/huurder/jaar/maand/dag/uur | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Aanduidingen per huurder | &quot;huurder&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;application-id&quot; | cmu/v2/huurder/jaar/maand/dag/uur/toepassings-id | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| De per-huurdertoepassing begint per uur met toepassingsnaam | &quot;huurder&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/huurder/jaar/maand/dag/uur/toepassings-id/toepassing | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Uurstaten per huurder mvpd | &quot;huurder&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;mvpd&quot;, &quot;platform&quot;, &quot;application-id&quot; | cmu/v2/huurder/jaar/maand/dag/uur/mvpd/platform/application-id | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| MVPD-uurwerk per huurder staat met toepassingsnaam | &quot;huurder&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;mvpd&quot;, &quot;platform&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/huurder/jaar/maand/dag/uur/mvpd/platform/application-id/application | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Per-huurder kanaal/platform uurstaten | &quot;huurder&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;channel&quot;, &quot;platform&quot;, &quot;application-id&quot; | cmu/v2/huurder/jaar/maand/dag/uur/kanaal/platform/application-id | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Per-huurderkanaal/platform staat per uur met toepassingsnaam | &quot;huurder&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;channel&quot;, &quot;platform&quot;, &quot;application-id&quot;, &quot;application&quot; | cmu/v2/huurder/year/month/day/hour/channel/platform/application-id/application | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Per-MVPD-uurstatistieken | &quot;mvpd&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot; | cmu/v2/mvpd/jaar/maand/dag/uur | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
| Per-MVPD huurder uurstatistieken | &quot;mvpd&quot;, &quot;year&quot;, &quot;month&quot;, &quot;day&quot;, &quot;hour&quot;, &quot;huurder&quot; | cmu/v2/mvpd/jaar/maand/dag/uur/huurder | &quot;active-users&quot;, &quot;active-sessies&quot;, &quot;started-sessies&quot;, &quot;completed-sessies&quot;, &quot;failed-proberen&quot;, &quot;disterminated-sessies&quot;, &quot;duration_0-15&quot;, &quot;duration_15-30&quot;, &quot;duration_30-60&quot;, &quot;duration_60-120&quot;, &quot;duration_2h-4h&quot;, &quot;duration_4h-8h&quot;, &quot;duration_8h-16h&quot;, &quot;duration_16h-1d&quot;, &quot;duration_1d-3d&quot;, &quot;duration_3d-7d&quot;, &quot;duration_1w-1m&quot;, &quot;duration_over-1m&quot; |
