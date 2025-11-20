---
title: Overzicht API-naslaggids
description: Volledige referentie voor de API voor gelijktijdige bewaking, inclusief eindpunten, verificatie en responsindelingen
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 2%

---


# Overzicht API-naslaggids {#api-reference-overview}

De API voor gelijktijdige bewaking biedt een RESTful-interface voor het beheren van streaming sessies en het afdwingen van beleid voor gelijktijdig gebruik. Deze naslaggids biedt volledige documentatie voor alle eindpunten, verificatiemethoden, indelingen voor aanvragen/antwoorden en foutafhandeling.

## API Basis-URL&#39;s

### Productie-omgeving

```
https://streams.adobeprimetime.com/v2/
```

### Stationele omgeving

```
https://streams-stage.adobeprimetime.com/v2/
```

**Nota:** gebruik altijd het het opvoeren milieu voor ontwikkeling en het testen. De geloofsbrieven van de productie worden verstrekt slechts na succesvolle het opvoeren integratie.

## Verificatie

Voor alle API-aanroepen is HTTP Basic-verificatie vereist met behulp van uw toepassingsreferenties:

- **Gebruikersnaam:** Uw toepassingsidentiteitskaart (die door Adobe wordt verstrekt)
- **Wachtwoord:** Leeg koord

### Voorbeeld van verificatieheader

```bash
curl -u "<your-app-id>:" https://streams-stage.adobeprimetime.com/v2/sessions

For an application with id "demo-app" the authentication header would be exactly as shown below, including the quotes and colon:
curl -u "demo-app:" https://streams-stage.adobeprimetime.com/v2/sessions
```

## Standaardinstellingen voor responsindeling

### Geslaagde reacties

Alle succesvolle reacties volgen deze structuur:

```json
{
  "status": "success",
  "data": {
    // Response-specific data
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

### Foutreacties

Alle foutreacties volgen deze structuur:

```json
{
  "associatedAdvice": [
    {
      "policyName": "string",
      "ruleName": "string",
      "scope": {},
      "attribute": "string",
      "threshold": 0,
      "conflicts": [
        {}
      ]
    }
  ],
  "obligations": [
    {
      "namespace": "string",
      "action": "string",
      "arguments": [
        "string"
      ]
    }
  ]
}
```

### Evaluatieresultaat

Wanneer het beleid wordt geëvalueerd (vooral voor 409 conflicten), omvatten de reacties een evaluatieresultaat:

```json
{
  "evaluationResult": {
    "decision": "DENY",
    "obligations": [
      {
        "id": "obligation-id",
        "fulfillOn": "DENY",
        "attributes": {
          "attribute1": "value1"
        }
      }
    ],
    "associatedAdvice": [
      {
        "id": "advice-id",
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "rule-name",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {
                "deviceId": "device-789",
                "channel": "Channel1"
              }
            }
          ]
        }
      }
    ]
  }
}
```

## Algemene HTTP-statuscodes

| Code | Beschrijving | Indien teruggestuurd |
|------|----------------------|------------------------------------------------|
| 200 | OK | GET-aanvragen succesvol |
| 202 | Gemaakt/geaccepteerd | Geslaagd sessie-ontwerp/hartslag opgenomen |
| 400 | Ongeldig verzoek | Ongeldige parameters of ontbrekende vereiste velden |
| 401 | Ongeautoriseerd | Ongeldige of ontbrekende verificatie |
| 403 | Verboden | Onvoldoende rechten |
| 404 | Sessie-id niet gevonden | Sessie-id niet gegenereerd door CM-service |
| 409 | Conflict | Beleidsovertreding (gelijktijdige limiet bereikt) |
| 410 | Gone | De sessie is verlopen of beëindigd |
| 429 | Te veel verzoeken | Maximumtarief overschreden |

## Methoden voor het doorgeven van parameters

### Padparameters

Vereiste parameters die deel uitmaken van het URL-pad:

1. `{idp}` - ID identiteitsprovider
2. `{subject}` - Gebruikersnaam (doorgaans uit Adobe Pass)
3. `{sessionId}` - Sessie-id (geretourneerd in Locatiekoptekst)

### Aanvullende parameters

Optionele parameters worden doorgegeven in de URL:

```bash
GET /sessions/{idp}/{subject}?platform=test
```

### Formuliergegevens (POST/PUT)

Metagegevens en sessiegegevens in aanvraaginstantie:

```bash
POST /sessions/{idp}/{subject}
Content-Type: application/x-www-form-urlencoded

channel=Channel1&deviceId=device-123&contentType=live
```

### Kopteksten

Speciale parameters die in HTTP-headers worden doorgegeven:

```bash
X-Terminate: termination-code-123
X-Client-Version: 1.0.0
```

## Fout bij het verwerken van aanbevolen procedures

### 409 Conflictbehandeling

Wanneer u een 409 Conflict reactie ontvangt:

1. **ontleed het evaluatieresultaat** om de beleidsschending te begrijpen
2. **Extraheer conflictinformatie** van `associatedAdvice`
3. **presenteer opties aan de gebruiker** die op uw LIFO/FIFO strategie wordt gebaseerd
4. **beëindigingscodes van het Gebruik** als het uitvoeren van het gedrag van LIFO

### 410 Gone-verwerking

Wanneer u een reactie krijgt van 410 Gone:

1. **Controle als de reactie een lichaam** heeft - wijst op verre beëindiging
2. **ontleed advies** om te begrijpen waarom de zitting werd geëindigd
3. **Update UI** om zittingsbeëindiging te wijzen
4. **Behandeling gracieus** - de zitting kan uit tijd natuurlijk hebben getild
5. **Begin een nieuwe zitting** - indien nodig geacht, een nieuwe zitting in werking stellen

### Snelheidsbeperking

Wanneer u 429 Te veel verzoeken ontvangt:

1. **de vraagfrequentie van de grens** tot maximaal 200 verzoeken per minuut, die het maximumniveau is dat door CM wordt goedgekeurd
2. **verzendt hartslagen bij het vereiste interval**, dat één per minuut is.

## Testgereedschappen

### Interactieve API Explorer

Gebruik onze [ Vwagen UI ](https://streams-stage.adobeprimetime.com/swagger-ui/index.html) voor het interactieve testen:

1. Voer uw toepassings-id in de rechterbovenhoek in
2. Klik op Verkennen om verificatie in te stellen
3. Eindpunten testen met reële parameters
4. Voorbeelden van verzoeken/reacties weergeven
