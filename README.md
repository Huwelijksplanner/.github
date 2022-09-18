# Huwelijksplanner
In de volgende stappen worden de benodigd API calls per stap doorgelicht

## Schermen
- [Scherm 0: Introductie](#scherm-0-introductie)
- [Scherm 1: Huwelijk of geregistreerd partnerschap?](#scherm-1-huwelijk-of-geregistreerd-partnerschap)
- [Scherm 2: datum kiezen](#scherm-2-datum-kiezen)
- [Scherm 3: overzicht keuze, door naar DigiD ](#scherm-3-overzicht-keuze-door-naar-digid)
- [Scherm: inloggen bij DigiD](#scherm-inloggen-bij-digid)
- [Scherm 4: contactgegevens invullen en gegevens controleren](#scherm-4-contactgegevens-invullen-en-gegevens-controleren)
- [Scherm 5: partner uitnodigen](#scherm-5-partner-uitnodigen)
- [Scherm: getuigen aanmelden](#scherm-getuigen-aanmelden)
- [Scherm: controles uitgevoerd](#scherm-controles-uitgevoerd)
- [Scherm: wachten op partner](#scherm-wachten-op-partner)
- [Scherm: betaling gelukt](#scherm-betaling-gelukt)
- [Scherm: getuigen aanpassen](#scherm-getuigen-aanpassen)
- [Scherm: extra bestellen](#scherm-extra-bestellen)
- [Scherm: annuleer reservering](#scherm-annuleer-reservering)
- [Scherm: bevestiging annulering](#scherm-bevestiging-annulering)

### Scherm 0: Introductie

[Bekijk prototype ](https://huwelijk.utrecht.eend.nl/docs/site/huwelijksplanner/index.html)

![img.png](img.png)

**Introductie**:
- verwachtingen
  - type trouwen dat kan
  - datum kiezen
  - locatie kiezen
- wat heb je nodig?
  - DigiD van jou en partner (niet beiden toegang met DigiD? ga naar de balie)
  - iDEAL (tenzij gratis)
  - gegevens van getuigen
  - 
**De voorwaarden worden niet expliciet vooraf meegedeeld**:
- partners zijn beiden 18 jaar (op trouwdatum)
- niet al getrouwd
- verklaring 2e graads bloedverwantschap

**Opmerking**:
- kun je ook een ambtenaar kiezen?

### Scherm 1: Huwelijk of geregistreerd partnerschap?

[Bekijk prototype ](https://huwelijk.utrecht.eend.nl/docs/site/huwelijksplanner/01-trouwen-of-partnerschap.html)

![img_1.png](img_1.png)

**Keuze verbintenis**:
- ik wil trouwen
- ik wil een geregistreerd partnerschap

**POST API**:

**Deze gegevens worden opgestuurd**:
- identifier voor keuze tussen trouwen of geregistreerd partnerschap
- Er zijn afspraken nodig over de identifier van het type.

### Scherm 2: datum kiezen

[Bekijk prototype ](https://huwelijk.utrecht.eend.nl/docs/site/huwelijksplanner/02b-trouwen-plannen-april.html)

![img_2.png](img_2.png)

![img_3.png](img_3.png)

**Rekening houden met**:
- Datum moet zijn nadat beide personen 18 zijn (persoonsgegevens zijn echter nog niet beschikbaar in deze stap)
- Datum alleen op beschikbare datums.
- Datum moet zijn na vandaag.
- Datum is maximaal uiterste datum X in toekomst.

**GET API**:
- Deze gegevens zijn nodig om de pagina te tonen.
- Request parameters:
- Gemeente (verplicht of afwezig en standaard Utrecht?)
- Type (optioneel, anders alle types)
- begindatum (optioneel, anders vanaf vandaag + minimumtijd - nu 14 dagen)
- einddatum (optioneel, anders uiterste datum in toekomst - nu maximaal 1 jaar in de toekomst)


**Response**:
lijst met unieke combinaties van datum + tijdstip + type
extra velden: locatie
Afspraken over identifiers:
gemeente
type
"geregistreerd partnerschap" - "flitshuwelijk" - "gratis huwelijk" - "eenvoudig
huwelijk" - "uitgebreid huwelijk"

**POST API**
Deze gegevens worden vanaf deze stap naar de server verstuurd.
- datum
- tijdstip
- type

### Scherm 3: overzicht keuze, door naar DigiD

[Bekijk prototype ](https://huwelijk.utrecht.eend.nl/docs/site/huwelijksplanner/03-inloggen-digid.html)

![img_17.png](img_17.png)

**Overzicht**:
- keuze product
- datum en tijdstip
- Betaal om melding te maken van voorgenomen huwelijk en tijd en locatie te reserveren.

**Opmerking**:
op dit punt kun je al een QR-code tonen zodat de partner ook kan inloggen, zodat je het echt "tegelijk en samen doet"

**GET API**:
Deze informatie is nodig om de pagina te tonen:
- gekozen type
- product prijs
- locatie
- gekozen tijdstip

**POST API**:

Een POST naar een URL met de intentie om te redirecten naar DigiD. Gegevens die
de server moet weten:
naar welke URLs geredirect moet worden wanneer DigiD een success is
de URL van de foutmelding pagina

Unhappy flow

DigiD is down.

### Scherm: inloggen met DigiD

[Bekijk prototype ](https://huwelijk.utrecht.eend.nl/docs/site/huwelijksplanner/03-inloggen-digid.html)

![img_4.png](img_4.png)

### Scherm: inloggen bij DigiD

[Bekijk prototype ](https://huwelijk.utrecht.eend.nl/docs/site/huwelijksplanner/03b-inloggen-digid.html)

![img_5.png](img_5.png)

### Scherm 4: contactgegevens invullen en gegevens controleren
[Bekijk prototype ](https://huwelijk.utrecht.eend.nl/docs/site/huwelijksplanner/04-melding-voorgenomen-huwelijk-anne.html)

![img_6.png](img_6.png)
![img_7.png](img_7.png)

**Deze pagina toont ter controle**:
- Overzicht persoonsgegevens
- Overzicht adresgegevens:
- Geeft de mogelijkheid voor persoon 1 voor het aanvullen van contactgegevens:
- telefoonnummer (optioneel, voor bijvoorbeeld Doven)
- e-mailadres

**GET API**:
De volgende gegevens zijn nodig om de pagina te tonen:
- Persoonsgegevens
- Burgerservicenummer
- Aanhef
- Voornamen
- Tussenvoegsel(s)
- Achternaam
- Burgelijke staat
- Geboortedatum
- Geboorteplaats
- Indicatie curateleregister
- Adresgegevens
- Straatnaam
- Huisnummer (+ toevoegingen)
- Postcode
- Woonplaats

**POST API**:
Deze pagina verstuurt het volgende naar de server:
telefoonnummer - optioneel
e-mailadres - verplicht

### Scherm 5: partner uitnodigen

[Bekijk prototype ](https://huwelijk.utrecht.eend.nl/docs/site/huwelijksplanner/05-vraag-sanne.html)

![img_8.png](img_8.png)

**Opmerkingen**:

kan misschien met QR code in plaats van link in e-mail, geen latency, geen junk-
mail risico

persoonsgegevens van partner komen uit DigiD, ipv zelf ingevuld

### Scherm: getuigen aanmelden

[Bekijk prototype ](https://huwelijk.utrecht.eend.nl/docs/site/huwelijksplanner/07b-getuigen-extra.html)

![img_16.png](img_16.png)


### Scherm: controles uitgevoerd

[Bekijk prototype ](https://huwelijk.utrecht.eend.nl/docs/site/huwelijksplanner/08-check05.html)

![img_9.png](img_9.png)

**Happy flow**: alle controles zijn geslaagd
**Unhappy flow**
één of meerdere checks zijn niet geslaagd

**GET API**:
Deze gegevens zijn nodig om de pagina te tonen:
- lijst van checks
- beschrijving controle
- geslaagd / niet geslaagd

**POST API**

Deze pagina verstuurt zelf geen informatie, maar de server moet wel weten naar
welke URLs geredirect moet worden na een succesvolle of mislukte betaling.

### Scherm: wachten op partner

[Bekijk prototype ](https://huwelijk.utrecht.eend.nl/docs/site/huwelijksplanner/06-wacht-op-sanne.html)

![img_10.png](img_10.png)

**Unhappy flow**
- Reservering verlopen.

**GET API**
- naam partner
- e-mailadres partner
- tijdstip verlopen reservering
- status reservering: verlopen / niet verlopen
- PUT API
- Gegevens om bestaande informatie aan te passen.
- naam partner
- e-mailadres partners

### Scherm: betaling gelukt

[Bekijk prototype ](https://huwelijk.utrecht.eend.nl/docs/site/huwelijksplanner/10-betaling-geslaagd.html)

![img_11.png](img_11.png)

**GET API**

Voor feedback:
status betaling: gelukt / niet gelukt
Voor overzicht gegevens:

E-mail: bevestiging melding van huwelijk

Mail naar beide partners.
Benodigde gegevens voor e-mail template:
naam 1
naam 2
datum en tijdstip
locatie


type
laatste datum getuigen melden
laatste datum extra's bestellen
URL om reservering te bekijken

### Scherm: getuigen aanpassen

[Bekijk prototype ](https://huwelijk.utrecht.eend.nl/docs/site/huwelijksplanner/11-getuigen-wijzig.html)

![img_14.png](img_14.png)

### Scherm: extra bestellen

[Bekijk prototype ](https://huwelijk.utrecht.eend.nl/docs/site/huwelijksplanner/11b-extra.html)

![img_15.png](img_15.png)!

Bijvoorbeeld, een huwelijksboekje bestellen en gelijk betalen.
Opmerking: is dit nog achteraf te wijzigen? Betaal je dan het bedrag voor het
nieuwe product, en wordt buiten de planner om het reeds betaalde bedrag
teruggestort? Of moet je dan kunnen bijbetalen? Dan moet de server ook weten
hoeveel reeds betaald is.

**GET API**:
Lijst extra producten, met varianten:
product naam (bijv. trouwboekje)

product ID
mogelijke varianten (rood leder / zwart leder / blauw textiel / naturel karton):
variant titel
variant prijs
variant ID

**POST API**:
één of meerdere product ID + mogelijk variant ID
Unhappy flow

Storing in betaalservice.

### Scherm: annuleer reservering

**Opmerking**: de knop "Annuleer reservering" is een beetje onfortuinlijk, omdat "Annuleer" ook vaak betekent "Ik wil die schermpje niet, doe niets". Misschien duidelijker om "Annuleren" (secondary button) en "Geen huwelijk reserveren" (primary button) te doen, of iets dergelijks.

> De vraag is ook even wat annuleren doet qua busnes logica, voor nu gaan we uit van
> - Verwijderen boeking in de calender voor locatie en trouwambtenaar
> - Mails versturen (zie Scherm: bevestiging annulering)
> - Status huwelijk omzetten naar geanuleerd
>
> **Overige vragen**
> - Zijn er er casusen waarin een annulering niet mag worden verwerkt? (los van huwelijk is reeds voltrokken)
> - Wie mag dit doen? bijde partners? inititatiefnemer?
> - Hoe gaan we de financiele afdeling op de hoogte brengen?

```json voor 
PUT {environment}/api/huwelijk/{id}

{
  "status": "cancelled"
}

RESPONCE

{
  ... het volledige huwelijks object
}
```

**Acceptatie critteria  Backend**
- [ ] Bij het proberen te setten van de status "cancelled" controleerd de backend of is voldaan aan voorwaarden en rechten
- [ ] Na het zetten van de status worden de genoemde mails verstuurd
- [ ] Na het zetten van de status worden genoemde agenda reserveringen verwijderd

### Scherm: bevestiging annulering

[Bekijk prototype ](https://huwelijk.utrecht.eend.nl/docs/site/huwelijksplanner/12-huwelijk-geannuleerd.html)

![img_12.png](img_12.png)

E-mail: bevestiging annulering

- e-mail aan partners
- andere e-mail aan getuigen
- andere e-mail aan trouwambtenaar <- Toevoeging RLI
- andere e-mail aan locatiemanager <- Toevoeging RLI
- andere e-mail aan afdeling burgerzaken <- Toevoeging RLI
- Details nader te bepalen.

> Wie leverdt de email templates aan?

**Acceptatie critteria  Backend**
- [ ] Verstuurde mails voldoen aan gestelde templates

## Techniek

De pagina moet voldoen aan DigiD richtlijnen rondom een strikte Content-Security-Policy . We willen kiezen om een server-side rendering te doen met React, zodat we een nonce kunnen genereren voor elke pagina.


