---
title: Metagegevenscertificaat van gebruiker voor versleuteling
description: Metagegevenscertificaat van gebruiker voor versleuteling
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a2
source-git-commit: 8c5203aa4a897a5e119a9886afc64a1b1556ee4f
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---


# Metagegevenscertificaat van gebruiker voor versleuteling

>[!NOTE]
>
>De inhoud op deze pagina wordt alleen ter informatie verstrekt. Voor het gebruik van deze API is een huidige licentie van Adobe vereist. Ongeautoriseerd gebruik is niet toegestaan.

Voor de integratie van Adobe Pass Authentication-verificatie voor gecodeerde gebruikersmetagegevens hebt u een persoonlijke/openbare sleutelpaar nodig.

In dit document wordt een proces beschreven voor het genereren van certificaten voor openbare sleutels voor gebruik in Adobe Pass-verificatie. Het hier beschreven proces maakt gebruik van de OpenSSL toolkit.

## Analyse van certificaatgeneratieproces (#generation)

1. Download en installeer de OpenSSL-toolkit (http://www.openssl.org).

1. CSR (Certificate Signing Request) genereren:

   * Een sleutelpaar genereren.  Open een Command/terminalvenster en voer de volgende opdracht uit:

     ```bash
     openssl genrsa -des3 -out mycompany-license.key 2048
     ```

   * Genereer de CSR. Voer de volgende handelingen uit op uw opdrachtregel:

     ```bash
     openssl req -new -key mycompany-license.key -out mycompany-license.csr -batch
     ```

     U wordt gevraagd het wachtwoord voor de persoonlijke sleutel in te voeren.

   * Maak een reservekopie van uw persoonlijke sleutel en wachtwoord. Voorbeeld-CSR:

     ```
     -----BEGIN CERTIFICATE REQUEST-----
     MIIBnTCCAQYCAQAwXTELMAkGA1UEBhMCU0cxETAPBgNVBAoTCE0yQ3J5cHRvMRIw
     EAYDVQQDEwlsb2NhbGhvc3QxJzAlBgkqhkiG9w0BCQEWGGFkbWluQHNlcnZlci5l
     eGFtcGxlLmRvbTCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEAr1nYY1Qrll1r
     uB/FqlCRrr5nvupdIN+3wF7q915tvEQoc74bnu6b8IbbGRMhzdzmvQ4SzFfVEAuM
     MuTHeybPq5th7YDrTNizKKxOBnqE2KYuX9X22A1Kh49soJJFg6kPb9MUgiZBiMlv
     tb7K3CHfgw5WagWnLl8Lb+ccvKZZl+8CAwEAAaAAMA0GCSqGSIb3DQEBBAUAA4GB
     AHpoRp5YS55CZpy+wdigQEwjL/wSluvo+WjtpvP0YoBMJu4VMKeZi405R7o8oEwi
     PdlrrliKNknFmHKIaCKTLRcU59ScA6ADEIWUzqmUzP5Cs6jrSRo3NKfg1bd09D1K
     9rsQkRc9Urv9mRBIsredGnYECNeRaK5R1yzpOowninXC
     -----END CERTIFICATE REQUEST-----
     ```

1. Verzend CSR naar een Instantie van het Certificaat (CA) (bijvoorbeeld, Verisign).

1. De CA stuurt u het certificaat in .p7b-indeling (PKCS#7, Cryptographic Message Syntax Standard)

1. Implementeer het .p7b-certificaat. Converteer het PKCS#7-bestand (.p7b) naar een PKCS#12-bestand (PFX-bestand, Personal Information Exchange Syntax Standard) met behulp van uw persoonlijke sleutel en geneer het PEM-bestand (samengevoegd certificaatcontainerbestand):

   * PKCS#7-bestand converteren naar een tijdelijk PEM-bestand. Voer de volgende handelingen uit op uw opdrachtregel:

     ```
     openssl pkcs7 -in mycompany-license.p7b -inform DER -out mycompany-license-temp.pem -outform PEM -print_certs
     ```

   * Zet het tijdelijke PEM-bestand om in een PFX-bestand.  Voer de volgende handelingen uit op uw opdrachtregel:

     ```
     openssl pkcs12 -export -inkey mycompany-license.key -in mycompany-license-temp.pem -out mycompany-license.pfx -passin pass:private_key_password -passout pass:pfx_password
     ```

   * Zet het tijdelijke PEM-bestand om in een definitief PEM-bestand. Voer de volgende handelingen uit op uw opdrachtregel:

     ```
     openssl x509 -in mycompany-license-temp.pem -inform PEM -out mycompany-license.pem -outform PEM
     ```

1. Verzend het definitieve PEM-bestand naar de Adobe om het te configureren.

   * De persoon die uiteindelijk het PEM-bestand moet ontvangen, is de Adobe enablement engineer die is toegewezen aan uw integratie/validatie. Als u niet direct met die persoon werkt, kunt u weten wie om het dossier naar van uw vertegenwoordiger van de Adobe te verzenden.
   * Adobe ondersteunt zowel een primair certificaat als een back-upcertificaat. Als uw primaire certificaat op een of andere manier in gevaar komt, kunt u het intrekken en overschakelen naar het secundaire certificaat om de aanvrager-id in uw app te ondertekenen. Dit zal een vlotte overgang tussen certificaten in productie verzekeren, met minimale gevolgen voor de klant.
   * Zodra de Adobe het PEM-bestand ontvangt, zullen verificatietechnici het toevoegen aan uw configuratie op de server en de ontvangst van het bestand bevestigen.
