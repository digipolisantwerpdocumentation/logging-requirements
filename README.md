# Logging guidelines

- [Logging guidelines](#logging-guidelines)
  * [Document historiek](#document-historiek)
  * [Context](#context)
  * [Architectuur](#architectuur)
  * [Hoe moet mijn applicatie loggen [WIP]](#hoe-moet-mijn-applicatie-loggen--wip-)
  * [Wat moet mijn applicatie loggen](#wat-moet-mijn-applicatie-loggen)
    + [Technische informatie](#technische-informatie)
    + [Functionele/Business informatie](#functionele-business-informatie)
  * [Waar moet mijn applicatie loggen](#waar-moet-mijn-applicatie-loggen)
  * [Wanneer moet mijn applicatie loggen](#wanneer-moet-mijn-applicatie-loggen)

## Document historiek

| Versie | Auteur               | Datum      | Commit  |
| ------ | -------------------- | ---------- | ------- |
| 1.0    | Quinten Scheppermans | 18/05/2020 | Release |
| -      | -                    | -          | -       |

## Context

Dit document bevat de richtlijnen voor het gebruik van de Logging Engine bij Digipolis Antwerpen.
Het focust enkel op de meest recente manier van loggen, die automatisch van toepassing is op elke containerized applicatie draaiende op [Openshift](https://www.openshift.com/) of [Platform 9](https://platform9.com/). De documentatie richt zich voornamelijk op developers, die custom applicaties in [NodeJS](https://nodejs.org/en/) en [.NET](https://dotnet.microsoft.com/) ontwikkelen. Als developer vind je er een antwoord op Architectuur je moet loggen, wat je moet loggen, waar je moet loggen en wanneer je moet loggen.

## Architectuur

<img src="images/architectuur.png" width="75%" />

Alle containers in onze **Openshift** Kubernetes clusters en via **Platform 9** opgezette Kubernetes clusters (MyACPAAS) produceren logs naar **stdout**. Deze logs worden via de default Docker JSON file logging driver opgepikt door **[Fluentbit](https://fluentbit.io/)**. Fluentbit is als DaemonSet gedeployed. Dit wil zeggen dat elke fysieke node in de cluster een kopie van de Fluentbit pod runt. In de config van Fluentbit worden de logs gefilterd, geparset en dan weggeschreven via een friendly url. De **F5** vertaalt deze url in het onderliggende ECE endpoint voor de juiste omgeving.

## Hoe moet mijn applicatie loggen [WIP]



## Wat moet mijn applicatie loggen

In zekere zin heeft elke applicatie of service de vrijheid om zelf te bepalen wat gelogd moet worden. Individuele noden maken het moeilijk een homogeen beeld te schetsen. Elke applicatie heeft een verschillend volume, verschillende privacy eisen, verschillende business eigenschappen,... Wel configureren we een aantal zaken standaard in de starter-kits, zodat correlatie tussen verschillende services eenvoudig wordt. Applicaties met afwijkende eisen kunnen de configuraties van de starter-kits dan zelf naar wens aanpassen.

### Technische informatie

Technische informatie wordt gelogd om te helpen bij het identificeren van problemen bij tracing en debugging van technische problemen. Bijvoorbeeld:

- [**Stacktraces**] Waarom produceert deze functie een NullPointerException?
- [**Availability**] Opstart of shutdown informatie van een applicatie
- [**Errors**] Een container crasht door een non-recoverable error zoals een OutOfMemoryError.
- [**Debug**] Een developer draait een applicatie in debug mode en produceert zeer fine-grained informatie over de veranderende staat van de applicatie.

Er is een veel hogere nood aan verbositeit, de structuur is moeilijker om vast te leggen en de levensduur is eerder kort. Denk in dagen, maximum weken. Het is dus aan te raden om deze informatie naar een andere index te loggen. Bepaalde zaken zijn ook niet wenselijk in productie (bijvoorbeeld debug informatie).

### Functionele/Business informatie

Functionele informatie wordt gelogd om de lopende acties/transacties/events binnen een applicatie te registreren. Ze geven een historisch hertraceerbaar beeld op de evolutie van de staat van een applicatie. Een applicatie kan bestaan uit meerdere componenten die met elkaar communiceren over het netwerk.

Functionele logs zijn nuttig bij het debuggen van functionele problemen, analyse, audit trails, customer support, security... 

- **[API-calls]** HTTP REST/SOAP API calls vertellen veel over de flow van informatie doorheen het systeem en kunnen gebruikt worden om een goed beeld te krijgen op het gebruik van applicaties (*analyse*). Bijvoorbeeld, op basis van de historiek kunnen er terugkerende periodes van hoog en lage belasting vastgesteld worden. Het geeft ook zichtbaarheid op vlak van *security*. Een hoog aantal 403s? Ook voor *auditing* biedt dit soort logs mogelijkheden. Denk aan “Customer X heeft Service Y Z-maal geraadpleegd”. Een gelogde API request/response biedt ook uitsluitsel bij discussies over communicatie tussen 2 applicaties, aangezien er exact getoond kan worden wat er binnen en buiten is gegaan (*debuggen van functionele problemen*). Customer support ten slotte spreekt voor zich. “Waarom worden mijn facturen niet getoond op de web interface?”
- **[Asynchrone Messaging Events]** Business flows verspreiden zich (mede dankzij microservices) steeds vaker over meerdere applicaties. Om broosheid van het geheel te vermijden (wat als 1 schakel in de ketting ontbreekt?) wordt er vaak gekozen om deze flows asynchroon te maken. Vooral in gechoreografeerde maar ook in georchestreerde flows leidt dit tot zeer slechte observeerbaarheid van het geheel. Hoe weet de initiator van de flow of het geheel succesvol is afgerond? Logging is hierbij van enorm belang.
- **[Database queries]** Hoewel een database query vaak verscholen is achter een API CRUD call of getriggerd wordt door een event kan loggen toch helpen bij het onderzoeken van meer low-level problemen. Zoals de vertaling van abstracte interface naar SQL statement.
- **[Expliciete logging]** Denk aan het applicatief triggeren van logs. Batch verwerking, zware functies,...

## Waar moet mijn applicatie loggen

Applicaties loggen naar **stdout**. Hier eindigt de verantwoordelijkheid van de applicatie en start de verantwoordelijkheid van de logging engine.

## Wanneer moet mijn applicatie loggen

De strategie voor wanneer een applicatie logs moet produceren is eenvoudig: **zo snel mogelijk**. Concreet betekent dit dat een gebeurtenis in het systeem gelogd moet worden zodra ze geïnitieerd is. Er hoeft niet eerst gewacht te worden tot de gebeurtenis voltrokken is. In een sterk gedistribueerd systeem is het perfect mogelijk dat één gebeurtenis, bijvoorbeeld een API call, een hele keten aan nieuwe gebeurtenissen ontketent voor een antwoord geformuleerd kan worden. Een **fictief** voorbeeld ter illustratie:

<img src="images/transactie.png" alt="transactie" width="75%" />

Een NodeJS service publisht een event op een Kafka topic [1]. De service **logt** dit naar stdout geformatteerd in JSON inclusief [correlation](https://github.com/digipolisantwerpdocumentation/api-design-and-patterns/blob/master/patterns/correlation.md), timestap en andere metadata. Een 2e NodeJS service, gesubscribed op hetzelfde topic consumeert het event [2] en **logt** dit onmiddelijk. Achterliggend triggert dit nieuwe acties. De 2e NodeJS service kopieert het correlation object en stuurt dit mee in een synchrone REST API call [3]. Stilzwijgend wordt dit object ook in alle hieropvolgende stappen do Het versturen van de call wordt **gelogd**. Als vorm van erkenning dat het request correct ontvangen zonder vervorming of blockage (bijvoorbeeld door netwerk, ESB, gateway, loadbalancer,...) **logt** de .NET core service het binnenkomende request. De .NET core service **logt** vervolgens het uitvoeren van een SQL statement op een PostgresQL database [4] en **logt** ook het resultaat van de query [5]. De response [6] wordt **gelogd** bij het versturen vanuit de .NET core service en wordt ook **gelogd** bij het ontvangen door de NodeJS service. De NodeJS service publisht een event op een topic verschillend van het eerste [7], deze actie wordt **gelogd**. De NodeJS service waarbij de transactie begon ten slotte, consumeert het event [8] en **logt** een laatste keer.

Het resultaat voor deze transactie is totale observeerbaarheid in de vorm van ***10*** logs, die een makkelijk herafspeelbare historiek van gebeurtenissen in het systeem weergeven.

