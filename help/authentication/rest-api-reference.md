---
title: REST API Reference
description: Referentie van rustapi
exl-id: 67e4639e-db0b-4400-bb81-e214263e8395
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '649'
ht-degree: 4%

---

# REST API Reference {#rest-api-reference}

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

## Antwoordindelingen {#response-formats}


>[!NOTE]
>
> De API&#39;s die in deze services worden aangeboden, kunnen reacties retourneren in XML of JSON (voor API&#39;s die een reactie retourneren). Er zijn drie verschillende manieren om het antwoordformaat in het verzoek te specificeren:
>
>* Koptekst HTTP accepteren instellen op `application/xml` of `application/json`.
>* Geef in de payload van de aanvraag de parameter op `format=xml` of `format=json`.
>* Het eindpunt van de webservice met extensie aanroepen `.xml` of `.json`. Bijvoorbeeld: `/regcode.xml` of `/regcode.json`
>
>U kunt een van de bovenstaande methoden opgeven. Als u meerdere methoden opgeeft met conflicterende indelingen, kan dit leiden tot fouten of ongewenste uitvoer.

## REST API-eindpunten {#clientless-endpoints}

&lt;reggie_fqdn>:

* Productie - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Productie - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Staging - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Overzicht webservices {#web_srvs_summary}

In de onderstaande tabel staan de beschikbare webservices voor de clientless-aanpak. Klik de eindpunten van de Webdiensten voor meer informatie (steekproefverzoek en reactie, inputparameters, de methodes van HTTP, enz.)


| Sr | Eindpunt webservice | Beschrijving | <!--[Diag.  </br>Ref](http://tve.helpdocsonline.com/api-reference-v2-test#illustration)-->. | Gehost op | Geroepen door |
| --- | --- | --- | --- | --- | --- |
| 1. | [&lt;reggie_fqdn>/reggie/v1/  </br>  {requestorId}/regcode](/help/authentication/registration-code-request.md) | Retourneert willekeurig gegenereerde registratiecode en inlogpagina-URI | 2 | Adobe  </br>Reg Code-service | Slim apparaat |
| 2. | [&lt;reggie_fqdn>/reggie/v1/  </br>  {requestorId}/regcode/  </br>  {registrationCode}](/help/authentication/return-registration-record.md) | Hiermee wordt de registratie-code met de registratiecode UUID, de registratiecode en de hashcomponent-id geretourneerd | 8 | Adobe  </br>Reg Code-service | Adobe Pass-verificatie |
| 3. | [&lt;sp_fqdn>/api/v1/config/  </br>  {requestorId}](/help/authentication/provide-mvpd-list.md) | Keert lijst van gevormde MVPDs voor de aanvrager terug | 5 | Adobe  </br>Adobe Pass  </br>verificatie  </br>Service | Aanmelden  </br>Web  </br>App |
| 4. | [&lt;sp_fqdn>/api/v1/authenticate](/help/authentication/initiate-authentication.md) | Hiermee wordt het AuthN-proces gestart door de MVPD-selectiegebeurtenis op de hoogte te stellen. Creeert een verslag op authentificatiegegevensbestand, dat in overeenstemming wordt gebracht wanneer een succesvolle reactie van MVPD wordt ontvangen (Stap 13) | 7 | Adobe  </br>Adobe Pass  </br>verificatie  </br>Service | Aanmelden  </br>Web  </br>App |
| 5. | SAML Assertion Consumer | Bestaande SAML-workflow tussen Adobe Pass-verificatie en MVPD | 13 | Adobe Pass  </br>verificatie  </br>Service | Adobe Pass-verificatie |
| 6. | [&lt;sp_fqdn>/api/v1/checkauthn/  </br>  {registrationCode}](/help/authentication/check-authentication-flow-by-second-screen-web-app.md) | De Web App van de Login kan controleren of de poging login stroom succesvol was |     | Adobe Pass  </br>verificatie   </br>Service | Aanmelden   </br>Web   </br>App |
| 7. | [&lt;sp_fqdn>/api/v1/tokens/authoring](/help/authentication/retrieve-authentication-token.md) | Hiermee worden metagegevens opgehaald die betrekking hebben op AuthN-token | 15 | Adobe Pass  </br>verificatie  </br>Service | Slim apparaat |
| 8. | [&lt;reggie_fqdn>/reggie/v1/  </br>  {requestorId}/regcode/  </br>  {registrationCode}](/help/authentication/delete-registration-record.md) | Hiermee verwijdert u de reg-coderecord en geeft u de reg-code vrij voor hergebruik | 16 | Adobe  </br>Reg Code-service | Adobe Pass-verificatie |
| 9. | [&lt;sp_fqdn>/api/v1/authorize](/help/authentication/initiate-authorization.md) | Verkrijgt de reactie van de vergunning. | 17 | Adobe Pass  </br>verificatie  </br>Service | Slim apparaat |
| 10. | [&lt;sp_fqdn>/api/v1/checkauthing](/help/authentication/check-authentication-token.md) | Geeft aan of het apparaat een niet-verlopen AuthN-token heeft. |     | Adobe Pass  </br>verificatie  </br>Service | Slim apparaat |
| 11. | [&lt;sp_fqdn>/api/v1/tokens/authoring](/help/authentication/retrieve-authentication-token.md) | Retourneert het token AuthN indien gevonden. |     | Adobe Pass  </br>verificatie  </br>Service | Slim apparaat |
| 12. | [&lt;sp_fqdn>/api/v1/tokens/authz](/help/authentication/retrieve-authorization-token.md) | Retourneert de token AuthZ indien gevonden. |     | Adobe Pass  </br>verificatie  </br>Service | Slim apparaat |
| 13. | [&lt;sp_fqdn>/api/v1/tokens/media](/help/authentication/obtain-short-media-token.md) | Retourneert het token voor korte media indien gevonden - gelijk aan /api/v1/mediatoken |     | Adobe Pass  </br>verificatie  </br>Service | Slim apparaat |
| 14. | [&lt;sp_fqdn>/api/v1/mediatoken](/help/authentication/obtain-short-media-token.md) | Verkrijgt de Korte Token van Media |     | Adobe Pass  </br>verificatie  </br>Service | Slim apparaat |
| 15. | [&lt;sp_fqdn>/api/v1/preAuthze](/help/authentication/retrieve-list-of-preauthorized-resources.md) | Hiermee wordt de lijst met vooraf geautoriseerde bronnen opgehaald |     | Adobe Pass  </br>verificatie  </br>Service | Slim apparaat |
| 16. | [&lt;sp_fqdn>/api/v1/preauthorize/{code}](/help/authentication/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | Hiermee wordt de lijst met vooraf gemachtigde bronnen opgehaald |     | Adobe Pass  </br>verificatie  </br>Service | Aanmeldingswebtoepassing |
| 17. | [&lt;sp_fqdn>/api/v1/logout](/help/authentication/initiate-logout.md) | AuthN- en AuthZ-tokens verwijderen uit opslag |     | Adobe Pass  </br>verificatie   </br>Service | Slim apparaat |
| 18. | [&lt;sp_fqdn>/api/v1/tokens/usermetadata](/help/authentication/user-metadata.md) | Hiermee worden gebruikersmetagegevens opgehaald nadat de verificatiestroom is voltooid | NVT | NVT | Slim apparaat |
| 19. | [&lt;sp_fqdn>/api/v1/authenticate/freepreview](/help/authentication/free-preview-for-temp-pass-and-promotional-temp-pass.md) | Een verificatietoken maken voor Temperatuur-controle of Promotie Temperatuur-controle | NVT | Adobe Pass  </br>verificatie  </br>Service | Slim apparaat |


## REST API-beveiliging {#security}

Alle client-API&#39;s voor Adobe Pass-verificatie moeten worden aangeroepen met behulp van het HTTPS-protocol voor veilige communicatie. Bovendien zouden de meeste geroepen APIs een toegangstoken moeten bevatten die door wordt verstrekt [Dynamische clientregistratie](/help/authentication/dynamic-client-registration.md).
