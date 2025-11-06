---
title: Adobe Pass Authentication and the Android 6 "Marshmallow" New Permissions Model
description: Adobe Pass Authentication and the Android 6 "Marshmallow" New Permissions Model
exl-id: 3c96769e-b25b-48ab-bb74-40f13d4e5a84
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '503'
ht-degree: 0%

---

# (Verouderd) Adobe Pass-verificatie en het nieuwe machtigingenmodel voor Android 6 &quot;Marshmallow&quot; {#adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [&#x200B; pagina van de Mededelingen van het Product &#x200B;](/help/authentication/product-announcements.md) wordt samengevoegd.

</br>

De nieuwe Android 6 Marshmallow-versie introduceert enkele updates van het machtigingenmodel, die het gedrag kunnen beïnvloeden van apps die de bestaande Adobe Pass Authentication SDK versie 1.8 en ouder gebruiken.

Als nieuwe eigenschap, biedt nieuw Android OS [&#x200B; korrelige controle over de toestemmingen aan die apps op het tijdstip van installatie en bij runtime &#x200B;](https://developer.android.com/about/versions/marshmallow/android-6.0-changes.html) vereisen.

>[!IMPORTANT]
>
>De hieronder beschreven veranderingen zullen **slechts toepassingen beïnvloeden die specifiek voor Android 6.0** worden ontwikkeld (targetSdkVersion=23). Ze zijn niet van invloed op oudere toepassingen die al op het apparaat van de gebruiker zijn geïnstalleerd tijdens de upgrade naar Android 6.0.


Specifiek, voor apps die in de Studio van Android worden ontwikkeld gebruikend [&#x200B; API niveau 23 &#x200B;](http://developer.android.com/sdk/api_diff/23/changes.html) en die de Authentificatie SDK van Adobe Pass gebruiken, zal de ontwikkelaar douanecode (zie codefragment hieronder) [&#x200B; moeten schrijven om toe te laten/ontkennen toestemmingendialoog &#x200B;](https://developer.android.com/training/permissions/requesting.html) te teweegbrengen.

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




**Van het perspectief van de gebruikers**, op installatie, worden de gebruikers begroet door een venster die hen ertoe aanzetten om lees/schrijftoestemmingen voor dossiers (zie hieronder figuur 2) te bevestigen. Dit leidt tot één van de volgende twee resultaten:

1. Als de gebruiker **&#x200B;**&#x200B;bevestigt de toestemmingen, zal de regelmatige authentificatiestroom worden gehouden en de tekenen zullen in de globale opslag worden opgeslagen. Gebruikers blijven geautoriseerd in de app en in verschillende apps met Adobe Pass-verificatie zolang de tokens geldig zijn.
1. Als de gebruiker **&#x200B;**&#x200B;ontkent de toestemmingen, schrijf acties in de opslag zullen ontbreken en de gebruikers zullen slechts voor authentiek worden verklaard tot zij app weggaan. Houd er rekening mee dat sommige toepassingen opnieuw worden geïnitialiseerd wanneer wordt geschakeld tussen de voor- en achtergrond, zodat de gebruikers worden afgemeld wanneer zij deze handeling uitvoeren. Tokens worden NIET opgeslagen en de gebruikers moeten elke keer dat zij de app gebruiken, worden geverifieerd.


>[!TIP]
>
>Er wordt momenteel een functie ontwikkeld voor de Adobe Pass Authentication SDK 1.9, waarmee opslagveerkracht wordt geïntroduceerd. De nieuwe SDK wordt gericht voor **versie in de laatste week van Oktober**. Wanneer de algemene opslag niet kan worden gebruikt, wordt de toepassing opnieuw opgeslagen in de sandbox van de toepassing. Dit geldt voor het geval waarin gebruikers voor toepassingen die zijn ontwikkeld op API-niveau 23 GEEN lees-/schrijfmachtigingen accepteren in de algemene opslag. De tokens worden afzonderlijk opgeslagen per app. Dit betekent dat Single Sign-On tussen apps die Adobe Pass-verificatie gebruiken, wordt uitgeschakeld.


![](../../../assets/android-permissions-request.png)

*Cijfer: De dialoog van het toestemmingsverzoek voor apps die gericht API niveau 23* worden geschreven

>[!IMPORTANT]
>
> Adobe adviseert **zijn partners om apps te ontwikkelen gebruikend API niveau 22 (targetSdkVersion=22) of ouder om de best mogelijke gebruikerservaring in het authentificatieproces** te waarborgen.
