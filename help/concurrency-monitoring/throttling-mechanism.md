---
title: Draaimechanisme
description: Draaimechanisme
source-git-commit: cbb45cae576332e2b63027992c597b834210988d
workflow-type: tm+mt
source-wordcount: '624'
ht-degree: 1%

---


# Draaimechanisme {#throttling-mechanism}

## Inleiding {#introduction}

Adobe moet in zijn rol als gegevensverwerker de nodige maatregelen nemen om ervoor te zorgen dat de gebruikers van onze klanten op billijke wijze gebruikmaken van bronnen en dat de service niet overstroomd wordt met onnodige API-verzoeken. Hiervoor hebben we een vertragingsmechanisme ingesteld.
Eén toepassing voor gelijktijdige bewaking kan door meerdere gebruikers worden gebruikt en één gebruiker kan meerdere sessies hebben. Daarom zal de dienst grenzen hebben die voor het aantal toegelaten vraag per gebruiker/zitting binnen een specifiek tijdinterval worden gevormd.
Wanneer de grens is bereikt, zullen de verzoeken met een specifieke reactiestatus (HTTP 429 Te veel Verzoeken) worden gemerkt. Om het even welke verdere vraag die na een &quot;429 Te Vele Antwoord van Verzoeken&quot;wordt gedaan zou met minstens één minuut koele down periode moeten worden gedaan om ervoor te zorgen het een geldige bedrijfsreactie zal verkrijgen.

## Overzicht van het mechanisme {#mechanism-overview}

Het mechanisme bepaalt het maximumaantal toegelaten vraag voor elk eindpunt van de Controle van de Valuta binnen een specifiek tijdinterval.
Zodra dit maximumaantal vraag is bereikt, zal onze dienst met &quot;429 Te veel verzoeken&quot;antwoorden. De header 429 response &quot;Expires&quot; bevat de tijdstempel wanneer de volgende aanroep als geldig wordt beschouwd of wanneer de vertraging verloopt. Op dit moment verloopt de vertraging na één minuut vanaf de eerste 429 reactie.

De eindpunten die met throttling worden gevormd zijn:
1. Een nieuwe sessie maken: POST /sessies/{idp}/{subject}
2. Aanroep hartslag: POST /sessies/{idp}/{subject}/{sessionId}
3. Een sessie beëindigen: DELETE /sessies/{idp}/{subject}/{sessionId}

De vertraging wordt gevormd op twee niveaus:
1. sessie: dezelfde unieke {sessionId} parameter verzonden in `Heartbeat` oproepen en `Terminate a session` vraag.
2. gebruiker: zelfde uniek {subject} parameter verzonden in `Create a new session` vraag.

De limiet voor het wijzigen van het sessieniveau is ingesteld op 200 verzoeken binnen één minuut.\
De limiet voor het wijzigen van de snelheid op gebruikersniveau is ingesteld op 200 verzoeken binnen één minuut.\
Beide grenzen (de throttling van het zittingsniveau en gebruiker - niveauthrottling) zijn configureerbaar, en wij zullen hen bijwerken voor het geval dat zij door geldige integratiescenario&#39;s zullen worden bereikt. Hiervoor raden we u aan contact op te nemen met het ondersteuningsteam.

**Scenario voor het vertragen van het zittingsniveau:**

| Tijd | Verzoek om verzending naar CM | Aantal verzoeken | Antwoord ontvangen van CM | Toelichting |
|-----------|-----------------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Tweede 10 | POST /sessies/idp1/subject1/session1 | 50 | Alle oproepen worden &quot;202 geaccepteerd&quot; | 50 vraag die van de grens wordt verbruikt |
| Tweede 50 | POST /sessies/idp1/subject1/session1 | 151 | 150 oproepen ontvangen &quot;202 Erkend&quot;en 1 vraag ontvangt &quot;429 Te veel verzoeken&quot; | 200 vraag die van de grens wordt verbruikt en 1 vraag zal 429 reactie ontvangen |
| Tweede 61 | DELETE /sessies/idp1/subject1/session1 | 1 | 1 oproep ontvangt &quot;429 Te veel verzoeken&quot; | Er zijn nog geen aanroepen beschikbaar binnen de limiet |
| Tweede 70 | DELETE /sessies/idp1/subject1/session1 | 1 | 1 uitnodiging ontvangt &quot;202 Accepted&quot; | Limiet die aan 200 beschikbare vraag wordt geplaatst omdat 60 seconden sinds tweede 10 zijn overgegaan |

**Scenario voor het vertragen van gebruikersniveau:**

| Tijd | Verzoek om verzending naar CM | Aantal verzoeken | Antwoord ontvangen van CM | Toelichting |
|-----------|------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Tweede 10 | POST/sessies/idp1/subject1 | 50 | 50 oproepen ontvangen &quot;202 geaccepteerd&quot; | 50 vraag die van de grens wordt verbruikt |
| Tweede 50 | POST/sessies/idp1/subject1 | 151 | 150 oproepen ontvangen &quot;202 Erkend&quot;en 1 vraag ontvangt &quot;429 Te veel verzoeken&quot; | 200 vraag die van de grens wordt verbruikt en 1 vraag zal 429 reactie ontvangen |
| Tweede 61 | POST/sessies/idp1/subject1 | 1 | 1 oproep ontvangt &quot;429 Te veel verzoeken&quot; | Er zijn nog geen aanroepen beschikbaar binnen de limiet |
| Tweede 70 | POST/sessies/idp1/subject1 | 1 | 1 uitnodiging ontvangt &quot;202 Accepted&quot; | Limiet die aan 200 beschikbare vraag wordt geplaatst omdat 60 seconden sinds tweede 10 zijn overgegaan |

**Voorbeeld van reactie:**

```
HTTP/2 429
date: Thu, 15 Feb 2024 07:54:20 GMT
content-length: 0
vary: Origin
vary: Access-Control-Request-Method
vary: Access-Control-Request-Headers
cache-control: no-store
expires: Thu, 15 Feb 2024 07:54:41 GMT
strict-transport-security: max-age=31536000 ; includeSubDomains
x-xss-protection: 1; mode=block
x-frame-options: DENY
x-content-type-options: nosniff
```

## Aanbevelingen voor klantintegratie {#customer-integration-recommendations}

Met een correcte implementatie, zullen de klanten &quot;429 Te veel Verzoeken&quot;reactie niet ontvangen.
Toch adviseert de Adobe dat elke klant &quot;429 Te Vele Antwoord van Verzoeken&quot;geschikt gebruikend de hierboven vermelde technische details behandelt. Wanneer het behandelen van de reactie, &quot;verloopt&quot;kopbal zou moeten worden gebruikt om te bepalen wanneer om het volgende geldige verzoek te verzenden.
