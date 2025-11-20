---
title: API-eindpunten
description: Volledige lijst met API's voor gelijktijdige bewaking
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '54'
ht-degree: 0%

---


# API-eindpunten

## Core Session Management

| Endpoint | Methode | Beschrijving |
|---------------------------------------|--------|---------------------------------------|
| `/sessions/{idp}/{subject}` | POST | Een nieuwe streaming sessie maken |
| `/sessions/{idp}/{subject}/{session}` | POST | Hartslag verzenden om de sessie in leven te houden |
| `/sessions/{idp}/{subject}/{session}` | DELETE | Een sessie beëindigen |
| `/runningStreams/{idp}/{subject}` | GET | Alle actieve sessies voor een onderwerp ophalen |

## Metagegevensbeheer

| Endpoint | Methode | Beschrijving |
|-------------|--------|----------------------------------------------|
| `/metadata` | GET | Vereiste metagegevensvelden voor toepassing ophalen |
