---
title: Ondersteuning voor SFSafariViewController op iOS SDK 3.2+
description: Ondersteuning voor SFSafariViewController op iOS SDK 3.2+
exl-id: 6691550f-c36f-4fae-aa77-082ca7d8a60a
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 0%

---

# Ondersteuning voor SFSafariViewController op iOS SDK 3.2+ {#sfsafariviewcontroller-support-on-ios-sdk-3.2}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

</br>


**Vanwege beveiligingsvereisten MOETEN sommige aanmeldingspagina&#39;s van het MVPD worden weergegeven in een SFSafariViewController in plaats van in webweergaven.**

Sommige MVPD&#39;s vereisen dat hun aanmeldingspagina&#39;s worden weergegeven in een beveiligde browsercontrole, zoals SFSafariViewController. Ze blokkeren actief webweergaven, dus om ermee te kunnen verifiëren moeten we SVC gebruiken.

## Compatibiliteit {#compatiblity}

Vanaf iOS SDK versie 3.1 geeft de AccessEnabler SDK automatisch de aanmeldingspagina voor specifieke MVPD&#39;s weer in een SFSafariViewController, gebaseerd op de serverconfiguratie.

Versie 3.1 van SDK stelt automatisch SFSafariViewController van het controlemechanisme van de wortelmening van de toepassing voor. Hoewel dit het beheer van aanmeldingspagina&#39;s voor implementoren vereenvoudigt, zijn er gevallen waarin het presenteren van de SFSafariViewController vanuit de hoofdweergavecontroller niet mogelijk is vanwege de specifieke implementatie van de app (zoals een modaal controller die al zichtbaar is).

In dergelijke gevallen introduceert de 3.2-versie de mogelijkheid voor de programmeur om de SVC handmatig te beheren.

## Handmatig SVC-beheer {#manual-svc-management}

Om SVC manueel te beheren moet de implementor de volgende stappen uitvoeren:


1. call **setOptions()[&quot;handleSVC&quot;:true])** na initialisatie AccessEnabler (zorg ervoor deze vraag wordt uitgevoerd alvorens de authentificatie begint). Hierdoor wordt &quot;handmatig&quot; SVC-beheer ingeschakeld, zal de SDK de SVC niet automatisch presenteren, maar in plaats daarvan, indien nodig, aanroepen **navigate(toUrl:*{url}* useSVC:true)**.

1. de optionele callback implementeren **navigateToUrl:useSVC:** binnen de implementatie moet u een svc-instantie maken met behulp van de SFSafariViewController-instantie met behulp van de opgegeven URL en deze op het scherm weergeven:

   ```obj-c
   func navigate(toUrl url: String!, useSVC: Bool) {
       svc =  SFSafariViewController(url: URL(string: url)!)
       svc.delegate = self
       myController.present(svc, animated: true)
       }
   ```

   ***Opmerkingen:***

   - *U kunt SFSafariViewController aanpassen om het even welke manier u wilt. Op iOS 11+ kunt u bijvoorbeeld het label Gereed wijzigen in Annuleren.*
   - *om het svc te kunnen ontslaan , hebt u een verwijzing naar het nodig ; creeer het niet in het toepassingsgebied van **navigateToUrl:useSVC***
   - *gebruik uw eigen weergavecontroller voor &quot;myController&quot;*


1. In de gedelegeerde implementatie van uw toepassing van **application(\_app: UIApplication, open url: URL, opties: \[UIAapplicationOpenURLOptionsKey: Willekeurig\]) -\> Bool**, voegt u code toe om de svc te sluiten. U zou reeds één of andere code daar moeten hebben die roept **accessEnabler.handleExternalURL()**. Net onder toevoegen:

   ```obj-c
   if(svc != nil) {
       svc.dismiss(animated: true)
   }
   ```

   Opnieuw, is svc een verwijzing naar SFSafariViewController u bij stap 2 creeerde.


1. Implementeren **safariViewControllerdidFinish(\_ controller: SFSafariViewController)** van **SFSafariViewControllerDelegate** om af te vangen wanneer de gebruiker de SVC heeft geannuleerd met de knop Gereed. In deze functie, om SDK mee te delen dat de authentificatie is geannuleerd moet u roepen:

   ```obj-c
   accessEnabler.setSelectedProvider(nil)
   ```
