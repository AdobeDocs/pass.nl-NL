---
title: REST API Reference
description: Referentie van rustapi
exl-id: 67e4639e-db0b-4400-bb81-e214263e8395
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 2%

---

# REST API Reference {#rest-api-reference}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Draaimechanisme

De Adobe Pass Authentificatie REST API wordt geregeerd door a [ het Throttling mechanisme ](/help/authentication/throttling-mechanism.md).

## Antwoordindelingen {#response-formats}


>[!NOTE]
>
> De API&#39;s die in deze services worden aangeboden, kunnen reacties retourneren in XML of JSON (voor API&#39;s die een reactie retourneren). Er zijn drie verschillende manieren om het antwoordformaat in het verzoek te specificeren:
>
>* Stel HTTP Accept Header in op `application/xml` of `application/json` .
>* Geef in de payload van de aanvraag de parameter `format=xml` of `format=json` op.
>* Roep het eindpunt van de webservice met de extensie `.xml` of `.json` aan. `/regcode.xml` of `/regcode.json`
>
>U kunt een van de bovenstaande methoden opgeven. Als u meerdere methoden opgeeft met conflicterende indelingen, kan dit leiden tot fouten of ongewenste uitvoer.

## REST API-eindpunten {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Productie - [ api.auth.adobe.com ](http://api.auth.adobe.com/)
* Het opvoeren - [ api.auth-staging.adobe.com ](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Productie - [ api.auth.adobe.com ](http://api.auth.adobe.com/)
* Het opvoeren - [ api.auth-staging.adobe.com ](http://api.auth-staging.adobe.com/)

</br>


## Overzicht webservices {#web_srvs_summary}

In de onderstaande tabel staan de beschikbare webservices voor de clientless-aanpak. Klik de eindpunten van de Webdiensten voor meer informatie (steekproefverzoek en reactie, inputparameters, de methodes van HTTP, enz.)


| Sr | Eindpunt webservice | Beschrijving | <!--[Diag.  </br>Ref](http://tve.helpdocsonline.com/api-reference-v2-test#illustration)-->. | Gehost op | Geroepen door |
| --- | --- | --- | --- | --- | --- |
| 1. | [&lt;REGGIE_FQDN>/reggie/v1/ </br> {requestorId} /regcode ](/help/authentication/registration-code-request.md) | Retourneert willekeurig gegenereerde registratiecode en inlogpagina-URI | 2 | Adobe </br> Reg de Dienst van de Code | Slim apparaat |
| 2. | [&lt;REGGIE_FQDN>/reggie/v1/ </br> {requestorId} /regcode/ </br> {registrationCode} ](/help/authentication/return-registration-record.md) | Hiermee wordt de registratie-code met de registratiecode UUID, de registratiecode en de hashcomponent-id geretourneerd | 8 | Adobe </br> Reg de Dienst van de Code | Adobe Pass-verificatie |
| 3. | [&lt;SP_FQDN>/api/v1/config/ </br> {requestorId} ](/help/authentication/provide-mvpd-list.md) | Keert lijst van gevormde MVPDs voor de aanvrager terug | 5 | Adobe </br> de authentificatie </br> Dienst van Adobe Pass </br> | Aanmeldings </br> Web </br> App |
| 4. | [&lt;SP_FQDN>/api/v1/authenticate](/help/authentication/initiate-authentication.md) | Hiermee wordt het AuthN-proces gestart door de MVPD-selectiegebeurtenis op de hoogte te stellen. Creeert een verslag op authentificatiegegevensbestand, dat in overeenstemming wordt gebracht wanneer een succesvolle reactie van MVPD wordt ontvangen (Stap 13) | 7 | Adobe </br> de authentificatie </br> Dienst van Adobe Pass </br> | Aanmeldings </br> Web </br> App |
| 5. | SAML Assertion Consumer | Bestaande SAML-workflow tussen Adobe Pass-verificatie en MVPD | 13 | Adobe Pass </br> authentificatie </br> de Dienst | Adobe Pass-verificatie |
| 6. | [&lt;SP_FQDN>/api/v1/checkauthn/ </br> {registrationCode} ](/help/authentication/check-authentication-flow-by-second-screen-web-app.md) | De Web App van de Login kan controleren of de poging login stroom succesvol was |     | Adobe Pass </br> -verificatie   </br> Service | Aanmelden   </br> Web   </br> App |
| 7. | [&lt;SP_FQDN>/api/v1/tokens/authn](/help/authentication/retrieve-authentication-token.md) | Hiermee worden metagegevens opgehaald die betrekking hebben op AuthN-token | 15 | Adobe Pass </br> authentificatie </br> de Dienst | Slim apparaat |
| 8. | [&lt;REGGIE_FQDN>/reggie/v1/ </br> {requestorId} /regcode/ </br> {registrationCode} ](/help/authentication/delete-registration-record.md) | Hiermee verwijdert u de reg-coderecord en geeft u de reg-code vrij voor hergebruik | 16 | Adobe </br> Reg de Dienst van de Code | Adobe Pass-verificatie |
| 9. | [&lt;SP_FQDN>/api/v1/authorize ](/help/authentication/initiate-authorization.md) | Verkrijgt de reactie van de vergunning. | 17 | Adobe Pass </br> authentificatie </br> de Dienst | Slim apparaat |
| 10. | [&lt;SP_FQDN>/api/v1/checkauthn ](/help/authentication/check-authentication-token.md) | Geeft aan of het apparaat een niet-verlopen AuthN-token heeft. |     | Adobe Pass </br> authentificatie </br> de Dienst | Slim apparaat |
| 11. | [&lt;SP_FQDN>/api/v1/tokens/authn](/help/authentication/retrieve-authentication-token.md) | Retourneert het token AuthN indien gevonden. |     | Adobe Pass </br> authentificatie </br> de Dienst | Slim apparaat |
| 12. | [&lt;SP_FQDN>/api/v1/tokens/authz](/help/authentication/retrieve-authorization-token.md) | Retourneert de token AuthZ indien gevonden. |     | Adobe Pass </br> authentificatie </br> de Dienst | Slim apparaat |
| 13. | [&lt;SP_FQDN>/api/v1/tokens/media](/help/authentication/obtain-short-media-token.md) | Retourneert het token voor korte media indien gevonden - gelijk aan /api/v1/mediatoken |     | Adobe Pass </br> authentificatie </br> de Dienst | Slim apparaat |
| 14. | [&lt;SP_FQDN>/api/v1/mediatoken ](/help/authentication/obtain-short-media-token.md) | Verkrijgt de Korte Token van Media |     | Adobe Pass </br> authentificatie </br> de Dienst | Slim apparaat |
| 15. | [&lt;SP_FQDN>/api/v1/preauthorize ](/help/authentication/retrieve-list-of-preauthorized-resources.md) | Hiermee wordt de lijst met vooraf geautoriseerde bronnen opgehaald |     | Adobe Pass </br> authentificatie </br> de Dienst | Slim apparaat |
| 16. | [ &lt;SP_FQDN>/api/v1/preauthorize/{code} ](/help/authentication/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | Hiermee wordt de lijst met vooraf gemachtigde bronnen opgehaald |     | Adobe Pass </br> authentificatie </br> de Dienst | Aanmeldingswebtoepassing |
| 17. | [ &lt;SP_FQDN>/api/v1/logout ](/help/authentication/initiate-logout.md) | AuthN- en AuthZ-tokens verwijderen uit opslag |     | Adobe Pass </br> -verificatie   </br> Service | Slim apparaat |
| 18. | [&lt;SP_FQDN>/api/v1/tokens/usermetadata ](/help/authentication/user-metadata.md) | Hiermee worden gebruikersmetagegevens opgehaald nadat de verificatiestroom is voltooid | NVT | NVT | Slim apparaat |
| 19. | [&lt;SP_FQDN>/api/v1/authenticate/freepreview](/help/authentication/free-preview-for-temp-pass-and-promotional-temp-pass.md) | Een verificatietoken maken voor Temperatuur-controle of Promotie Temperatuur-controle | NVT | Adobe Pass </br> authentificatie </br> de Dienst | Slim apparaat |


## REST API-beveiliging {#security}

Alle REST-API&#39;s voor Adobe Pass-verificatie moeten worden aangeroepen met behulp van het HTTPS-protocol voor veilige communicatie. Bovendien zouden de meeste geroepen APIs een toegangstoken moeten bevatten zoals die in [ wordt beschreven terugwinnen toegangstoken ](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) API documentatie.
