---
product: adobe primetime
feature: Concurrency Monitoring
audience: end-user
user-guide-title: Gelijktijdige bewaking van Adobe Pass
user-guide-description: Leer hoe u limieten kunt definiëren en afdwingen voor gelijktijdig gebruik in meerdere applicaties.
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 5%

---


# Hulp bij Adobe Pass-controle op gelijktijdige valuta {#cm}

- [&#x200B; Inleiding CM &#x200B;](cm-home.md) {#cm-intro}
- Aan de slag {#getting-started}
   - [Aan de slag met Gelijktijdige controle](getting-started/getting-started-overview.md)
   - [Belangrijke concepten](getting-started/key-concepts.md)
- Integratiegids {#integration-guide}
   - [Verklarende woordenlijst](cm-glossary.md)
   - API-naslag {#api-reference}
      - [&#x200B; Overzicht &#x200B;](api/api-reference-overview.md)
      - [API-eindpunten](api/api-endpoints.md)
      - [API-naslag](api/cm-api-overview.md)
   - [API voor gelijktijdige bewaking](reports/cmu-api.md)
   - [API-toegang](reports/cmu-api-access.md)
   - [Voorbeelden van gebruiksrapporten voor gelijktijdige bewaking](reports/cm-usage-reports-examples.md)
- Gevallen en implementatie gebruiken {#use-cases-implementation}
   - [Overzicht van gevallen gebruiken](use-cases/cm-use-cases.md)
   - [LIFO versus FIFO-strategieën](use-cases/lifo-fifo-strategies.md)
   - [Sessieconflicten afhandelen](use-cases/handling-409-errors.md)
   - [Uitvoeringsmodellen](technical/implementation-models.md)
   - [Single Tenant Policy Multiple Apps](technical/single-tenant-policy-mult-app.md)
   - [Gelijktijdig gebruik van meerdere toepassingen beperken](technical/restrict-concurr-usage-mult-apps.md)
- [&#x200B; de Rapporten van het Gebruik van CM &#x200B;](reports/cm-usage-reports.md) {#cm-usage-reports}
- Technische notities {#tech-notes}
   - [Standaardmetagegevenskenmerken](technical/standard-metadata-attributes.md)
   - [Aangepaste metagegevens](technical/custom-metadata.md)
   - [Beleidsbeslissingspunt](technical/cm-policy-decision-point.md)
   - [Beleidsinformatie](technical/policy-info-pt-versionone.md)
   - [Throttling](technical/throttling-mechanism.md)
   - [Hoe te: Distinguished between VOD and Live Content in Concurrency Monitoring](technical/vod-live-dist.md)
   - [Beleid voor gegevensbewaring](technical/data-retention-policy.md)
- Uitstoot {#cm-release-notes}
   - [Gelijktijdige bewaking - Opmerkingen bij de release 3.6.2](releases/rn-cm-services-362.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 3.6.1](releases/rn-cm-services-361.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 3.6.0](releases/rn-cm-services-360.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 3.5.1](releases/rn-cm-services-351.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 3.5.0](releases/rn-cm-services-350.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 3.4.4](releases/rn-cm-services-344.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 3.4.3](releases/rn-cm-services-343.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 3.4.2](releases/rn-cm-services-342.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 3.4.0](releases/rn-cm-services-340.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 3.3.7](releases/rn-cm-services-337.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 3.3.6](releases/rn-cm-services-336.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 3.3.2](releases/rn-cm-services-332.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 3.3.1](releases/rn-cm-services-331.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 3.3.0](releases/rn-cm-services-330.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 3.2.3](releases/rn-cm-services-323.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 3.2.2](releases/rn-cm-services-322.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 3.2.1](releases/rn-cm-services-321.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 3.2](releases/rn-cm-services-320.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 3.1](releases/rn-cm-services-31.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 3.0](releases/rn-cm-services-30.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release van 2.9.6](releases/rn-cm-296.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release van 2.9](releases/rn-cm-29.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 2.8.2](releases/rn-cm-282.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 2.7.2](releases/rn-cm-272.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 2.6.0](releases/rn-cm-260.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 2.5.0](releases/rn-cm-250.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 2.3.2](releases/rn-cm-232.md)
   - [Gelijktijdige bewaking - Opmerkingen bij de release 2.2.2](releases/rn-cm-222.md)
- [&#x200B; procedures van de Steun &#x200B;](support/cm-escalation-procedures.md) {#support-procedures}
