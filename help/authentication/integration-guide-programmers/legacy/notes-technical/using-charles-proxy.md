---
title: Charles Proxy gebruiken
description: Charles Proxy gebruiken
exl-id: bb38543f-f6bc-4b5a-91b8-41bc51ee4c56
source-git-commit: 175755aa7463257487b29c5f4da989cf34e91bfd
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 0%

---

# (Verouderd) Charles Proxy gebruiken {#using-charles-proxy}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

>[!IMPORTANT]
>
> Zorg ervoor u over de recentste het productmededelingen van de Authentificatie van Adobe Pass en ontmantelingschronologie wordt geïnformeerd die in de [&#x200B; pagina van de Mededelingen van het Product &#x200B;](/help/authentication/product-announcements.md) wordt samengevoegd.

**Karel:** <http://charlesproxy.com>


## Download, installeer en krijg Begonnen met de Volmacht van Charles {#download-install-and-get-stared-with-charles-proxy}

- **Download** - <http://www.charlesproxy.com/download/>
- **installeer** - <http://www.charlesproxy.com/documentation/installation/>
- **Aan de slag** - <http://www.charlesproxy.com/documentation/getting-started/>


## Structuur versus tabs volgreeks {#structure-vs-sequence-tabs}

Er zijn twee verschillende manieren om het verkeer te bekijken:

1. **Structuur** - de verzoeken worden gegroepeerd door gastheer
1. **Opeenvolging** - de verzoeken zijn vermeld in de orde zij worden geroepen


## SSL en certificaten {#ssl-and-certificates}

SSL-proxy inschakelen `\[ *Proxy -\> Proxy Settings... -\> SSL* \]`

Schakel het selectievakje SSL-proxy inschakelen in en voeg alle HTTPS-locaties toe.

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/ProxySettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/SSLSettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AddHttpsLocations.PNG)
-->

- SSL Proxying - <http://www.charlesproxy.com/documentation/proxying/ssl-proxying/>
- SSL-certificaten - <http://www.charlesproxy.com/documentation/using-charles/ssl-certificates/>
- SSL Proxying van mobiele apparaten - Zie hieronder.


## Gastheren negeren/uitsluiten {#ignore-/-exclude-hosts}

Als de uitvoer te onoverzichtelijk wordt, kunt u kiezen om locaties te negeren of uit te sluiten. U kunt locaties negeren of uitsluiten door een van de volgende twee handelingen uit te voeren:

- Klik met de rechtermuisknop op de verzoeken die u wilt negeren en selecteer vervolgens Negeren
- Voeg handmatig de locaties toe die u wilt uitsluiten van `\[ *Proxy -\> Recording Settings... -\> Exclude* \]`


## DNS-steunkleuren {#dns-spoffing}

`\[ *Tools -\> DNS Spoofing...* \]`



DNS spoofing is zeer nuttig wanneer het proberen om een verzoek aan verschillende IP, vooral opnieuw te richten wanneer het werken met mobiele apparaten:

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/DNSSpoofing.PNG)
-->

<http://www.charlesproxy.com/documentation/tools/dns-spoofing/>


## Externe kaart {#map-remote}

`\[ *Tools -\> Map Remote...* \]`



Met kaart ver kunt u een &quot;inkomend&quot;verzoek aan een verschillend eindpunt opnieuw richten. Het meest gebruikte hoofdlettergebruik voor deze functie is &quot;Map&quot; `AccessEnabler.swf` naar `AccessEnablerDebug.swf:`

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemote.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemoteAdd.PNG)
-->

<http://www.charlesproxy.com/documentation/tools/map-remote/>



## Proxy omkeren {#reverse-proxy}

<http://www.charlesproxy.com/documentation/proxying/reverse-proxy/>

## Mobiel {#mobile}

### Charles gebruiken op een iOS-apparaat (iPhone / iPad) {#use-charles-on-an-ios-device-(iphone-/-ipad)}

#### SSL-verbinding van iPhone {#ssl-connection-from-iphone}

Blader naar <http://charlesproxy.com/charles.crt> op uw iOS-apparaat.  Hiermee wordt het dialoogvenster voor certificaatinstallatie gestart:

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate1\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate2\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate3.PNG)
-->

</br>

Klik op `\[ *Install*... *Install*... *Done* \]` om de installatie van het certificaat te voltooien.

<http://www.charlesproxy.com/documentation/faqs/ssl-connections-from-within-iphone-applications/>



#### Charles gebruiken vanaf een iOS-apparaat {#using-charles-from-an-ios-device}

Selecteer op uw iOS-apparaat `\[ *Settings* -\> *Wi-FI* -\> (*YOUR\_WIFI\_NETWORK)* \]` . Klik op de kleine blauwe pijl naast uw netwerk, en ga dan naar de Volmacht van HTTP en selecteer &quot;Handmatig&quot;:


</br>

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy1.png)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy2.PNG)
-->

</br>
Hier moet u IP en haven van de machine specificeren waar u Charles in werking stelt. <span style="line-height: 1.6em;"> als u nu Safari op uw apparaat van iOS opent en probeert om een Web-pagina te openen, zou u het volgende popup op de machine moeten krijgen die Charles in werking stelt:

</br>

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy3.PNG)
-->

</br>
Klik op Toestaan om toe te staan dat het apparaat Charles gebruikt om al zijn
verzoeken.

<http://www.charlesproxy.com/documentation/faqs/using-charles-from-an-iphone/>


#### iOS - Certificaten vertrouwen {#ios-trust-any-certificates}

<http://stackoverflow.com/questions/933331/how-to-use-nsurlconnection-to-connect-with-ssl-for-an-untrusted-cert>

#### iOS-verificatiefout - adobepass.ios.app is niet gevonden

<https://tve.zendesk.com/entries/22135907-ios-authentication-error-adobepass-ios-app-cannot-be-found>


## Charles gebruiken voor Android

<http://www.charlesproxy.com/documentation/configuration/browser-and-system-configuration>


Blader naar [&#x200B; de volmacht van Charles &#x200B;](http://charlesproxy.com/charles.crt) van uw apparaat van Android.
