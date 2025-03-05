---
title: Media Tokens
description: Media Tokens
exl-id: 7e486d2c-e078-464d-90b1-14e2cfb4d20a
source-git-commit: a19f4fd40c9cd851a00f05f82adbabb85edd8422
workflow-type: tm+mt
source-wordcount: '682'
ht-degree: 0%

---

# Media Tokens {#media-tokens}

>[!IMPORTANT]
>
> De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Het media teken is een teken dat door de Authentificatie van Adobe Pass [ wordt geproduceerd REST API V2 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) als resultaat van een vergunningsbesluit dat wordt bedoeld om het bekijken toegang tot beschermde inhoud (middel) te verlenen.

Het mediatoken is geldig gedurende een beperkte en korte periode (standaard 7 minuten) die op het moment van uitgifte is opgegeven. Deze tijd geeft de tijdslimiet aan voordat deze moet worden geverifieerd en gebruikt door de clienttoepassing. Het mediatoken is beperkt tot eenmalig gebruik en mag nooit in de cache worden opgeslagen.

Het media-token bestaat uit een ondertekende tekenreeks op basis van PKI (Public Key Infrastructure) die in duidelijke tekst wordt verzonden. Met de op PKI-Gebaseerde bescherming, wordt het teken ondertekend gebruikend een asymmetrische sleutel die aan Adobe door een CertificatieInstantie (CA) wordt uitgegeven.

Het media teken wordt overgegaan tot de Programmer, die het dan kan bevestigen gebruikend de Symbolische Verifier van Media alvorens de videostroom te beginnen om de veiligheid van toegang voor die middel te verzekeren.

De Media Token Verifier is een bibliotheek die door de Authentificatie van Adobe Pass wordt verdeeld die de authenticiteit van een media teken verifieert.

## Verificator mediatokens {#media-token-verifier}

Adobe Pass Authentication raadt programmeurs aan het mediatoken naar hun eigen back-endservice te verzenden, waarbij de bibliotheek Media Token Verifier wordt geïntegreerd om een veilige toegang te garanderen voordat de videostream wordt gestart. De time-to-live (TTL) van het media token is ontworpen om rekening te houden met mogelijke problemen met kloksynchronisatie tussen de token-genererende server en de validerende server.

De Authentificatie van Adobe Pass adviseert sterk tegen het ontleden van het media teken en het direct halen van zijn gegevens, aangezien het symbolische formaat niet gewaarborgd is en in de toekomst kan veranderen. De bibliotheek Media Token Verifier moet het enige hulpmiddel zijn dat wordt gebruikt om de inhoud van het token te analyseren.

De bibliotheek Media Token Verifier kan van de volgende verbinding worden gedownload:

* https://tve.zendesk.com/hc/en-us/articles/204963159-Media-Token-Verifier-library

De bibliotheek van de Verificateur van de Token van Media vereist versie JDK 1.5 of hoger en steunt het gebruik van een aangewezen leverancier van de Uitbreiding van de Cryptografie van Java (JCE) voor het handtekeningsalgoritme (`SHA256WithRSA`).

De bibliotheek Media Token Verifier die wordt vertegenwoordigd door het Java-archief van `mediatoken-verifier-VERSION.jar` bevat:

* Adobe public key.
* Symbolische verificatie-API (`ITokenVerifier.java`).
* Referentie-implementatie (`com.adobe.entitlement.test.EntitlementVerifierTest.java`).
* Afhankelijkheden en certificaatsleutelarchieven.

>[!IMPORTANT]
> 
> Het standaardwachtwoord voor het inbegrepen sleutelarchief van het certificaat is `123456`.

### Methoden {#methods}

De klasse `ITokenVerifier` definieert de volgende methoden:

* De methode `isValid()` die wordt gebruikt om het media-token te valideren. Het keurt één enkel argument, het [ middelherkenningsteken ](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier) goed. Als de opgegeven resource-id `null` is, valideert de methode alleen de authenticiteit en geldigheidsperiode van het media-token.

  De methode `isValid()` retourneert een van de volgende statuswaarden:

  | VALID_TOKEN | Tokvalidaties uitgevoerd |
  |----------------------|-------------------------------------------|
  | INVALID_TOKEN_FORMAT | Token-indeling is ongeldig |
  | INVALID_SIGNATURE | De tokenauthenticiteit kan niet worden gevalideerd |
  | TOKEN_EXPIRED | Token-TTL is niet geldig |
  | INVALID_RESOURCE_ID | Token is niet geldig voor een bepaalde resource |
  | ERROR_UNKNOWN | Token is nog niet gevalideerd |

* De methode `getResourceID()` die wordt gebruikt om de resource-id op te halen die aan het media-token is gekoppeld, en deze te vergelijken met de id die uit het antwoord op het machtigingsbesluit is geretourneerd.

* De methode `getTimeIssued()` die wordt gebruikt om de tijd op te halen waarop het media-token werd uitgegeven.

* De methode `getTimeToLive()` die wordt gebruikt om TTL van het media token op te halen.

* De `getUserSessionGUID()` methode die wordt gebruikt om een geanonimiseerde GUID terug te winnen die door MVPD wordt geplaatst.

* De methode `getMvpdId()` die wordt gebruikt om de id op te halen van de MVPD die de gebruiker heeft geverifieerd.

* De methode `getProxyMvpdId()` die wordt gebruikt om de id op te halen van de Proxy MVPD die de gebruiker heeft geverifieerd.

### Monster {#sample}

Het archief van de Verificateur van de Token van Media bevat een verwijzingsimplementatie (`com.adobe.entitlement.test.EntitlementVerifierTest.java`) en een voorbeeld om API met de testklasse aan te halen. Dit voorbeeld (`com.adobe.entitlement.text.EntitlementVerifierTest.java`) illustreert de integratie van de bibliotheek van de Verificateur van het token van Media in een media server.

```JAVA
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

## REST API V2 {#rest-api-v2}

Het mediatoken kan worden opgehaald met de volgende API:

* [Autorisatiebesluiten ophalen met specifieke mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Verwijs naar de **secties van de Reactie** en **Steekproeven** van bovengenoemde API om de structuur van vergunningsbesluiten en media tokens te begrijpen.

>[!IMPORTANT]
>
> De cliënttoepassing moet geen afzonderlijk eindpunt vragen om de [ media tokens ](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md) terug te winnen, aangezien zij reeds inbegrepen in de vergunningsbesluiten zijn die gebruikerstoegang toestaan.

Raadpleeg het volgende document voor meer informatie over hoe en wanneer u de bovenstaande API wilt integreren:

* [Basisvergunningsstroom uitgevoerd binnen primaire toepassing](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

>[!MORELIKETHIS]
>
> [ FAQs van de Fase van de Toestemming ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)
