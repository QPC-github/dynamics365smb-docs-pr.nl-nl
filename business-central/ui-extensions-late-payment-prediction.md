---
title: Te late betaling voorspellen voor verkoopdocumenten
description: In dit onderwerp wordt uitgelegd hoe u ons voorspellende model gebruikt om te voorspellen of een factuur op tijd wordt betaald.
author: brentholtorf
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.search.keywords: 'customer, payment, invoice, sales, invoice, quote'
ms.search.form: '1950, 1951,'
ms.date: 12/20/2021
ms.author: bholtorf
---
# De extensie Voorspelling van te late betaling

Effectief beheer van tegoeden is belangrijk voor de algemene financiële status van een bedrijf. De extensie Voorspelling van te late betaling kan u helpen uitstaande tegoeden te reduceren en uw inningsstrategie af te stemmen door te voorspellen of verkoopfacturen op tijd worden betaald. Als bijvoorbeeld wordt voorspeld dat een betaling te laat zal zijn, kunt u besluiten de betalingsvoorwaarden of de betalingsmethode voor de klant aan te passen.

## Aan de slag

Wanneer u een geboekt verkoopdocument opent, wordt boven op de pagina een bericht weergegeven. Als u de Extensie Voorspelling van te late betaling wilt gebruiken, kunt u deze inschakelen door **Activeren** te kiezen in het bericht. U kunt ook handmatig de extensie instellen. Bijvoorbeeld als u betreurt dat u het bericht hebt gesloten.  

Als u de extensie handmatig wilt inschakelen, voert u de volgende stappen uit:

1. Kies het ![Lampje dat de functie Vertel me opent.](media/ui-search/search_small.png "Vertel me wat u wilt doen") voer **Voorspelling van te late betalingen instellen** in en kies vervolgens de gerelateerde koppeling.  
2. Vul de vereiste velden in.

> [!NOTE]
> Houd er rekening mee dat als u besluit de extensie handmatig in te schakelen, [!INCLUDE[prod_short](includes/prod_short.md)] dit niet toestaat als de kwaliteit van het model laag is. De modelkwaliteit geeft aan hoe accuraat de voorspellingen van het model waarschijnlijk zijn. Verschillende factoren kunnen de kwaliteit van een model beïnvloeden. Bijvoorbeeld dat er onvoldoende gegevens waren of dat de gegevens niet voldoende variatie bevatten. U kunt de kwaliteit van het model dat u momenteel gebruikt, bekijken op de pagina **Voorspelling van te late betalingen instellen**. U kunt ook een minimumdrempelwaarde voor de modelkwaliteit opgeven.   

## Alle betalingsvoorspellingen weergeven

Als u de extensie inschakelt, is de tegel **Voorspelling van te late betalingen instellen** beschikbaar in het rolcentrum **Bedrijfsmanager**. De tegel bevat het aantal betalingen dat voorspeld wordt te laat te zijn en u kunt de pagina **Klantenposten** openen waarin u dieper op de geboekte facturen kunt inzoomen. Er zijn drie kolommen om aandacht aan te besteden:  

* **Te late betaling**: geeft aan of de betaling van de factuur voorspeld wordt om te laat te zijn.
* **Zekerheid van voorspelling**: geeft aan hoe betrouwbaar de voorspelling is. **Hoog** betekent dat de voorspelling minimaal 90% zeker is, **Gemiddeld** ligt tussen 80 en 90% en **Laag** ligt onder 80%.
* **Zekerheidspercentage van voorspelling**: toont het werkelijke percentage achter de betrouwbaarheidsscore. Standaard wordt deze kolom niet weergegeven, maar u kunt deze toevoegen als u wilt. Zie [Uw werkruimte personaliseren](ui-personalization-user.md) voor meer informatie.

> [!TIP]
> De pagina Klantenposten bevat ook een feitenblok aan de rechterkant. Terwijl u voorspellingen bekijkt, kunnen de gegevens in de sectie **Klantdetails** handig zijn. Wanneer u de factuur in de lijst hebt gekozen, bevat het gedeelte gegevens over de klant. U kunt ook direct actie ondernemen. Als een klant bijvoorbeeld slecht betaalt, kunt u de klantenkaart vanuit het feitenblok openen en de klant blokkeren voor toekomstige verkopen.  

## Een betalingsvoorspelling weergeven voor een specifiek verkoopdocument

U kunt vooraf te late betalingen voorspellen. Op de pagina's **Verkoopoffertes**, **Verkooporders** en **Verkoopfacturen** kunt u de actie **Betaling voorspellen** gebruiken om een voorspelling te genereren voor het verkoopdocument dat u bekijkt.

<!--## Scheduling Payment Predictions
On the **Late Payment Prediction Setup** page you can schedule updates to payment predictions for a time that is convenient for you. -->

## Ontwerpdetails

Microsoft implementeert en gebruikt een aantal voorspellende webservices in alle regio's waar [!INCLUDE[prod_short](includes/prod_short.md)] beschikbaar is. Toegang tot deze webservices is inbegrepen in uw [!INCLUDE[prod_short](includes/prod_short.md)]-abonnement. Zie de Microsoft Dynamics 365 Business Central Licentiehandleiding voor meer informatie. De gids kan worden gedownload op de [Business Central](https://dynamics.microsoft.com/business-central/overview/)-website.

De webservices werken in drie modi:

