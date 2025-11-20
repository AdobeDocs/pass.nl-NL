---
title: Belangrijke concepten
description: Leer de fundamentele concepten Gelijktijdige Controle met inbegrip van zittingen, beleid, meta-gegevens, en meer
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 0%

---


# Belangrijke concepten {#key-concepts}

Kennis van de kernconcepten van Gelijktijdige bewaking is van essentieel belang voor een geslaagde implementatie. Deze handleiding legt de fundamentele bouwstenen en de manier waarop ze samenwerken uit.

## Basisconcepten {#core-concepts}

### Sessies

A **zitting** vertegenwoordigt een actieve video het stromen instantie. Beschouw het als een &quot;ticket&quot; waarmee een gebruiker inhoud kan bekijken.

#### Sessielevenscyclus

1. **Creatie** - wanneer de gebruiker begint te letten op inhoud
2. **Actief** - terwijl de gebruiker kijkt (die door hartslagen wordt gehandhaafd)
3. **Beëindiging** - wanneer de gebruiker houdt letten op of zitting verloopt

#### Sessieeigenschappen

```json
{
  "sessionId": "unique-session-identifier",
  "idp": "identity-provider",
  "subject": "user-identifier",
  "startTime": "2024-01-15T10:30:00Z",
  "expiresAt": "2024-01-15T10:40:00Z",
  "metadata": {
    "deviceId": "device-123",
    "channel": "Channel1",
    "contentType": "live"
  }
}
```

### Beleid

**Beleid** is bedrijfsregels die grenzen en beperkingen voor gelijktijdig het stromen bepalen. Ze bepalen wanneer gebruikers nieuwe streams kunnen starten.

#### Beleidscomponenten

- **Regels** - Specifieke grenzen en voorwaarden
- **Vereisten van Meta-gegevens** - welke gegevens nodig zijn
- **Logica van de Evaluatie** - hoe het beleid wordt toegepast

#### Voorbeeldbeleid

```json
{
  "name": "basic_stream_limit",
  "description": "Maximum 3 concurrent streams per user",
  "rules": [
    {
      "type": "maxstreams",
      "limit": 3,
      "message": "You have reached your maximum of 3 concurrent streams"
    }
  ]
}
```

### Metagegevens

**Meta-gegevens** verstrekt context over elke zitting. Het bevat informatie over het apparaat, de inhoud, de gebruiker en de toepassing.

#### Metagegevenscategorieën

| Categorie | Voorbeelden | Doel |
|-----------------|------------------------------------------|---------------------------------|
| **Apparaat** | `deviceId`, `deviceType`, `osName` | Apparaten identificeren en categoriseren |
| **Inhoud** | `channel`, `contentType`, `assetId` | Beschrijven wat wordt gecontroleerd |
| **Gebruiker** | `subject`, `subscriptionTier` | Gebruikersspecifieke informatie |
| **Toepassing** | `applicationName`, `applicationPlatform` | Toepassingsspecifieke details |
| **Plaats** | `country`, `hba` | Geografische informatie en netwerkinformatie |

#### Vereiste vs. optionele metagegevens

- **Vereiste meta-gegevens** - moet voor zittingsverwezenlijking worden verstrekt
- **Facultatieve meta-gegevens** - kan voor verbeterd beleid worden verstrekt
- **Standaard meta-gegevens** - Vooraf bepaalde gebieden beschikbaar aan alle toepassingen
- **meta-gegevens van de Douane** - toepassing-specifieke gebieden u bepaalt

## Identiteit en verificatie {#identity-and-authentication}

### Identiteitsprovider (IdP)

Een **Leverancier van de Identiteit** is de dienst die gebruikers voor authentiek verklaart en hun identiteitsinformatie verstrekt. In Adobe Pass-integratie is dit meestal een MVPD.

#### IdP-voorbeelden

- Kabelbedrijven (Comcast, Charter, enz.)
- Satellietaanbieders (DirecTV, Dish)
- Streaming services (Netflix, Hulu)
- Mobiele dragers (Verizon, AT&amp;T)

### Onderwerp

A **onderwerp** is het unieke herkenningsteken voor een gebruiker binnen een Leverancier van de Identiteit. Dit is doorgaans de account-id of abonnee-id van de gebruiker.

## Sessiebeheer {#session-management}

### Heartbeats

**Hartbeats** zijn periodieke API vraag die zittingen actief houdt. Ze vertellen het systeem: &quot;Deze gebruiker kijkt nog steeds.&quot;

#### Vereisten voor hartslag

- **Frequentie** - om de 60 seconden (geadviseerd)
- **Doel** - Levende zitting houden, update meta-gegevens
- **Beëindiging** - de Zitting verloopt als de hartslagen ophouden


### Sessiebeëindiging

Sessies kunnen op verschillende manieren worden beëindigd:

