---
title: De token-verificator voor media integreren
description: De token-verificator voor media integreren
exl-id: 1688889a-2e30-4d66-96ff-1ddf4b287f68
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '887'
ht-degree: 0%

---

# De token-verificator voor media integreren

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Informatie over de token-verificator voor media {#about-media-token-verifier}

Wanneer de vergunning slaagt, leidt de Authentificatie van Adobe Pass tot een teken van de vergunning (AuthZ) voor lange tijd.  Het token AuthZ wordt doorgegeven aan de client-kant of opgeslagen op de server, afhankelijk van het platform van de client.  (Zie [ Begrijpend Tokens ](/help/authentication/kickstart/programmer-overview.md#understanding-tokens) voor hoe de tekenen op verschillende cliëntsystemen, samen met andere details worden opgeslagen.)


Een teken AuthZ machtigt de gebruiker van de plaats om een bepaalde middel te bekijken.  Het heeft typisch tijd-aan-levende (TTL) van 6 tot 24 uren, waarna het teken verloopt. **voor daadwerkelijke het bekijken toegang, gebruikt de Authentificatie van Adobe Pass het teken AuthZ om een kort-levend media teken te produceren dat u verkrijgt en tot uw media server** overgaat. Deze kortstondige mediatokens hebben een zeer korte TTL (doorgaans een paar minuten).


In Integraties AccessEnabler, verkrijgt u het korte-levende media teken via `setToken()` callback. Voor clientloze API-integratie krijgt u het kortstondige mediatoken met de API-aanroep `<SP_FQDN>/api/v1/tokens/media` . Het token is een tekenreeks die in duidelijke tekst wordt verzonden en die door de Adobe is ondertekend en die tokenbeveiliging gebruikt op basis van PKI (Public Key Infrastructure). Met deze op PKI-Gebaseerde bescherming, wordt het teken ondertekend gebruikend een asymmetrische sleutel, die aan Adobe door een certificatieautoriteit wordt uitgegeven.


Omdat er aan de clientzijde geen validatie voor het token bestaat, kan een kwaadwillende gebruiker tools gebruiken om nepaanroepen `setToken()` te injecteren. Daarom kunt u **niet** eenvoudig op het feit vertrouwen dat `setToken()` werd teweeggebracht, wanneer het overwegen van of een gebruiker al dan niet wordt gemachtigd. U moet bevestigen dat het kort-levende teken zelf wettig is. Het gereedschap voor het uitvoeren van de validatie is de Media Token Verifier Library.


>[!TIP]
>
>U moet de volledige lengte van het teruggekeerde symbolische koord tot de Symbolische Verifier van Media overgaan voor bevestiging.

## Tokens met korte levensduur valideren met de Media Token Verifier {#validate-short-livedttokens}

We raden programmeurs aan het token naar een webservice te verzenden die de Media Token Verifier Library gebruikt om het token te valideren voordat de videostream daadwerkelijk wordt gestart. Zeer korte TTL van de kortstondige media tokens wordt bepaald om voldoende lang te zijn om voor kloksynchronisatiekwesties tussen de server toe te staan die het teken en de server die het teken produceren te bevestigen, maar niet meer.



De [ Symbolische Bibliotheek van de Verificateur van Media Symbolische ](https://adobeprimetime.zendesk.com/auth/v2/login/signin?return_to=https%3A%2F%2Ftve.zendesk.com%2Fhc%2Fen-us%2Farticles%2F204963159-Media-Token-Verifier-library&amp;theme=hc&amp;locale=en-us&amp;brand_id=343429&amp;auth_origin=343429%2Cfalse%2Ctrue) {target=_blank} is beschikbaar voor de partners van de Authentificatie van Adobe Pass.



De bibliotheek Media Token Verifier bevindt zich in het Java-archief `mediatoken-verifier-VERSION.jar` . De bibliotheek definieert:

* Een API voor tokenverificatie (`ITokenVerifier` interface), met JavaDoc-documentatie
* De openbare sleutel van de Adobe die wordt gebruikt om te verifiëren dat het token inderdaad afkomstig is van Adobe
* Een verwijzingsimplementatie (`com.adobe.entitlement.test.EntitlementVerifierTest.java`) die toont hoe te om de Verifier API te gebruiken en hoe te om de openbare sleutel van de Adobe te gebruiken in de bibliotheek om zijn oorsprong te verifiëren


Het archief bevat alle afhankelijkheden en certificaatsleutelarchieven. Het standaardwachtwoord voor het inbegrepen certificaatsleutelarchief is &quot;123456&quot;.

* Voor de verificatiebibliotheek is JDK versie 1.5 of hoger vereist.
* Gebruik de JCE-provider van uw voorkeur voor het handtekeningalgoritme &quot;SHA256WithRSA&quot;.


**de Bibliotheek van de Verificateur moet de enige middelen zijn die worden gebruikt om de symbolische inhoud te analyseren. Programmeurs moeten de token niet parseren en de gegevens zelf extraheren, omdat de token-indeling niet gegarandeerd is en in de toekomst kan worden gewijzigd.** Alleen de API voor verificatie werkt naar behoren. Het rechtstreeks parseren van de tekenreeks werkt mogelijk tijdelijk, maar dit leidt in de toekomst tot problemen wanneer de opmaak kan veranderen. De API van de Verifier wint informatie van het teken terug, zoals:

* Is het token geldig (de methode `isValid()` )?
* De bron-id die aan het token (de methode `getResourceID()` ) is gekoppeld; deze kan worden vergeleken met (en moet overeenkomen) de andere parameter van de functie callback van `setToken()` . Als het niet aanpast, zou dit op frauduleus gedrag kunnen wijzen.
* De tijd het teken werd uitgegeven (`getTimeIssued()` methode).
* De TTL (`getTimeToLive()` methode).
* Een geanonimiseerde authentificatie GUID die van MVPD (`getUserSessionGUID()` wordt ontvangen methode).
* De identiteitskaart van de verdeler die de gebruiker voor authentiek verklaarde en als het geval - volmacht-MVPD is die de authentificatie voor de verdeler verstrekte.

## De API voor verificatie gebruiken {#using-verifier-api}

De klasse `ITokenVerifier` definieert methoden die u gebruikt om de tokenauthenticiteit voor een bepaalde bron te valideren. Gebruik de methoden `ITokenVerifier` om een token te analyseren dat wordt ontvangen als reactie op een `setToken()` -aanvraag.


De methode `isValid()` is het belangrijkste middel om een token te valideren. Er is één argument nodig, een resource-id. Als u een ongeldige middelidentiteitskaart overgaat, bevestigt de methode slechts de symbolische authenticiteit en geldigheidsperiode.


De methode `isValid()` retourneert een van de volgende statuswaarden:



| VALID_TOKEN | Alle validaties voltooid |
|--------------------|-----------------------------------------|
| INVALID_TOKEN_FORMAT | Token-indeling is ongeldig |
| INVALID_SIGNATURE | De tokenauthenticiteit kan niet worden gevalideerd |
| TOKEN_EXPIRED | Token-TTL is niet geldig |
| INVALID_RESOURCE_ID | Token is niet geldig voor een bepaalde resource |
| ERROR_UNKNOWN | Token is nog niet gevalideerd |

De extra methodes verlenen specifieke toegang tot middelidentiteitskaart, de uitgegeven tijd, en tijd-aan-levende voor een bepaald teken.

* Gebruik `getResourceID()` om de bron-id op te halen die aan het token is gekoppeld en deze te vergelijken met de id die door de aanvraag setToken() is geretourneerd.
* Gebruik `getTimeIssued()` om de tijd op te halen waarop het token is uitgegeven.
* Gebruik `getTimeToLive()` om de TTL op te halen.
* Gebruik `getUserSessionGUID()` om een geanonimiseerde GUID terug te winnen die door MVPD wordt geplaatst.
* Gebruik `getMvpdId()` om de id op te halen van de MVPD die de gebruiker heeft geverifieerd.
* Gebruik `getProxyMvpdId()` om identiteitskaart van de Volmacht terug te winnen MVPD die de gebruiker voor authentiek verklaarde.

## Voorbeeldcode {#sample-code}

Het archief van de Verificateur van de Token van Media bevat een verwijzingsimplementatie (`com.adobe.entitlement.test.EntitlementVerifierTest.java`) en een voorbeeld om API met de testklasse aan te halen. Dit voorbeeld (`com.adobe.entitlement.text.EntitlementVerifierTest.java`) illustreert de integratie van de symbolische controlebibliotheek in een media server.


```Java
package com.adobe.entitlement.test; 
import com.adobe.entitlement.verifier.CryptoDataHolder;
import com.adobe.entitlement.verifier.ITokenVerifier;
import com.adobe.entitlement.verifier.ITokenVerifierFactory;
import com.adobe.entitlement.verifier.SimpleTokenPKISignatureVerifierFactory;
import com.adobe.tve.crypto.SignatureVerificationCredential; 
import java.io.InputStream; 

public class EntitlementVerifierTest { 
    String mRequestorID = null;
    String mTokenToVerify = null;
    String mPathToCertificate = null;
    String mKeystoreType = null;
    String mKeystorePasswd = null;
    String mResourceID = null;

    public static void main(String[] args) { 
        if (args == null || args.length < 2 ) {
            System.out.println("Incorrect args: Usage: EntitlementVerifierTest requestorID tokenToVerify [resourceID]");
            return;
        } 
        String requestorID = args[0];
        String tokenToVerify = args[1];
        String pathToCertificate = "media_token_keystore.jks"; // the default keystore provided in the entitlement jar 
        String keystoreType = "jks";
        String keystorePasswd = "123456"; // password for the default keystore 
        if (requestorID == null || tokenToVerify == null) {
            System.out.println("One or more arguments is null");
            return;
        } 
        System.out.println("RequestorID: " + requestorID);
        System.out.println("token: " + tokenToVerify);
        System.out.println("cert: " + pathToCertificate);
        System.out.println("keystoretype: " + keystoreType);
        System.out.println("keystore passwd: " + keystorePasswd);
        String resourceID = null;
        if (args.length > 2) {
            resourceID = args[2];
        }
        System.out.println("Resource ID: " + resourceID);
        EntitlementVerifierTest verifier = new EntitlementVerifierTest(requestorID,
            tokenToVerify, pathToCertificate, keystoreType, keystorePasswd, resourceID);
        verifier.verifyToken();
    } 

    protected EntitlementVerifierTest(String inRequestorID,
                                      String inTokenToVerify,
                                      String inPathToCertificate,
                                      String inKeystoreType,
                                      String inKeystorePasswd, String inResourceID) {
        mRequestorID = inRequestorID;
        mTokenToVerify = inTokenToVerify;
        mPathToCertificate = inPathToCertificate;
        mKeystoreType = inKeystoreType;
        mKeystorePasswd = inKeystorePasswd;
        mResourceID = inResourceID;
    } 

    protected void verifyToken() {
        // It is expected that the SignatureVerificationCredential and 
        // CryptoDataHolder could be created at Init time in a web application 
        // and be reused for all token verifications. 
        CryptoDataHolder cryptoData = createCryptoDataHolder(mPathToCertificate, mKeystoreType, mKeystorePasswd);
        ITokenVerifierFactory tokenVerifierFactory = new SimpleTokenPKISignatureVerifierFactory();
        ITokenVerifier tokenVerifier = tokenVerifierFactory.getInstance(mRequestorID, mTokenToVerify, cryptoData);
        ITokenVerifier.eReturnValue status = tokenVerifier.isValid(mResourceID);
        System.out.println("Is token Valid? : " + status.toString());
        System.out.println("Token User ID: " + tokenVerifier.getUserSessionGUID());
        System.out.println("Token was generated at: " + tokenVerifier.getTimeIssued());

        System.out.println("Token Mvpd ID: " + tokenVerifier.getMvpdId());
        System.out.println("Token Proxy Mvpd ID: " + tokenVerifier.getProxyMvpdId());
    } 
    protected CryptoDataHolder createCryptoDataHolder(String pathToCertificate,
                                                      String keystoreType, String keystorePasswd) {
        SignatureVerificationCredential verificationCredential =
            readShortTokenVerificationCredential(pathToCertificate, keystoreType, keystorePasswd);
        CryptoDataHolder cryptoData = new CryptoDataHolder();
        cryptoData.setCertificateInfo(verificationCredential);
        return cryptoData;
    } 
    protected SignatureVerificationCredential readShortTokenVerificationCredential(String keystoreFile,
                                                                                   String keystoreType,
                                                                                   String keystorePasswd) {
        SignatureVerificationCredential cred = null; 
        if (keystoreFile != null){
            try {
                // load the keystore file 
                ClassLoader loader = EntitlementVerifierTest.class.getClassLoader();
                InputStream certInputStream =  loader.getResourceAsStream(keystoreFile);
                if (certInputStream != null) {
                    cred = new SignatureVerificationCredential(certInputStream, keystorePasswd, keystoreType);          
                }
            }
            catch (Exception e) {
                System.out.println("Error creating short token server credentials: " + e.getMessage());
            }
        }
        if (cred == null) {
            System.out.println("Error creating short token server credentials");
        } 
        return cred;
    } 
}
```