* Train het model. De webservice traint het model op basis van de geleverde dataset.
* Evalueer het model. De webservice controleert of het model betrouwbare gegevens voor de geleverde dataset retourneert.
* Voorspel. Webservice past het model toe op de geleverde gegevensset om een voorspelling te doen.

Deze webservices zijn staatloos, wat betekent dat ze gegevens alleen gebruiken om voorspellingen op aanvraag te berekenen. Ze slaan geen gegevens op. 

> [!NOTE]  
> U kunt ook uw eigen voorspellende webservice gebruiken in plaats van de onze. Zie [Uw eigen voorspellende webservice voor voorspellingen van te late betalingen maken en gebruiken](#AnchorText).

### Gegevens die nodig zijn om het model te trainen en te evalueren

Voor elke **klantenpost** die een gerelateerde **geboekte verkoopfactuur** heeft:

* Bedrag (LV) inclusief btw
* Betalingsvoorwaarden in dagen worden berekend als **Vervaldatum** min **Boekingsdatum**.
* Of er een toegepaste creditnota is. 

Bovendien is de record verrijkt met geaggregeerde gegevens van andere facturen die betrekking hebben op dezelfde klant. Dit omvat het volgende:

- Totaal aantal en bedrag van betaalde facturen
- Totaal aantal en bedrag van facturen die te laat zijn betaald
- Totaal aantal en bedrag van openstaande facturen
- Totaal aantal en bedrag van openstaande facturen die al te laat zijn
- Gemiddeld aantal dagen te laat
- Verhouding aantal te laat betaalde/betaalde facturen
- Verhouding bedrag te laat betaalde/betaalde facturen
- Verhouding aantal openstaande late/openstaande facturen
- Verhouding bedrag te late openstaande/openstaande facturen

> [!NOTE]
> De informatie over de klant is niet opgenomen in de dataset.

### Standaardmodel en Mijn model

De extensie Voorspelling van te late betaling gebruikt een voorspellend model dat wordt getraind met gegevens die representatief zijn voor allerlei kleine tot middelgrote bedrijven. Wanneer u facturen begint te boeken en betalingen begint te ontvangen, beoordeelt [!INCLUDE[prod_short](includes/prod_short.md)] of het standaardmodel past bij uw bedrijfsstroom. 

Als blijkt dat uw processen niet overeenkomen met het standaardmodel, kunt u de extensie nog wel gebruiken, maar heeft u wel meer gegevens nodig. Blijf [!INCLUDE[prod_short](includes/prod_short.md)] gewoon gebruiken.
> [!NOTE]
> Wij gebruiken elke week een beetje van uw computertijd wanneer we het model evalueren en opnieuw trainen. 

[!INCLUDE[prod_short](includes/prod_short.md)] voert training en evaluatie automatisch uit wanneer er voldoende betaalde en te late facturen beschikbaar zijn. U kunt het echter handmatig uitvoeren wanneer u maar wilt.

#### Uw model trainen en gebruiken

1. Kies het ![Lampje dat de functie Vertel me opent.](media/ui-search/search_small.png "Vertel me wat u wilt doen") voer **Voorspelling van te late betalingen instellen** in en kies vervolgens de gerelateerde koppeling.  
2. Kies in het veld **Geselecteerd model** de optie **Mijn model**.
3. Kies de actie **Mijn model maken** om het model met uw gegevens te trainen.  

## <a name="AnchorText"> </a>Uw eigen voorspellende webservice voor voorspellingen van te laten betalingen maken en gebruiken

U kunt uw eigen voorspellende webservice maken op basis van een openbaar model met de naam **Voorspellend experiment voor Dynamics 365 Business Central**. Dit voorspellend model is online beschikbaar in de Azure AI-galerie. Ga als volgt te werk om het model te gebruiken:  

1. Open een browser en ga naar de [Azure AI-galerie](https://go.microsoft.com/fwlink/?linkid=2086310).  
2. Zoek naar **Voorspellend experiment voor Dynamics 365 Business Central** en open het model in Azure Machine Learning Studio.  
3. Gebruik het Microsoft-account om u aan te melden voor een werkruimte en kopieer vervolgens het model.  
4. Voer het model uit en publiceer het als een webservice.  
5. Noteer de API-URL en de API-sleutel. U kunt deze aanmeldingsgegevens voor een cashflowinstelling gebruiken.  
6. Kies het ![Lampje dat de functie Vertel me opent.](media/ui-search/search_small.png "Vertel me wat u wilt doen"), voer **Voorspelling van te late betalingen instellen** in en kies vervolgens de gerelateerde koppeling.  
7. Kies het selectievakje **Mijn Azure-abonnement gebruiken**.
8. Voer op het sneltabblad **Aanmeldingsgegevens van mijn model** de API-URL en API-sleutel voor uw model op.  .  

## Zie gerelateerde [Microsoft-training](/training/modules/predict-late-payments-sales-documents/)

## Zie ook

[Documentatie van Azure Machine Learning Studio](/azure/machine-learning/classic/)  
[Business Central aanpassen met extensies](ui-extensions.md)  
[Welkom bij [!INCLUDE[prod_long](includes/prod_long.md)]](welcome.md)  
[Artificial Intelligence in Microsoft Dynamics 365 Business Central gebruiken](/training/paths/use-artificial-intelligence/)  

[!INCLUDE[footer-include](includes/footer-banner.md)]