1. **de einden van de Gebruiker letten op** - de vraag van de Toepassing het eindpunt van DELETE
2. **Zitting verloopt** - Geen hartslag die binnen onderbreking wordt ontvangen
3. **de schending van het Beleid** - de Nieuwe zitting beëindigt oude (FIFO)
4. **Verre beëindiging** - Een ander apparaat beëindigt de zitting

## Beleidsevaluatie {#policy-evaluation}

### Hoe het beleid werkt

1. **verzoekt de Gebruiker nieuwe stroom**
2. **het Systeem evalueert beleid** tegen huidig gebruik
3. **Beslissing gemaakt** - staat of ontkent toe
4. **Verzonden Reactie** - Succes of conflictinformatie

## Conflict oplossen {#conflict-resolution}

### Wat is een conflict?

A **conflict** komt voor wanneer een gebruiker probeert om een nieuwe stroom te beginnen maar hun grenzen zou overschrijden.

### Conflict antwoord

Wanneer een conflict voorkomt, keert het systeem een 409 Conflict reactie met gedetailleerde informatie terug:

```json
{
  "status": "error",
  "error": {
    "code": "POLICY_VIOLATION",
    "message": "Concurrent usage limit exceeded"
  },
  "evaluationResult": {
    "decision": "DENY",
    "associatedAdvice": [
      {
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "max_streams",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {...}
            }
          ]
        }
      }
    ]
  }
}
```

### Resolutiestrategieën

#### LIFO (laatst in, eerst uit)

- **Oude zittingen worden beschermd**
- **Nieuwe zitting wordt geblokkeerd**
- **Eenvoudigere UI**
- **beter voor uitgebreide het bekijken**


#### FIFO (eerste in, eerste uit)

- **de Nieuwe zitting beëindigt bestaande**
- **Gebruiker kiest welke zitting te houden**
- **complexere vereiste UI**
- **beter voor inhoudsomschakeling**

## Aanvragen en huurders {#applications-and-tenants}

### Toepassing

Een **toepassing** is een softwareprogramma dat met Gelijktijdige Controle integreert. Elke toepassing heeft een unieke id en kan een eigen beleid hebben.

#### Voorbeelden van toepassingen

- Mobiele app (iOS/Android)
- Webtoepassing
- Slimme tv-app
- Toepassing van vak Boven instellen

### Aannemer

A **huurder** is een organisatie die één of meerdere toepassingen bezit. De huurders kunnen beleid over toepassingen delen.

#### Tenantstructuur

```
Tenant: "Streaming Company"
├── Application: "Mobile App"
├── Application: "Web App"
└── Application: "TV App"
```

## Omgevingen {#environments}

### Stationele omgeving

- **Doel** - Ontwikkeling en het testen
- **URL** - `https://streams-stage.adobeprimetime.com/v2/`
- **Geloofsbrieven** - toepassings IDs van de test
- **Gegevens** - de Gegevens van de Test slechts

### Productie-omgeving

- **Doel** - Levend gebruikersverkeer
- **URL** - `https://streams.adobeprimetime.com/v2/`
- **Geloofsbrieven** - toepassings IDs van de productie
- **Gegevens** - Echte gebruikersgegevens

## Integratiestroom {#integration-flow}

### Basisstroom

1. **Authentificatie van de Gebruiker** - verifieer met Adobe Pass
2. **de Making van de Zitting** - creeer de zitting van CM met gebruikersmeta-gegevens
3. **Beheer van de Hartslag** - verzend periodieke hartslagen
4. **Beëindiging van de Zitting** - Eindig wanneer de gebruiker het letten op ophoudt

### Foutafhandeling

1. **404 niet Gevonden** - de verzoeken van de handvat met zittingsidentiteitskaart niet die door de dienst van cm wordt geproduceerd
2. **409 Conflicten** - de beleidsschendingen van de Handle
3. **410 Gone** - de zittingsbeëindiging van de greep
4. **de Fouten van het Netwerk** - de kwesties van de de connectiviteitsbehandeling
5. **de Fouten van de Authentificatie** - de crediteurkwesties van de Handle


## Algemene terminologie {#common-terminology}

| Term | Definitie |
|-----------------|--------------------------------------------|
| **Zitting** | Actieve instantie voor videostreaming |
| **Beleid** | Zakelijke regel die gebruikslimieten definieert |
| **Metagegevens** | Contextuele informatie over sessies |
| **IDP** | Identiteitsprovider (verificatieservice) |
| **Onderwerp** | Gebruiker-id binnen een IDP |
| **Hartslag** | Periodieke vraag om zitting actief te houden |
| **Conflict** | Beleidsovertreding bij het starten van nieuwe stream |
| **LIFO** | Laatste in, eerste uit conflictoplossing |
| **FIFO** | Eerste in, eerste uit conflictoplossing |
| **Aannemer** | Bedrijven die toepassingen bezitten |
| **Toepassing** | Softwareprogramma met CM |

