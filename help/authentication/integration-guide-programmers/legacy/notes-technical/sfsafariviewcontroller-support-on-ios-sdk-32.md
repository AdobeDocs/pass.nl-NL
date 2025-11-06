---
title: Ondersteuning voor SFSafariViewController op iOS SDK 3.2+
description: Ondersteuning voor SFSafariViewController op iOS SDK 3.2+
exl-id: 6691550f-c36f-4fae-aa77-082ca7d8a60a
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# (Verouderd) SFSafariViewController-ondersteuning op iOS SDK 3.2+ {#sfsafariviewcontroller-support-on-ios-sdk-3.2}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [&#x200B; pagina van de Mededelingen van het Product &#x200B;](/help/authentication/product-announcements.md) wordt samengevoegd.

</br>


**wegens veiligheidsvereisten, moeten sommige MVPD login pagina&#39;s in een SFSafariViewController, in plaats van Webmeningen worden voorgesteld.**

Sommige MVPD vereisen dat hun login pagina&#39;s in een veilige browser controle zoals SFSafariViewController worden voorgesteld. Ze blokkeren actief webweergaven, dus om ermee te kunnen verifiëren moeten we SVC gebruiken.

## Compatibiliteit {#compatiblity}

Vanaf iOS SDK versie 3.1 wordt de aanmeldingspagina voor specifieke MVPD automatisch weergegeven in een SFSafariViewController, gebaseerd op de serverconfiguratie.

Versie 3.1 van de SDK stelt automatisch SFSafariViewController voor vanuit de hoofdweergavecontroller van de toepassing. Hoewel dit het beheer van aanmeldingspagina&#39;s voor implementoren vereenvoudigt, zijn er gevallen waarin het presenteren van de SFSafariViewController vanuit de hoofdweergavecontroller niet mogelijk is vanwege de specifieke implementatie van de app (zoals een modaal controller die al zichtbaar is).

In dergelijke gevallen introduceert de 3.2-versie de mogelijkheid voor de programmeur om de SVC handmatig te beheren.

## Handmatig SVC-beheer {#manual-svc-management}

Om SVC manueel te beheren moet de implementor de volgende stappen uitvoeren:


1. vraag **setOptions ([ &quot;handleSVC&quot;:true])** na de initialisering AccessEnabler (zorg ervoor deze vraag wordt uitgevoerd alvorens de authentificatie begint). Hierdoor wordt &quot;handmatig&quot; SVC-beheer ingeschakeld, zal de SDK de SVC niet automatisch presenteren, maar in plaats daarvan, wanneer dat nodig is     vraag **navigate (toUrl:*{url}* useSVC :true)**.

1. Implementeer de optionele callback **`navigateToUrl:useSVC:`** binnen de implementatie en u moet een svc-instantie maken met behulp van de SFSafariViewController-instantie met behulp van de opgegeven url, en deze op het scherm presenteren:

   ```obj-c
   func navigate(toUrl url: String!, useSVC: Bool) {
       svc =  SFSafariViewController(url: URL(string: url)!)
       svc.delegate = self
       myController.present(svc, animated: true)
       }
   ```

   ***Nota&#39;s:***

   - *u kunt SFSafariViewController aanpassen om het even welke manier u wilt. Op iOS 11+ kunt u bijvoorbeeld het label &quot;Done&quot; wijzigen in &quot;Cancel&quot;.*
   - *om svc te kunnen ontslaan, hebt u een verwijzing naar het nodig, te creëren gelieve niet het in het werkingsgebied van **navigateToUrl:useSVC***
   - *gebruik uw eigen meningscontrolemechanisme voor &quot;myController&quot;*


1. In de gedelegeerde implementatie van uw toepassing van **toepassing (\_app: UIApplication, open url: URL, opties: \[UIApplicationOpenURLOptionsKey: Om \]) - \> Bool**, voeg code toe om svc te sluiten. U zou wat code daar reeds moeten hebben die **accessEnabled.handleExternalURL ()** roept. Net onder toevoegen:

   ```obj-c
   if(svc != nil) {
       svc.dismiss(animated: true)
   }
   ```

   Opnieuw, is svc een verwijzing naar SFSafariViewController u bij stap 2 creeerde.


1. Voer **safariViewControllerdidFinish (\_ controlemechanisme: SFSafariViewController)** uit van **SFSafariViewControllerDelegate** om te vangen wanneer de gebruiker svc gebruikend de &quot;Gedaane&quot;knoop annuleerde. In deze functie moet u, om de SDK te informeren dat verificatie is geannuleerd, het volgende oproepen:

   ```obj-c
   accessEnabler.setSelectedProvider(nil)
   ```
