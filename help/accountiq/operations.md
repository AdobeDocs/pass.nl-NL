---
title: Activiteiten in rekening-IQ
description: De verrichtingen in Rekening IQ impliceren acties om automatiserings en bulkverrichtingen op abonneerekeningen uit te voeren en hun gevolgen te volgen.
exl-id: ba6bceca-221c-42db-b207-804e4b9f6d54
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 0%

---

# Bewerkingen {#operations-tab-next-steps}

Als u de gebruikspatronen van uw abonnees hebt begrepen en het delen van wachtwoorden voor een geselecteerd segment hebt bepaald (met [!DNL Analytics] in Account IQ) kunt u gerichte acties ondernemen om het delen van wachtwoorden te beperken.

De functionaliteit van Verrichtingen in IQ van de Rekening helpt u effectief om geloofsbrieven te pakken en te beheren die door gerichte procedures worden genoemd verrichtingen. Het geeft u opties om een objectieve, op maat gesneden acties (op het doel gebaseerd) voor een specifieke groepen abonneerekeningen te ontwerpen, en hun uitvoering voor een toekomstige duur te automatiseren. Via de functionaliteit Bewerkingen kunt u niet alleen bewerkingen maken en uitvoeren, maar ook de effecten ervan meten. Zo, door de gevolgen in kaart te brengen kunt u uw strategie aanpassen om het effect te optimaliseren, of het converteren van leners of het verlichten van credentiedelen.

Naar weergave **Bewerkingen** pagina selecteren **Bewerkingen** optie onder **Handelingen** in linkernavigatie van de toepassing van IQ van de Rekening. De pagina van Verrichtingen maakt een lijst van alle verrichtingen die reeds op het systeem van IQ van de Rekening samen met hun details bestaan.

![](assets/operations-page.png)

*Afbeelding: Lijst en details van bestaande bewerkingen in Account IQ*

Op de pagina van Verrichtingen, kunt u:

* Een lijst weergeven met bewerkingen die al in Account IQ bestaan

* Bewerkingsdetails weergeven, zoals:

   * status (Gepland, Lopend, Beëindigd, Fout, of Gestopt)

   * voortgang (in percentage van voltooiing)

   * doelpubliek (segment om de bewerking uit te voeren)

   * schema (begin- en einddatum van de exploitatie)

   * aanmaakdatum en einddatum van de operatie

* [Nieuwe bewerking maken](/help/accountiq/operation-affecting-user-segment.md)

* [Bewerkingsrapporten weergeven](#operation-reports)

<!--* Search from the list of operations using Search field

* Stop an operation.

* Create a duplicate operation.

* [Configure columns of Operations details page](#configure-columns)-->

## Bewerkingsrapporten weergeven {#operation-reports}

U kunt de effecten van een bewerking analyseren door het bijbehorende rapport te bekijken. Het rapport van een bewerking weergeven:

1. Selecteer de naam van de bewerking op de hoofdpagina Bewerkingen.

   Het rapport wordt weergegeven in de vorm van een gestapelde kolomgrafiek.

   ![](assets/operation-impact-report.png)

   *Afbeelding: Operationeel rapport om de effecten van de activiteiten te bekijken*

   De X-as geeft de evaluatieperiode weer en de y-as geeft het effect van de operatie weer (in termen van het aantal rekeningen in een segment tijdens de evaluatieperiode). Elke balk bestaat uit drie delen.

   * Een deel geeft het aantal rekeningen weer die nog steeds aan de criteria van het exploitatiesegment voldoen.

   * Een ander deel vertegenwoordigt het aantal actieve rekeningen voor die periode die oorspronkelijk in het segment waren, maar niet langer aan de criteria van het exploitatiesegment voldoen.

   * Het derde deel vertegenwoordigt de rekeningen die in die periode niet actief waren.

   >[!NOTE]
   >
   >De eerste balk geeft het aantal rekeningen weer dat aan het begin van de evaluatieperiode aan de voorwaarden van het exploitatiesegment voldoet.

   In de loop der tijd geeft de grafiek het effect van de handeling weer (door de bewerking) door het aantal accounts aan te geven dat hun gedrag ten opzichte van de oorspronkelijke criteria heeft gewijzigd (bijvoorbeeld met een gedeelde waarschijnlijkheid van meer dan 90 en meer dan 5 apparaten), of dat inactief is geworden.

<!--For example, in the above image the variable on the y-axis is number of accounts. Looking at the graph you can compare the number of accounts that are in the operations' segment versus the number of accounts that are outside the operations segment at a particular time (such as week 2nd of the operations evaluation period). Therefore, you can analyze how over the evaluation period do number of accounts vary within the operation segment and outside the segment.

So, if your operation was to send out warning emails to suspecting accounts, and accounts in operations segment were those with sharing probability more than 90 and using more than 5 devices to stream content, then in the beginning of the evaluation period accounts in segment are more than 17 thousand. This number changes over the evaluation period as shown in the graph, thereby indicating the impact of operation. Based on the evaluation, you can take remedial measures on suspecting accounts, or continue with the operation, or adjust your strategy for better outcomes to curb credential sharing.-->

1. Om het rapport te sluiten en terug naar de belangrijkste pagina van Verrichtingen te gaan, selecteer **Bewerkingen** optie onder **Handelingen** in linkernavigatie.

<!--

![](assets/operations-details.png)

*Figure: Operation details*
## Configure columns {#configure-columns}

You can select the icon to **Configure columns** on the top of the operations table.

![](assets/config-columns.png)

*Figure: Configure columns of Operations details page*-->