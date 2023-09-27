---
title: Charles Proxy gebruiken
description: Charles Proxy gebruiken
exl-id: bb38543f-f6bc-4b5a-91b8-41bc51ee4c56
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 0%

---

# Charles Proxy gebruiken {#using-charles-proxy}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.


**Charles:** <http://charlesproxy.com>


## Download, installeer en krijg Begonnen met de Volmacht van Charles {#download-install-and-get-stared-with-charles-proxy}

- **Downloaden** - <http://www.charlesproxy.com/download/>
- **Installeren** - <http://www.charlesproxy.com/documentation/installation/>
- **Aan de slag** - <http://www.charlesproxy.com/documentation/getting-started/>


## Structuur versus tabs volgreeks {#structure-vs-sequence-tabs}

Er zijn twee verschillende manieren om het verkeer te bekijken:

1. **Structuur** - Verzoeken worden gegroepeerd op host
1. **Reeks** - Verzoeken worden vermeld in de volgorde waarin ze worden aangeroepen


## SSL en certificaten {#ssl-and-certificates}

SSL-proxy inschakelen `\[ *Proxy -\> Proxy Settings... -\> SSL* \]`

Schakel het selectievakje SSL-proxy inschakelen in en voeg alle HTTPS-locaties toe.


![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/ProxySettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/SSLSettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AddHttpsLocations.PNG)



- SSL-proxy - <http://www.charlesproxy.com/documentation/proxying/ssl-proxying/>
- SSL-certificaten - <http://www.charlesproxy.com/documentation/using-charles/ssl-certificates/>
- SSL Proxying van mobiele apparaten - Zie hieronder.


## Gastheren negeren/uitsluiten {#ignore-/-exclude-hosts}

Als de uitvoer te onoverzichtelijk wordt, kunt u kiezen om locaties te negeren of uit te sluiten. U kunt locaties negeren of uitsluiten door een van de volgende twee handelingen uit te voeren:

- Klik met de rechtermuisknop op de verzoeken die u wilt negeren en selecteer vervolgens Negeren
- Handmatig de locaties toevoegen waarvan u wilt uitsluiten `\[ *Proxy -\> Recording Settings... -\> Exclude* \]`


## DNS-steunkleuren {#dns-spoffing}

`\[ *Tools -\> DNS Spoofing...* \]`



DNS spoofing is zeer nuttig wanneer het proberen om een verzoek aan verschillende IP, vooral opnieuw te richten wanneer het werken met mobiele apparaten:

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/DNSSpoofing.PNG)

<http://www.charlesproxy.com/documentation/tools/dns-spoofing/>


## Externe kaart {#map-remote}

`\[ *Tools -\> Map Remote...* \]`



Met kaart ver kunt u een &quot;inkomend&quot;verzoek aan een verschillend eindpunt opnieuw richten. Het meest gebruikte hoofdlettergebruik voor deze functie is &quot;Kaart&quot; `AccessEnabler.swf` tot `AccessEnablerDebug.swf:`

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemote.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemoteAdd.PNG)

<http://www.charlesproxy.com/documentation/tools/map-remote/>



## Proxy omkeren {#reverse-proxy}

<http://www.charlesproxy.com/documentation/proxying/reverse-proxy/>

## Mobiel {#mobile}

### Charles gebruiken op een iOS-apparaat (iPhone / iPad) {#use-charles-on-an-ios-device-(iphone-/-ipad)}

#### SSL-verbinding van iPhone {#ssl-connection-from-iphone}

Bladeren naar <http://charlesproxy.com/charles.crt> op uw iOS-apparaat.  Hiermee wordt het dialoogvenster voor certificaatinstallatie gestart:

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate1\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate2\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate3.PNG)

</br>

Klikken `\[ *Install*... *Install*... *Done* \]` om de installatie van het certificaat te voltooien.

<http://www.charlesproxy.com/documentation/faqs/ssl-connections-from-within-iphone-applications/>



#### Charles gebruiken vanaf een iOS-apparaat {#using-charles-from-an-ios-device}

Selecteer op uw iOS-apparaat `\[ *Settings* -\> *Wi-FI* -\> (*YOUR\_WIFI\_NETWORK)* \]`. Klik op de kleine blauwe pijl naast uw netwerk, en ga dan naar de Volmacht van HTTP en selecteer &quot;Handmatig&quot;:


</br>

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy1.png)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy2.PNG)


</br>
Hier moet u IP en haven van de machine specificeren waar u Charles in werking stelt. <span style="line-height: 1.6em;">Als u Safari nu opent op uw iOS-apparaat en een webpagina probeert te openen, kunt u het volgende popup-programma weergeven op het apparaat waarop Charles wordt uitgevoerd:

</br>

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy3.PNG)

</br>
Klik op Toestaan om toe te staan dat het apparaat Charles gebruikt als proxy voor alle aanvragen.

<http://www.charlesproxy.com/documentation/faqs/using-charles-from-an-iphone/>


#### iOS - Certificaten vertrouwen {#ios-trust-any-certificates}

<http://stackoverflow.com/questions/933331/how-to-use-nsurlconnection-to-connect-with-ssl-for-an-untrusted-cert>

#### iOS-verificatiefout - adobepass.ios.app is niet gevonden

<https://tve.zendesk.com/entries/22135907-ios-authentication-error-adobepass-ios-app-cannot-be-found>


## Charles gebruiken voor Android

<http://www.charlesproxy.com/documentation/configuration/browser-and-system-configuration>


Bladeren naar [Charles, proxy](http://charlesproxy.com/charles.crt) van uw Android-apparaat.
