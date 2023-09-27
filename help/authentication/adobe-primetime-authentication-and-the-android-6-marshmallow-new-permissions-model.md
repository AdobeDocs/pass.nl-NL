---
title: Adobe Pass-verificatie en het nieuwe machtigingenmodel voor Android 6 "Marshmallow"
description: Adobe Pass-verificatie en het nieuwe machtigingenmodel voor Android 6 "Marshmallow"
exl-id: 3c96769e-b25b-48ab-bb74-40f13d4e5a84
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '480'
ht-degree: 0%

---

# Adobe Pass-verificatie en het nieuwe machtigingenmodel voor Android 6 &quot;Marshmallow&quot; {#adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>

De nieuwe Android 6 Marshmallow-versie introduceert enkele updates van het machtigingenmodel, die het gedrag kunnen beïnvloeden van apps die de bestaande Adobe Pass Authentication SDK versie 1.8 en ouder gebruiken.

Het nieuwe Android-besturingssysteem biedt als nieuwe functie [granulaire controle over de machtigingen die worden vereist op het moment van installatie en bij uitvoering](https://developer.android.com/about/versions/marshmallow/android-6.0-changes.html).

>[!IMPORTANT]
>
>De hieronder beschreven wijzigingen zullen **alleen van toepassing op toepassingen die specifiek zijn ontwikkeld voor Android 6.0** (targetSDKVersion=23). Ze zijn niet van invloed op oudere toepassingen die al op het apparaat van de gebruiker zijn geïnstalleerd tijdens de upgrade naar Android 6.0.


Specifiek voor toepassingen die zijn ontwikkeld in Android Studio met [API-niveau 23](http://developer.android.com/sdk/api_diff/23/changes.html) en die de Adobe Pass Authentication SDK gebruiken, moet de ontwikkelaar aangepaste code schrijven (zie het codefragment hieronder) [om het dialoogvenster voor machtigingen toestaan/weigeren te activeren](https://developer.android.com/training/permissions/requesting.html).

Hier volgt het codefragment dat wordt gebruikt voor het aanvragen van schrijftoegang tot de externe opslag van het apparaat:

```java
// Here, thisActivity is the current activity
if (ContextCompat.checkSelfPermission(thisActivity,
                Manifest.permission.WRITE_EXTERNAL_STORAGE)
        != PackageManager.WRITE_EXTERNAL_STORAGE) {

    // Should we show an explanation?
    if (ActivityCompat.shouldShowRequestPermissionRationale(thisActivity,
            Manifest.permission.WRITE_EXTERNAL_STORAGE)) {

        // Show an expanation to the user *asynchronously* -- don't block
        // this thread waiting for the user's response! After the user
        // sees the explanation, try again to request the permission.

    } else {

        // No explanation needed, we can request the permission.

        ActivityCompat.requestPermissions(thisActivity,
                new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE},
                MY_PERMISSIONS_REQUEST_WRITE_EXTERNAL_STORAGE);

        // MY_PERMISSIONS_REQUEST_WRITE_EXTERNAL_STORAGE is an
        // app-defined int constant. The callback method gets the
        // result of the request.
    }
}
```




**Vanuit het perspectief van de gebruikers** Na de installatie worden gebruikers begroet met een venster waarin ze worden gevraagd lees- en schrijfmachtigingen voor bestanden te bevestigen (zie figuur 2 hieronder). Dit leidt tot één van de volgende twee resultaten:

1. Als de gebruiker **bevestigen** de machtigingen , de normale verificatiestroom en tokens worden opgeslagen in de algemene opslag . Gebruikers blijven geautoriseerd in de app en in verschillende apps met Adobe Pass-verificatie zolang de tokens geldig zijn.
1. Als de gebruiker **ontzegging** de machtigingen, schrijfhandelingen in de opslagruimte mislukken en de gebruikers worden pas geverifieerd als ze de app afsluiten. Houd er rekening mee dat sommige toepassingen opnieuw worden geïnitialiseerd wanneer wordt geschakeld tussen de voor- en achtergrond, zodat de gebruikers worden afgemeld wanneer zij deze handeling uitvoeren. Tokens worden NIET opgeslagen en de gebruikers moeten elke keer dat zij de app gebruiken, worden geverifieerd.


>[!TIP]
>
>Er is momenteel een functie in ontwikkeling voor de Adobe Pass Authentication SDK 1.9 die de opslagveerkracht introduceert. De nieuwe SDK is bedoeld voor **vrijgave in de laatste week van oktober**. Wanneer de algemene opslag niet kan worden gebruikt, wordt de toepassing opnieuw opgeslagen in de sandbox van de toepassing. Dit geldt voor het geval waarin gebruikers voor toepassingen die zijn ontwikkeld op API-niveau 23 GEEN lees-/schrijfmachtigingen accepteren in de algemene opslag. De tokens worden afzonderlijk opgeslagen per app. Dit betekent dat Single Sign-On tussen apps die Adobe Pass-verificatie gebruiken, wordt uitgeschakeld.


![](assets/android-permissions-request.png)

*Afbeelding: Het dialoogvenster voor het aanvragen van machtigingen voor toepassingen die zijn geschreven voor API-niveau 23*

>[!IMPORTANT]
>
> Adobe adviseert **zijn partners om toepassingen te ontwikkelen met gebruik van API-niveau 22 (targetSdkVersion=22) of ouder, zodat een optimale gebruikerservaring bij het verificatieproces wordt gegarandeerd.**.
