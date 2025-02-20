---
layout: traffictype
permalink: /rautatieliikenne/
section: Tietolähteet
traffictypes: Rautatieliikenne
searchable: true
swagger-source: null
hero-image: rail
lang: fi
ref: railway-traffic
title: Rautatieliikenne
intro: Junien aikataulut, toteumatiedot, sijainnit ja kokoonpanot.
links:
  - ["Väylävirasto", "https://vayla.fi"]
  - ["Fintraffic", "https://fintraffic.fi"]
  - ["Swagger", "https://rata.digitraffic.fi/swagger/"]
---

Tämän avoimen rajapinnan tarkoituksena on jakaa tietoa Suomen rataverkolla kulkevien junien aikatauluista, sijainneista, kokoonpanoista sekä täsmällisyystiedoista. Palvelun omistaa Fintraffic ja tietolähteenä toimii Fintrafficin ratakapasiteetin ja liikenteenohjauksen Liike-perheen sovellukset sekä matkustajainformaatiojärjestelmä MIKU.

![LIIKE]({{ site.baseurl }}{{ "/img/rata/liike.png" }}) ![REAALI]({{ site.baseurl }}{{ "/img/rata/reaali.png" }}) ![LOKI]({{ site.baseurl }}{{ "/img/rata/loki.png" }})

Rajapinnasta saatavien tietojen avulla on mahdollista vastata esimerkiksi seuraaviin kysymyksiin:

* Onko junani aikataulussa?
* Missä junani sijaitsee tällä hetkellä?
* Millä junalla voin matkustaa paikasta A paikkaan B ajanhetkenä C?
* Mitkä junat lähtevät ja saapuvat asemalta seuraavaksi?
* Mistä vaunuista junani koostuu?
* Mitä palveluita vaunut tarjoavat?
* Oliko juna aikataulussa esimerkiksi kaksi kuukautta sitten?

Saatavilla on [GraphQL-rajapinta](https://www.digitraffic.fi/rautatieliikenne/#graphql) sekä [Swagger-dokumentaatio](https://rata.digitraffic.fi/swagger/) 

Rajapinnasta saatavien tietojen käyttölupa on [Creative Commons Nimeä 4.0](#avoimen-datan-käyttölupa).

Avoimeen dataan tuotavaa tietoa koostetaan eri lähteistä. Lähdejärjestelmien tiedoissa voi olla puutteita tai virheitä, tietoa ei aina saada tai tieto jää jostakin syystä välittymättä avoimeen dataan. Arkaluonteisia tietoja ei tuoda avoimeen dataan lainkaan. Näistä syistä emme suosittele aineiston käyttämistä turvallisuuskriittisissä sovelluksissa.

# Sisältö
1. [Yleistä](#yleistä)
    1. [Palvelun kehittäjäyhteisö](#palvelun-kehittäjäyhteisö)
    1. [Suunnittellut ominaisuudet](#suunnittellut-ominaisuudet)
    1. [Toteutetut ominaisuudet](#toteutetut-ominaisuudet)
    1. [HTTPS](#https)
    1. [Dataa tukevat rajapinnat / työkalut](#dataa-tukevat-rajapinnat--työkalut)
1. [REST-rajapinnat](#rest-rajapinnat)
    1. [GraphQL](#graphql) 
    1. [Junien tiedot (/trains)](#junien-tiedot-trains)
        - [Yhden junan tiedot](#yhden-junan-tiedot)
        - [Päivän junien tiedot](#päivän-junien-tiedot)
        - [Kaikkien junien tiedot](#kaikkien-junien-tiedot)
        - [GTFS](#gtfs)
        - [Vanhat junat zip-paketteina](#vanhat-junat-zip-paketteina)
        - [Junan versiohistoria](#junan-versiohistoria)
    1. [Aktiivisten junien seuranta (/live-trains)](#aktiivisten-junien-seuranta-live-trains)
        - [Liikennepaikan saapuvat ja lähtevät junat (lukumäärärajoitus)](#liikennepaikan-saapuvat-ja-lähtevät-junat-lukumäärärajoitus)
        - [Liikennepaikan saapuvat ja lähtevät junat (aikavälirajoitus)](#liikennepaikan-saapuvat-ja-lähtevät-junat-aikavälirajoitus)
        - [Reittiperusteinen haku](#reittiperusteinen-haku)
        - [Kohta lähtevien tai saapuvien junien seuranta](#kohta-lähtevien-tai-saapuvien-junien-seuranta)
    1. [Junien GPS-sijainnit (/train-locations)](#junan-gps-sijainnit-train-locations)
        - [Kaikkien junien sijainnit](#kaikkien-junien-sijainnit)
        - [Yhden junan sijainti](#yhden-junan-sijainti)
        - [Vanhat sijainnit zip-paketteina](#vanhat-sijainnit-zip-paketteina)
    1. [Tarkempi seuranta kulkutietoviestien avulla (/train-tracking)](#tarkempi-seuranta-kulkutietoviestien-avulla-train-tracking)
        - [Kaikkien junien seuranta](#kaikkien-junien-seuranta)
        - [Yhden junan seuranta](#yhden-junan-seuranta)
        - [Liikennepaikan seuranta](#liikennepaikan-seuranta)
        - [Raideosuuden seuranta](#raideosuuden-seuranta)
    1. [Kokoonpanotiedot (/compositions)](#kokoonpanotiedot-compositions)
        - [Junan kokoonpanohaku](#junan-kokoonpanohaku)
        - [Junien kokoonpanohaku](#junien-kokoonpanohaku)
        - [Kaikkien kokoonpanojen seuranta](#kaikkien-kokoonpanojen-seuranta)
        - [Vanhat kokoonpanot zip-paketteina](#vanhat-kokoonpanot-zip-paketteina)   
        - [Kokoonpanojen versiohistoria](#kokoonpanojen-versiohistoria)
    1. [Kulkutievaraukset (/routesets)](#kulkutievaraukset-routesets)
        - [Kaikkien kulkutievarauksien seuranta](#kaikkien-kulkutievarauksien-seuranta)
        - [Yhden junan kulkutievaraukset](#yhden-junan-kulkutievaraukset)
        - [Liikennepaikan kulkutievaraukset](#liikennepaikan-kulkutievaraukset)
    1. [Ratatyötiedot (/trackwork-notifications) ja liikenteen rajoitetiedot (/trafficrestriction-notifications)](#ratatyötiedot-trackwork-notifications-ja-liikenteen-rajoitetiedot-trafficrestriction-notifications)
        - [Ratatyöilmoitusten haku aikavälillä](#ratatyöilmoitusten-haku-aikavälillä)
        - [Ratatyöilmoitusten uusimpien versioiden haku aikavälillä](#ratatyöilmoitusten-uusimpien-versioiden-haku-aikavälillä)
        - [Ratatyöilmoitusten kaikkien versioiden palautus](#ratatyöilmoitusten-kaikkien-versioiden-palautus)
        - [Ratatyöilmoitusten tietyn version palautus](#ratatyöilmoitusten-tietyn-version-palautus)
        - [Liikenteen rajoite-ilmoitusten haku aikavälillä](#liikenteen-rajoite-ilmoitusten-haku-aikavälillä)
        - [Liikenteen rajoite-ilmoitusten uusimpien versioiden haku aikavälillä](#liikenteen-rajoite-ilmoitusten-uusimpien-versioiden-haku-aikavälillä)
        - [Liikenteen rajoite-ilmoituksen kaikkien versioiden palautus](#liikenteen-rajoite-ilmoituksen-kaikkien-versioiden-palautus)
        - [Liikenteen rajoite-ilmoituksen tietyn version palautus](#liikenteen-rajoite-ilmoituksen-tietyn-version-palautus)
    1. [Metatiedot (/metadata)](#metatiedot-metadata)
        - [Liikennepaikkatiedot](#liikennepaikkatiedot)
        - [Operaattoritiedot](#operaattoritiedot)
        - [Syyluokat](#syyluokat)
        - [Syykoodit](#syykoodit)
        - [Kolmannen tason syykoodit](#kolmannen-tason-syykoodit)
        - [Junalajit](#junalajit)
        - [Junatyypit](#junatyypit)
        - [Raideosuudet](#raideosuudet)
        - [Herätepisteet](#herätepisteet)
        - [Aikataulukaudet ja muutosajankohdat](#aikataulukaudet-ja-muutosajankohdat)    
1. [WebSocket (MQTT)](#websocket-mqtt)
    1. [Yleistä MQTT:stä](#yleistä-mqttstä)
    1. [Junien kuuntelu](#junien-kuuntelu)
    1. [Kokoonpanojen kuuntelu](#kokoonpanojen-kuuntelu)
    1. [GPS-sijaintien kuuntelu](#gps-sijaintien-kuuntelu)
    1. [Kulkutietoviestien kuuntelu](#kulkutietoviestien-kuuntelu)
    1. [Kulkutievarauksien kuntelu](#kulkutievarauksien-kuuntelu)
1. [Vastaustyypit](#vastaustyypit)
    1. [Junat](#junat)
    1. [Kokoonpanot](#kokoonpanot)
    1. [GPS-sijainnit](#gps-sijainnit)
    1. [Kulkutietoviestit](#kulkutietoviestit)
    1. [Liikennepaikat](#liikennepaikat)
    1. [Operaattorit](#operaattorit)
    1. [Syyluokat](#syyluokat-1)
    1. [Syykoodit](#syykoodit-1)
    1. [Kolmannen tason syykoodit](#kolmannen-tason-syykoodit-1)
    1. [Junalajit](#junalajit-1)
    1. [Junatyypit](#junatyypit-1)
    1. [Raideosuudet](#raideosuudet-1)
    1. [Herätepisteet](#herätepisteet-1)
    1. [Aikataulukaudet ja muutosajankohdat](#aikataulukaudet-ja-muutosajankohdat)
    1. [Versiohistoria](#versiohistoria)
1. [Versionumeroiden käyttö](#versionumeroiden-käyttö)
1. [Avoimen datan käyttölupa](#avoimen-datan-käyttölupa)

# Yleistä

## Palvelun kehittäjäyhteisö

Jos sinulla on kysymyksiä tietosisällöstä, kehitysehdotuksia tai tarvitset apua rajapinnan käyttöön, palvelulle on perustettu julkinen [rata.digitraffic.fi Google-ryhmä](https://groups.google.com/forum/#!forum/rata_digitraffic_fi).

## Suunnittellut ominaisuudet

Listassa ensimmäisenä oleva pyritään toteuttamaan ensimmäisenä. Ominaisuuksia saatetaan vielä hyllyttää esim. operaattorien liikesalaisuussyistä tai odottamattomien teknisten vaikeuksien takia.

Otamme mielellämme vastaan kehitysehdotuksia [rata.digitraffic.fi -keskusteluryhmässä](https://groups.google.com/forum/#!forum/rata_digitraffic_fi) 

**Suunnitteilla olevat ominaisuudet:**

* Siri-sanomat
    * Sisältävät asemakuulutusten kaltaista tietoa. Esimerkiksi "Raide 5 on remontissa Pasilassa".

## Toteutetut ominaisuudet

* 13.10.2020
    * GraphQL:n v2, jossa dataa pystyy käsittelemään monipuolisemmin
* 13.5.2020
    * Kalustoyksikkönumero lähiliikenteen sähkömoottorijunille
* 26.3.2020
    * Rajapinnat ratatyö- ja liikenteen rajoite-ilmoituksille
* 15.10.2019
    * Rajapinta, josta voi hakea junien ja kokoonpanojen versiohistorian
* 20.8.2019
    * Liikennepaikan junia mahdollista suodattaa lunalajilla (matkustajajunien erottelu tavarajunista)
* 19.8.2019
    * Sijainnilliset vastaukset saa puhtaassa GeoJSON-muodossa
* 20.5.2019
    * Kulkutievaraukset (routeset-viestit)
* 22.1.2019
    * Suojattu MQTT-yhteys
* 16.11.2018
    * Aikataulukaudet ja muutosajankohdat
* 30.10.2018
    * Kaikki paitsi metadata-tiedot saatavilla MQTT:n kautta
* 12.9.2018
    * Vanhat GPS-sijainnit zip-paketteina
* 13.8.2018
    * Junan "myöhässä"-toiminallisuus    
* 21.5.2018
    * Junan sijaintihistorian haku (/train-locations/\<departure_date>/\<train_number>)
* 12.2.2018
    * Kokoonpanojen haku versionumeron avulla
* 5.2.2018 
    * GraphQL. Tapa filtteröidä, rajoittaa ja yhdistellä vastauksia
* 1.2.2018
    * Uusi tyyli etusivulle
    * Vanhat junat ja kokoonpanot zip-paketteina
* 12.12.2017
    * Junien GPS-sijainnit
* 03.10.2017
    * Junien tiedot GTFS-muodossa
* 22.08.2017
    * Swagger-dokumentaatio
* 20.06.2017
    * MIKU-järjestelmän käsiennusteet (Etelä-Suomen junien tarkemmat ennusteet)
* 18.05.2017
    * Säännöllisen ja tulevaisuuden kiireellisen kapasiteetin jakaminen
* 30.03.2017
    * Syytietojen (eli myöhästymissyyn) kolmas taso
* 24.02.2017
    * Junan lähtövalmiusilmoitus (trainReady).
* 31.01.2017
    * Junan aikataulun hyväksymishetki (timeTableAcceptanceDate).
* 24.01.2017
    * Rajapinta, joka palauttaa kaikki muuttuneet junat (/trains?version).
* 11.01.2017
    * Julkaistu junan aikataulun ratakapasiteettihakemuksen tyyppi.
* 02.01.2017
    * Syyluokkiin ja -koodeihin viitataan id:llä.
* 25.11.2016
    * Rajoitettu aikataulurajapinnan käyttöä. Lue alhaalta rajapintakuvauksesta lisätietoa.
* 23.11.2016
    * Uusi algoritmi ennusteiden tuottamiseen
* 15.06.2016
    * Kulkutietoviestit asemapaikoittain ja raideosuuksittain
* 16.03.2016
    * Mahdollisuus kuunnella junia websocketilla
    * Herätepisteet
* 29.12.2015
    * Toteumatiedon haku aikavälirajoituksin
* 18.11.2015
    * Aikatauluttomien junien kulkutietoviestit
* 08.10.2015
    * Raideosuudet
    * Kulkutietoviestit
    * Liikennepaikkaluetteloon lisää tietoja
* 05.03.2015
    * Operaattoriluettelo junanumeroavaruuksilla
    * Liikennepaikkaluettelo
    * Rajoitetun kokokoonpanotietojen julkaiseminen.
    * Reaaliaikaisen liikennetilanteen ja toteumatiedon julkaiseminen.
    * Voimassa olevan kapasiteetin julkaiseminen.
    
## HTTPS

Rajapinta tukee sekä HTTP- että HTTPS-muotoa. Suosittelemme HTTPS:n käyttöä.

## Dataa tukevat rajapinnat / työkalut

* [https://rata.digitraffic.fi/infra-api/](https://rata.digitraffic.fi/infra-api/)
    * Rajapinta, josta löytyy Suomen rautatieverkon yksityiskohtainen kuvaus
* [https://rata.digitraffic.fi/jeti-api/](https://rata.digitraffic.fi/jeti-api/)
    * Rajapinta, josta löytyy tietoja rataverkkoon kohdistuvista huoltotöistä
* [https://rata.digitraffic.fi/history/](https://rata.digitraffic.fi/history/)
    * Työkalu, jolla voi tarkastella missä tilassa jokin juna tai kokoonpano oli tiettynä ajanhetkenä   
* [https://rata.digitraffic.fi/vuosisuunnitelmat/](https://rata.digitraffic.fi/vuosisuunnitelmat/)  
    * Työkalu, jolla voi tarkastella ratatöiden vuosisuunnitelmia   

# REST-rajapinnat

Rest-rajapintoja on kahta eri tyyyppiä: 
 1. [GraphQL](#graphql)-rajapinta
 2. Staattiset rajapinnat
 
[GraphQL](#graphql)-rajapinta tarjoaa mahdollisuuden valita miten tietoa filtteröidään, järjestetään ja mitä kenttiä otetaan mukaan vastaukseen

Staattiset rajapinnat palauttavat tiedot aina samassa formaatissa. Tietojen filtteröinti, järjestäminen ja kenttien määrittely ei ole mahdollista 

Staattiset rajapinta on jaettu kahdeksaan osaan:

* [Junien tiedot (/trains)](#junien-tiedot-trains)
* [Aktiivisten junien seuranta (/live-trains)](#aktiivisten-junien-seuranta-live-trains))
* [Junan GPS-sijainnit (/train-locations)](#junan-gps-sijainnit-train-locations)
* [Kulkutietoviestit (/train-tracking)](#tarkempi-seuranta-kulkutietoviestien-avulla-train-tracking)
* [Kokoopanotiedot (/compositions)](#kokoonpanotiedot-compositions)
* [Kulkutievaraukset (/routesets)](#kulkutievaraukset-routesets)
* [Ratatyötiedot (/trackwork-notifications) ja liikenteen rajoitetiedot (/trafficrestriction-notifications)](#ratatyötiedot-trackwork-notifications-ja-liikenteen-rajoitetiedot-trafficrestriction-notifications)
* [Metatiedot (/metadata)](#metatiedot-metadata)

Palvelussa on junien aikataulu- ja toteumatiedot noin 720 päivää taaksepäin. Tulevaisuuteen tiedot ovat saatavilla niin pitkälle kuin rataviranomainen on hyväksynyt operaattoreiden aikatauluhakemukset. Rajapinnasta saatavat aikataulut voivat muuttua aikataulujen muutosajankohdissa, joita on noin kolmen kuukauden välein. Tämä koskee erityisesti tavaraliikennettä, mutta myös henkilöliikenteeseen voi tulla muutoksia näissä ajankohdissa. Tämän vuoksi sellaisten junien aikatauluihin, joiden lähtöpäivä on seuraavan muutosajankohdan jälkeen, ei voi täydellä varmuudella luottaa.

Rajapinnan tulokset tallennetaan välimuistiin, jossa säilytysaika riippuu tehdystä kyselystä ja muodostetusta vastauksesta, esimerkiksi asematiedot pidetään välimuistissa pidempään kuin reaaliaikaiset kulkutiedot.

Käytettävä versio rajapinnasta kerrotaan osoitteessa. Esimerkiksi [https://rata.digitraffic.fi/api/v1/trains/latest/1](https://rata.digitraffic.fi/api/v1/trains/latest/1), jossa v1 on rajapinnan versiotunnus.

Kaikki aikaleimat ovat [ISO-8601](https://en.wikipedia.org/wiki/ISO_8601):n mukaisia (esimerkiksi `2018-03-28T04:35:00.000Z`). Huomaa, että aikaleimoissa aikavyökkeenä on `Z` eli ne ovat [UTC-aikaa](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), eivät Suomen aikaa.

Palvelun tilaa voi seurata osoitteessa [https://status.digitraffic.fi/](https://status.digitraffic.fi/).

## GraphQL

GraphQL voidaan käyttää vastausten rajoittamiseen, filtteröintiin, järjestämiseen ja yhdistelyyn. GraphQL:n avulla voidaan esimerkiksi rajata mukaan vain tietyt kentät tai filtteröidä vastausta käyttäen lähes mitä tahansa vastauksesta löytyvää kenttää. 

GraphQL-kyselyitä voi kokeilla ja kirjoitella GraphiQL-työkalulla osoitteessa [https://rata.digitraffic.fi/api/v2/graphql/graphiql](https://rata.digitraffic.fi/api/v2/graphql/graphiql)

Kuva schemasta löytyy osoitteesta [https://rata.digitraffic.fi/api/v2/graphql/schema.svg](https://rata.digitraffic.fi/api/v2/graphql/schema.svg). Schemasta käy ilmi mahdolliset kyselyt, niiden parametrit sekä mihin tietoihin voidaan yhdistyä. Vastausmuoto on [Trains](#junat)-tyyppinen

Kaikille kyselyille ja niihin liittyville tiedoille voi antaa
* filtterin (tai useampia) `where`-parametrilla    
* järjestyksen (tai useampia) `orderBy`-parametrilla
* kappalemäärän `skip`- ja `take`-parametrilla 

### Rajoituksia

* Kyselyssä ei saa olla sama kenttä kahdesti. Esimerkiksi kysely `train { compositions { train } }` on laiton 
* `contains`:a ei voi käyttää kahdesti samassa `where`-argumentissä. Esimerkiksi `where: {compositions: {contains: {journeySections: {contains: {maximumSpeed: {greaterThan: 50}}}}}}` on laiton

### Esimerkkejä

#### Kaikki kulussa olevat VR:n junat ja niille viimeisin sijainti, jossa nopeus on yli 30 km/h [kokeile](https://rata.digitraffic.fi/api/v2/graphql/graphiql?query=%7B%0A%20%20currentlyRunningTrains(where%3A%20%7Boperator%3A%20%7BshortCode%3A%20%7Bequals%3A%20%22vr%22%7D%7D%7D)%20%7B%0A%20%20%20%20trainNumber%0A%20%20%20%20departureDate%0A%20%20%20%20trainLocations(where%3A%20%7Bspeed%3A%20%7BgreaterThan%3A%2030%7D%7D%2C%20orderBy%3A%20%7Btimestamp%3A%20DESCENDING%7D%2C%20take%3A%201)%20%7B%0A%20%20%20%20%20%20speed%0A%20%20%20%20%20%20timestamp%0A%20%20%20%20%20%20location%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D)
```
{
  currentlyRunningTrains(where: {operator: {shortCode: {equals: "vr"}}}) {
    trainNumber
    departureDate
    trainLocations(where: {speed: {greaterThan: 30}}, orderBy: {timestamp: DESCENDING}, take: 1) {
      speed
      timestamp
      location
    }
  }
}
```

#### Kaikki junat tietyltä päivämäärältä, joiden operaattori on VR ja lähilinjaliikennetunnus ei ole Z järjestettynä laskevasti junanumeron mukaan [kokeile](https://rata.digitraffic.fi/api/v2/graphql/graphiql?query=%7B%0A%20%20trainsByDepartureDate(%0A%20%20%20%20departureDate%3A%20%222020-10-05%22%2C%20%0A%20%20%20%20where%3A%20%7Band%3A%20%5B%20%7Boperator%3A%20%7BshortCode%3A%20%7Bequals%3A%20%22vr%22%7D%7D%7D%2C%20%7BcommuterLineid%3A%20%7Bunequals%3A%20%22Z%22%7D%7D%5D%7D%2C%20%0A%20%20%20%20orderBy%3A%20%7BtrainNumber%3A%20DESCENDING%7D)%20%0A%20%20%7B%0A%20%20%20%20trainNumber%0A%20%20%20%20departureDate%0A%20%20%20%20commuterLineid%0A%20%20%20%20operator%20%7B%0A%20%20%20%20%20%20shortCode%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D)
```
{
  trainsByDepartureDate(
    departureDate: "2020-10-05", 
    where: {and: [ {operator: {shortCode: {equals: "vr"}}}, {commuterLineid: {unequals: "Z"}}]}, 
    orderBy: {trainNumber: DESCENDING}) 
  {
    trainNumber
    departureDate
    commuterLineid
    operator {
      shortCode
    }
  }
}
```

#### Kulussa olevat junat järjestetynä operaattorilla ja junanumerolla [kokeile](https://rata.digitraffic.fi/api/v2/graphql/graphiql?query=%7B%0A%20%20currentlyRunningTrains(orderBy%3A%20%5B%7Boperator%3A%7BshortCode%3AASCENDING%7D%7D%2C%7BtrainNumber%3AASCENDING%7D%5D)%20%7B%0A%20%20%20%20operator%20%7B%0A%20%20%20%20%20%20shortCode%0A%20%20%20%20%7D%0A%20%20%20%20trainNumber%0A%20%20%7D%0A%7D%0A)
```
{
  currentlyRunningTrains(orderBy: [{operator:{shortCode:ASCENDING}},{trainNumber:ASCENDING}]) {
    operator {
      shortCode
    }
    trainNumber
  }
}
```

#### Junat, jotka kulkevat Ylöjärven kautta [kokeile](https://rata.digitraffic.fi/api/v2/graphql/graphiql?query=%7B%0A%20%20trainsByDepartureDate(departureDate%3A%20%222020-10-06%22%2C%20%0A%20%20%20%20where%3A%20%7BtimeTableRows%3A%7Bcontains%3A%7Bstation%3A%7BshortCode%3A%7Bequals%3A%22YL%C3%96%22%7D%7D%7D%7D%7D%0A%20%20%20%20)%20%7B%0A%20%20%20%20trainNumber%0A%20%20%20%20departureDate%0A%20%20%20%20timeTableRows%20%7B%0A%20%20%20%20%20%20station%20%7B%0A%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20uicCode%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A)
```
{
  trainsByDepartureDate(departureDate: "2020-10-06", 
    where: {timeTableRows:{contains:{station:{shortCode:{equals:"YLÖ"}}}}}
    ) {
    trainNumber
    departureDate
    timeTableRows {
      station {
        name
        uicCode
      }
    }
  }
}
```

### GraphQL-kysely omassa sovelluksessa

GraphQL-kysely on POST-tyyppinen pyyntö osoitteeseen `https://rata.digitraffic.fi/api/v2/graphql/graphql`

HTTP-pyyntöön tulee lisätä otsikot `Content-Type: application/json` ja `Accept-Encoding: gzip`

Itse kysely on jsonia POST:n bodyssä. Esimerkiksi:
```
{"query":"{  trainsByDepartureDate(departureDate:\"2020-10-05\", where: {and: [{operator: {shortCode: {eq: \"vr\"}}}, {commuterLineid: {ne: \"Z\"}}]}, orderBy:{trainNumber:DESCENDING}) {    trainNumber\n    departureDate\n    commuterLineid\n    operator {\n      shortCode\n    }\n  }\n}","variables":null,"operationName":null}
```

Kysely kokonaisuudessaan curl:lla:
```
curl 'https://rata.digitraffic.fi/api/v2/graphql/graphql' --compressed -H 'content-type: application/json' --data-binary '{"query":"{\n  currentlyRunningTrains(where: {operator: {shortCode: {equals: \"vr\"}}}) {\n    trainNumber\n    departureDate\n    trainLocations(where: {speed: {greaterThan: 30}}, orderBy: {timestamp: DESCENDING}, take: 1) {\n      speed\n      timestamp\n      location\n    }\n  }\n}","variables":null,"operationName":null}'
```

## Junien tiedot (/trains)

Tämän rajapinnan kautta voidaan kysyä junien aikataulutietoja. Erilaisia tapoja kysyä junien tietoja ovat: junanumero, lähtöpäivämäärä, asema ja versio.

Toteumatiedoista osa perustuu liikenteenohjauksen tekemiin käsikirjauksiin, jonka vuoksi osa toteumakirjauksista tehdään tapahtumahetkeä 0-5 minuuttia myöhemmin (siis historiaan). Esimerkiksi Tampereen ja Seinäjoen liikennepaikoilla ei saada automaattisia toteumatietoja, vaan kaikki toteumat perustuvat käsikirjauksiin.

Automaattinen ennusteen laskeminen perustuu toteutuneisiin toteumiin. Historiasta voidaan laskea keskinopeus kuinka nopeasti juna on ajanut liikennepaikkavälin tietyllä kalustolla ja tätä keskinopeutta käytetään ennustenopeutena.

### Yhden junan tiedot

URL: `/trains/<departure_date>/<train_number>`

Esimerkkejä
- [/trains/latest/1](https://rata.digitraffic.fi/api/v1/trains/latest/1)
- [/trains/2017-01-01/1](https://rata.digitraffic.fi/api/v1/trains/2017-01-01/1)

**Kuvaus**

Palauttaa yhden junan tiedot

**Hakuehdot**

|&nbsp;&nbsp;&nbsp;&nbsp;| Nimi | Formaatti | Esimerkki | Selitys
| --- | --- | --- | --- | ---
| ![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }})| train_number | 1-99999 | 1, 3402 | Junan numero. Esimerkiksi junan "IC 59" junanumero on 59.
|  ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})| departure_date | date(yyyy-mm-dd) | 2017-01-01 | Junan ensimmäisen lähdön päivämäärä Suomen ajassa. Jos parametriksi annetaan "latest", pyritään päättelemään juna joka on lähinnä nykyhetkeä. Päättely tehdään siten, että haetaan kaikki junanumeron junat lähipäiviltä ja etsitään nykyhetkeä lähinnä oleva aikataulurivi (rajauksella 4 tuntia taaksepäin, 16 tuntia eteenpäin. Vertailussa käytetään aikataulurivien suunnitteltuja aikoja.
|  ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})| version | positive integer | 159123295871 | Versiorajoitus. Jos juna ei ole muuttunut sitten määritellyn version, palautetaan tyhjä tulos.
|  ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})| include_deleted | boolean | false | Palautetaanko vastauksessa myös poistetut junat. Oletuksena false
  
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen
 
 **Paluuarvo**
 
 Palauttaa [junat](#junat) -tyyppisen vastauksen.
 
### Päivän junien tiedot
 
 URL: `/trains/<departure_date>`
 
 Esimerkki: [/trains/2017-11-09](https://rata.digitraffic.fi/api/v1/trains/2017-11-09)
 
 **Kuvaus**
 
 Palauttaa kaikki junat halutulta lähtöpäivämäärältä.
 
 **Hakuehdot**
 
|&nbsp;&nbsp;&nbsp;&nbsp;| Nimi | Formaatti | Esimerkki |
| --- | --- | --- | ---
| ![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) | departure_date | date (yyyy-mm-dd) | 2017-01-01
|  ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})| include_deleted | boolean | false | Palautetaanko vastauksessa myös poistetut junat. Oletuksena false
 
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen
 
 **Paluuarvo**
 
 Palauttaa [junat](#junat)-tyyppisen vastauksen.
 
### Kaikkien junien tiedot
 
 URL: `/trains?version`
 
 Esimerkki: [/trains?version=1234567891234](https://rata.digitraffic.fi/api/v1/trains?version=1234567891234)
 
 **Kuvaus**
 
 Palauttaa kaikkien junien tiedot, jotka ovat muuttuneet sitten annetun versionumeron. Vastaus rajoitettu 2500 junaan.
 
 **Hakuehdot**
 
|&nbsp;&nbsp;&nbsp;&nbsp;| Nimi | Formaatti | Esimerkki | Selitys
| --- | --- | --- | --- | ---
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})  | version | positive integer | 159123295871 | Versiorajoitus. Palauttaa kaikki junat, jotka ovat muuttuneet sitten version-version. Jos versionumeroa ei anneta, palautetaan uusimmalla versionumerolla muuttuneet junat.
 
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen
 
 **Paluuarvo**
 
 Palauttaa [junat](#junat)-tyyppisen vastauksen.
 
### GTFS
 
 URL:t
 - [/trains/gtfs-all.zip](https://rata.digitraffic.fi/api/v1/trains/gtfs-all.zip)
 - [/trains/gtfs-passenger.zip](https://rata.digitraffic.fi/api/v1/trains/gtfs-passenger.zip)
 
 **Kuvaus**
 
Junien tiedot saa myös GTFS-muodossa (General Transit Feed Specification). Paketti gtfs-all.zip sisältää kaikki junat ja gtfs-passenger.zip sisältää vain matkustajajunat.

Paketti generoidaan uusiksi päivittäin noin klo 5:00. Se sisältää kaikki tulevaisuuden junat ja menneisyyden junat viimeiseltä seitsemältä päivältä.
 
 **Paluuarvo**
 
 Palauttaa [GTFS-muodossa](https://github.com/google/transit/blob/master/gtfs/spec/en/reference.md) olevan zip-paketin
 
### Vanhat junat zip-paketteina

Vanhojen junien tiedot löytyvät zip-paketteina osoittesta [/api/v1/trains/dumps/list.html](https://rata.digitraffic.fi/api/v1/trains/dumps/list.html)

Paketin sisältämä json on saman muotoista kuin muutkin [juna-vastaukset](#junat).

Uusi paketti luodaan joka kuun viides päivä.
 
### Junan versiohistoria
 
 URL: `/trains/history/{departure_date}/{train_number}`
 
 Esimerkki: [/trains/history/2019-10-14/1](https://rata.digitraffic.fi/api/v1/trains/history/2019-10-14/1)
 
 **Kuvaus**
 
 Palauttaa junan kaikki versiot. Historiatietoa säilytetään tallessa 14 päivää
 
**Hakuehdot**

|&nbsp;&nbsp;&nbsp;&nbsp;| Nimi | Formaatti | Esimerkki | Selitys
| --- | --- | --- | --- | ---
| ![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }})| train_number | 1-99999 | 1, 3402 | Junan numero. Esimerkiksi junan "IC 59" junanumero on 59.
| ![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }})| departure_date | date(yyyy-mm-dd) | 2017-01-01 | Junan ensimmäisen lähdön päivämäärä Suomen ajassa. 
  
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen
 
 **Paluuarvo**
 
 Palauttaa [versiohistoria](#versiohistoria)-tyyppisen vastauksen, jossa json-kenttä on muotoa [junat](#junat).
 
## Aktiivisten junien seuranta (/live-trains)
 
### Liikennepaikan saapuvat ja lähtevät junat (lukumäärärajoitus)
 
 URL: `/live-trains/station/<station_shortcode>?arrived_trains=arrived_trains>&arriving_trains=arriving_trains> &departed_trains=<departed_trains>&departing_trains=<departing_trains>&version=<change_number>`
 
 Esimerkkejä:
 - [/live-trains/station/HKI](https://rata.digitraffic.fi/api/v1/live-trains/station/HKI)
 - [/live-trains/station/HKI?arrived_trains=5&arriving_trains=5&departed_trains=5&departing_trains=5&include_nonstopping=false](https://rata.digitraffic.fi/api/v1/live-trains/station/HKI?arrived_trains=5&arriving_trains=5&departed_trains=5&departing_trains=5&include_nonstopping=false)
 - [/live-trains/station/HKI?arrived_trains=5&arriving_trains=5&departed_trains=5&departing_trains=5&include_nonstopping=false&train_categories=Commuter](https://rata.digitraffic.fi/api/v1/live-trains/station/HKI?arrived_trains=5&arriving_trains=5&departed_trains=5&departing_trains=5&include_nonstopping=false&train_categories=Commuter)
 
 **Kuvaus**
 
Palauttaa asemalla pysähtyvistä junista viimeksi lähteneet tai saapuneet, tai seuraavaksi lähtevät tai saapuvat.

Parametreillä voidaan rajoittaa palautettavien junien määrää. Junien kokonaismäärän rajoitus on 1000. Rajoitusparametrien yhteenlaskettu summa ei siis voi olla tätä suurempi.

Haku tehdään aikatauluaikojen perusteella taakse ja eteenpäin 24 tuntia. Tämä tarkoittaa, että harvaan liikennöidyllä liikennepaikkalla junien määrä saattaa olla pieni.
 
![warning]({{ site.baseurl }}{{ "/img/rata/warn.png" }}) Koska sama juna voi kuulua useampaan joukkoon (esim. saapunut juna voi olla yhtäaikaisesti myös lähtevä), palautettava kokonaismäärä on yleensä pienempi kuin parametrien summa.
 
Oletuksena haulla palautetaan vain junat, jotka pysähtyvät asemalle. Parametrin "include_nonstopping" avulla voidaan palauttaa myös junat, jotka ajavat aseman ohi pysähtymättä.

Versionumerohaulla ei palauteta junasta tietoa, mikäli junan tiedot eivät ole muuttuneet kyselyiden välillä. Tämä tarkoittaa, että tulosjoukon koko voi olla tällöin pienempi.
 
| | Nimi | Formaatti | Oletusarvo | Esimerkki | Selitys 
|---|---|---|---|---|---|
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) | station | string |   | "HKI" | Aseman lyhenne. Esimerkiksi HKL, TPE, PSL. Lista lyhenteistä löytyy [täältä](https://rata.digitraffic.fi/api/v1/metadata/stations).
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) | arrived_trains | positive integer, 1-600 | 5 | 20 | Kuinka monta saapunutta junaa palautetaan maksimissaan.
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) | arriving_trains | positive integer, 1-600 | 5 | 20 | Kuinka monta saapuvaa junaa palautetaan maksimissaan.
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) | departed_trains | positive integer, 1-600 | 5 | 20 | Kuinka monta lähtenyttä junaa palautetaan maksimissaan.
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})  | departing_trains | positive integer, 1-600 | 5 | 20 | Kuinka monta lähtevää junaa palautetaan maksimissaan.
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) | include_nonstopping | true/false | false | true | Palautetaanko aseman ohi pysähtymättä ajavat junat.
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) | train_categories | string |  | Commuter,Long-distance | Junalaji-rajaus pilkulla eroteltuna. Lista junalajeista löytyy [täältä](https://rata.digitraffic.fi/api/v1/metadata/train-categories)
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) | version | positive integer |   | 159123295871 | Versiorajaus. Palauttaa kaikki junat, jotka ovat muuttuneet sitten version-version. Jos versionumeroa ei anneta, palautetaan uusimmat tiedot.

![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen
   
**Paluuarvo**

Palauttaa [junat](#junat)-tyyppisen vastauksen.
   
### Liikennepaikan saapuvat ja lähtevät junat (aikavälirajoitus)

URL: `/live-trains/station/<station_shortcode>?minutes_before_departure=<minutes_before_departure>&minutes_after_departure=<minutes_after_departure>&minutes_before_arrival=<minutes_before_arrival>&minutes_after_arrival=<minutes_after_arrival>&version=<change_number>&includeNonstopping=<includeNonstopping`

Esimerkkejä:
- [/live-trains/station/HKI?minutes_before_departure=15&minutes_after_departure=15&minutes_before_arrival=15&minutes_after_arrival=15](https://rata.digitraffic.fi/api/v1/live-trains/station/HKI?minutes_before_departure=15&minutes_after_departure=15&minutes_before_arrival=15&minutes_after_arrival=15)
- [/live-trains/station/HKI?minutes_before_departure=15&minutes_after_departure=15&minutes_before_arrival=15&minutes_after_arrival=15&train_categories=Commuter](https://rata.digitraffic.fi/api/v1//live-trains/station/HKI?minutes_before_departure=15&minutes_after_departure=15&minutes_before_arrival=15&minutes_after_arrival=15&train_categories=Commuter)

**Kuvaus**

Palauttaa asemalla pysähtyvistä junista viimeksi lähteneet tai saapuneet, tai seuraavaksi lähtevät tai saapuvat.

Parametreillä voidaan rajoittaa lähteviä ja saapuvia junia aikamääreiden avulla.

Aikavälirajoituksen maksimikoko on 24 tuntia. Tämä tarkoittaa, että harvaan liikennöidyllä liikennepaikkalla junien määrä saattaa olla pieni.

Oletuksena haulla palautetaan vain junat, jotka pysähtyvät asemalle. Parametrin "include_nonstopping" avulla voidaan palauttaa myös junat, jotka ajavat aseman ohi pysähtymättä.

Versionumerorajoituksen avulla voidaan suodattaa pois junat, jotka eivät ole muuttuneet sitten annetun versionumeron.

**Hakuehdot**

|&nbsp;&nbsp;&nbsp;&nbsp;| Nimi | Formaatti | Oletusarvo | Esimerkki | Selitys
|---|---|---|---|---|---
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) | station | string |   | "HKI" | Aseman lyhenne. Esimerkiksi HKL, TPE, PSL. Lista lyhenteistä löytyy [täältä](https://rata.digitraffic.fi/api/v1/metadata/stations).
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) | minutes_before_departure | positive integer, 0-1440 |  | 20 | Kuinka monta minuuttia juna näytetään ennen sen lähtöä.
 ![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }})| minutes_after_departure | positive integer, 0-1440 |  | 20 | Kuinka monta minuuttia juna näytetään sen lähdön jälkeen.
 ![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }})| minutes_before_arrival | positive integer, 0-1440 |  | 20 | Kuinka monta minuuttia juna näytetään ennen sen saapumista.
 ![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }})| minutes_after_arrival | positive integer, 0-1440 |  | 20 | Kuinka monta minuuttia juna näytetään sen saapumisen jälkeen.
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) | include_nonstopping | true/false | false | true | Palautetaanko aseman ohi pysähtymättä ajavat junat.
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) | train_categories | string |  | Commuter,Long-distance | Junalaji-rajaus pilkulla eroteltuna. Lista junalajeista löytyy [täältä](https://rata.digitraffic.fi/api/v1/metadata/train-categories)
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) | version | positive integer |   | 159123295871 | Versiorajoitus. Palauttaa kaikki junat, jotka ovat muuttuneet sitten version-version. Jos versionumeroa ei anneta, palautetaan uusimmat tiedot.

![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen

**Paluuarvo**

Palauttaa [junat](#junat)-tyyppisen vastauksen.

### Reittiperusteinen haku

URL: `/live-trains/station/<departure_station_code>/<arrival_station_code>?departure_date=<departure_date>&startDate=<startDate>&endDate=<endDate>&limit=<limit>`

Esimerkki: [/live-trains/station/HKI/TPE](https://rata.digitraffic.fi/api/v1/live-trains/station/HKI/TPE)

**Kuvaus**

Palauttaa junat, jotka kulkevat departure_station_code- ja arrival_station_code-asemien kautta ja pysähtyvät asemilla.

Haku palauttaa vain suorat junayhteydet.. Hakutulos ei siis sisällä operaattorin tarjoamia reittivaihtoehtoja, joissa matkustaja joutuu esimerkiksi vaihtamaan junaa. Päivämääräväli rajattu maksimissaan kahteen päivään.

Oletuksena haulla palautetaan vain junat, jotka pysähtyvät asemallilla. Parametrin "include_nonstopping" avulla voidaan palauttaa myös junat, jotka ajavat asemien ohi pysähtymättä.

**Hakuehdot**

|&nbsp;&nbsp;&nbsp;&nbsp;| Nimi | Formaatti | Esimerkki | Selite
|---|---|---|--- |--- 
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) | departure_station | string | "HKI" | Lähtöaseman lyhenne. Lyhennekoodit löytyvät [täältä](https://rata.digitraffic.fi/api/v1/metadata/stations).
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) | arrival_station | string | "RI" | Määränpääaseman lyhenne. Lyhennekoodit löytyvät [täältä](https://rata.digitraffic.fi/api/v1/metadata/stations).
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) | departure_date | date(yyyy-mm-dd) | 2017-01-01 | Päivämäärä jolta junia haetaan. Jos lähtöpäivämäärä on tyhjä, etsitään seuraavan 24 tunnin aikana lähteviä junia.
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) | startDate | datetime(ISO 8601) | 2017-01-01T23:28:59.564Z | departure_date päivämäärän sijasta voidaan määritellä aikaväli, jolta junia haetaan. Tämä parametri määrittelee aikavälin alun. Päivämääräväliä verrataan junan aikataulun mukaisen lähtöaikaan reittihaun lähtöasemalta.
 ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})| endDate | datetime(ISO 8601) | 2017-01-01T23:28:59.564Z | Tämä parametri määrittelee aikavälin lopun. Jos tämä parametri jätetään tyhjäksi, haetaan junia seuraavalle 24 tunnille asti.
 ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})| limit | positive integer | 15 | Rajaa palautettavien junien määrää. Oletusarvo on 1000.
 ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})| include_nonstopping | true/false | false | Palautetaanko aseman ohi pysähtymättä ajavat junat.

![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen

**Paluuarvo**

Palauttaa [junat](#junat)-tyyppisen vastauksen.

### Kohta lähtevien tai saapuvien junien seuranta

URL: `/live-trains?version=<version>`

Esimerkkejä:
- [/live-trains?version=12345671234567](https://rata.digitraffic.fi/api/v1/live-trains?version=12345671234567)
- [/live-trains](https://rata.digitraffic.fi/api/v1/live-trains)

**Kuvaus**

Palauttaa kaikkien lähiaikoina kulussa olevien junien tiedot.

Kulussa oleva juna määritellään siten, että junan aikataulutapahtuman (suunniteltu, ennuste tai toteuma reitin jollain liikennepaikalla) hetkestä on kulunut alle 4 tuntia nykyhetkeen verrattuna.

**Hakuehdot**

|&nbsp;&nbsp;&nbsp;&nbsp;| Nimi | Formaatti | Esimerkki | Selitys
  |---|---|---|--- |--- 
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) | version | positive integer | 6403053026 | Versiorajoitus. Palauttaa kaikki junat, jotka ovat muuttuneet sitten version-version. Jos versionumeroa ei anneta, palautetaan uusimmat tiedot.

![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen

**Paluuarvo**

Palauttaa [junat](#junat)-tyyppisen vastauksen.

## Junan GPS-sijainnit (/train-locations)

Tämän rajapinnan kautta voidaan lukea junan GPS:n tuottamaa sijaintitietoa. Kaikissa junissa ei ole GPS:ää käytössä. Jos näiden junien tarkka sijainti kiinnostaa, voidaan seuraamiseen käyttää käyttää esimerkiksi kulkutietoviestejä.

Junan GPS-laite toimittaa sijainnin tällä hetkellä 15 sekunnin välein. Sijainnit saapuvat kuitenkin eri aikoihin, joten kyselyiden cache-aika on yksi sekunti.

GPS-datan tuotto kytkeytyy päälle ja pois veturin kuljettajan toimesta, joten on mahdollista, että juna tuottaa GPS-dataa vaikka se ei olekaan ajossa. Yli 500 metriä lähimmästä radasta sijaitsevat GPS-sijainnit suodatetaan pois.

Kehittäjien kannattaa huomioida junien sijaintitietojen puutteet sovelluksissaan ja kertoa rajoitteista selkeästi myös sovellusten loppukäyttäjille. Puutteiden ja virheiden vuoksi tietojen käyttöä turvallisuuteen liittyvissä sovelluksissa kannattaa harkita tarkkaan.

### Kaikkien junien sijainnit

URL: `/train-locations/latest?bbox=<points>`

* Esimerkki 1: [/train-locations/latest/](https://rata.digitraffic.fi/api/v1/train-locations/latest/)
* Esimerkki 2: [/train-locations/latest?bbox=20,60,35,70](https://rata.digitraffic.fi/api/v1/train-locations/latest?bbox=20,60,35,70)
* Esimerkki 2: [/train-locations.geojson/latest?bbox=20,60,35,70](https://rata.digitraffic.fi/api/v1/train-locations.geojson/latest?bbox=20,60,35,70)

**Kuvaus**

Palauttaa kaikkien junien GPS-tiedot, jotka ovat olleet aktiivisia viimeisen 15 minuutin sisällä.

|&nbsp;&nbsp;&nbsp;&nbsp;| Nimi | Formaatti | Esimerkki | Selitys
|---|---|---|---|--- 
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) | bbox | 4 numeroa | 	1,1,70,70 | 	Maantieteellinen rajaus WSG84-koordinaateilla. Kahdesta ensimmäisestä ja kahdesta viimeisestä numerosta muodostetaan rajausneliön määrittelevät pisteet

![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen

**Paluuarvo**

Palauttaa [GPS-sijainnit](#gps-sijainnit) -tyyppisen vastauksen.

### Yhden junan sijainti

URL: `/train-locations/<departure_date>/<train_number>?bbox=<points>`

Esimerkkejä:
* [/train-locations/latest/1](https://rata.digitraffic.fi/api/v1/train-locations/latest/1)
* [/train-locations/2018-03-01/1](https://rata.digitraffic.fi/api/v1/train-locations/2018-03-01/1)

**Kuvaus**

Palauttaa junan GPS-sijainnit lähtöpäivänä.

Mikäli lähtöpäivänä käytetään arvoa "latest" palautetaan GPS-sijainti, joka on päivittynyt viimeisen 15 minuutin sisällä.

|&nbsp;&nbsp;&nbsp;&nbsp;| Nimi | Formaatti | Esimerkki | Selitys
|---|---|---|---|---
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) | train_number | 1-99999 | 	1 | Junan numero. Esimerkiksi junan "IC 59" junanumero on 59.
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) | departure_date | date(yyyy-mm-dd) | 2017-01-01 | Junan ensimmäisen lähdön päivämäärä Suomen ajassa. Jos arvo on "latest" (esim. train-locations/latest/1), palautetaan uusin, vähintään 15 minuutin sisällä päivittynyt GPS-sijainti. 
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) | bbox | 4 numeroa | 	1,1,70,70 | 	Maantieteellinen rajaus WSG84-koordinaateilla. Kahdesta ensimmäisestä ja kahdesta viimeisestä numerosta muodostetaan rajausneliön määrittelevät pisteet

![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen

**Paluuarvo**

Palauttaa [GPS-sijainnit](#gps-sijainnit) -tyyppisen vastauksen.

### Vanhat sijainnit zip-paketteina

Vanhat sijainnit löytyvät zip-paketteina osoitteesta [/api/v1/train-locations/dumps/list.html](https://rata.digitraffic.fi/api/v1/train-locations/dumps/list.html)

Paketin sisältämä json on saman muotoista kuin muutkin [GPS-sijainti-vastaukset](#gps-sijainnit).

Uusi paketti luodaan joka päivä noin kello 15:30. Paketin sisältö on kaksi päivää vanhaa. Esimerkiksi 11.9.2018 15:30 luodaan päivän 9.9. junat.

### Sijainnit GeoJSON-muodossa

Lisää osoitteen perään `.geojson`

Esimerkkejä:

*  [/train-locations.geojson/latest](https://rata.digitraffic.fi/api/v1/train-locations.geojson/latest)
*  [/train-locations.geojson/latest/1](https://rata.digitraffic.fi/api/v1/train-locations.geojson/latest/1)

## Tarkempi seuranta kulkutietoviestien avulla (/train-tracking)

Liikennepaikkakohtaisten toteumien ja ennusteiden lisäksi junaa voidaan seurata ja paikantaa raideosuustarkkuudella kulkutietoviestien avulla

Kun juna saapuu raideosuudelle, aktivoituu raideosuuden anturi ja raideosuus varautuu kyseiselle junalle. Varatumisesta muodostuu "OCCUPY"-tyyppinen kulkutietoviesti. Junan poistuessa raideosuudelta syntyy puolestaan "RELEASE"-tyyppinen kulkutietoviesti. Kulkutietoviestit kertovat siis mitä raideosuuksia juna on varannut itselleen kuljettavaksi.

Kulkutietoviestejä voi seurata kahdella tapaa. Perinteisellä REST-rajapinalla (eli kuten esimerkiksi "live-trains"-liittymää) tai socketeilla (MQTT).

Kulkutietoviestejä kertyy päivittäin yli 300 000. On siis hyvä miettiä halutaanko hyödyntää kulkutietoviestejä vai luvussa 1.1 kuvattuja liikennepaikkakohtaisia toteumia ja ennusteita.

![warning]({{ site.baseurl }}{{ "/img/rata/warn.png" }}) Datan laatu ei ole aina optimaalista. Tunnettuja välillä esiintyviä vikoja:

* Seuraavan ja edellisen aseman/raideosuuden puuttuvat
* Junan lähtöpäivämäärä tyhjä
* Viestejä esiintyy tuplana (samat tiedot, eri id)

Kulkutietoviestit välitetään avoimen datan rajapintaan käytännössä sellaisena kuin ne saadaan kauko-ohjausjärjestelmistä. Virheellisiä viestejä lähettäviä kauko-ohjausjärjestelmiä pyritään korjaamaan jatkuvasti palautteen avulla.

### Kaikkien junien seuranta

URL: `/train-tracking?version=<version>`

Esimerkiksi: [/train-tracking?version=65403053026](https://rata.digitraffic.fi/api/v1/train-tracking?version=65403053026)

**Kuvaus**

Palauttaa kaikki kulkutietoviestit, joiden versionumero on suurempi kuin parametrina annettuna versio.

Maksimissaan palautetaan 2500 kulkutietoviestiä.

**Hakuehdot**

|&nbsp;&nbsp;&nbsp;&nbsp;| Nimi | Formaatti | Esimerkki | Selitys
|---|---|---|---|--- 
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) | version | positive integer | 6403053026 | Versionumero, jota uudemmat kulkutietoviestit palautetaan. Jos versionumeroa ei anneta, palautetaan kulkutietoviestit uusimmalla versionumerolla

![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen

**Paluuarvo**

Palauttaa [Kulkutietoviestit](#kulkutietoviestit)-tyyppisen vastauksen.

### Yhden junan seuranta

URL: `/train-tracking/<departure_date>/<train_number>?version=<version>`

Esimerkki: [/train-tracking/2017-01-01/1?version=1000](https://rata.digitraffic.fi/api/v1/train-tracking/2017-01-01/1?version=1000)

**Kuvaus**

Palauttaa halutun yhden junan kulkutietoviestit.

![warning]({{ site.baseurl }}{{ "/img/rata/warn.png" }}) Kyselyyn otetaan mukaan myös kulkutietoviestit, joilla ei ole lähtöpäivämäärää (departureDate) edellisen ja seuraavan vuorokauden rajauksella. Tällöin saattaa palautua "eilisen" kulkutietoviestejä.

**Hakuehdot**

|&nbsp;&nbsp;&nbsp;&nbsp;| Nimi | Formaatti | Esimerkki | Selitys
|---|---|---|--- |--- 
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) | train_number | 1-99999 | 1, 3402 | Junan numero. Esimerkiksi junan "IC 59" junanumero on 59.
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) | departure_date | date(yyyy-mm-dd) | 2017-01-01 | Junan ensimmäisen lähdön päivämäärä Suomen ajassa. Jos arvo on "latest" (esim. train-tracking/latest/1) , palauttaa uusimman lähdön kulkutietoviestejä. Palauttaa lisäksi kulkutietoviestit ilman lähtöpäivämäärää +1..-1 päivän rajauksella.
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})  | version | positive integer | 159123295871 | Versiorajoitus. Jos juna ei ole muuttunut sitten määritellyn version, palautetaan tyhjä tulos. Jos tyhjä, ei tehdä versiorajoitusta.

![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen

**Paluuarvo**

Palauttaa [Kulkutietoviestit](#kulkutietoviestit)-tyyppisen vastauksen.

### Liikennepaikan seuranta

URL: `/train-tracking/station/<station>/<departure_date>`

Esimerkki: [/train-tracking/station/JY/2017-08-01](https://rata.digitraffic.fi/api/v1/train-tracking/station/JY/2017-08-01)

**Kuvaus**

Palauttaa liikennepaikan kulkutietoviestit.

![warning]({{ site.baseurl }}{{ "/img/rata/warn.png" }}) Kyselyyn otetaan mukaan myös kulkutietoviestit, joilla ei ole lähtöpäivämäärää (departureDate) edellisen ja seuraavan vuorokauden rajauksella. Tällöin saattaa palautua "eilisen" kulkutietoviestejä.

**Hakuehdot**

|&nbsp;&nbsp;&nbsp;&nbsp;| Nimi | Formaatti | Esimerkki | Selitys
|---|---|---|--- |--- 
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }})  | station | string | "HKI" | Liikennepaikan lyhenne. Lyhennekoodit löytyvät [täältä](https://rata.digitraffic.fi/api/v1/metadata/stations)
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }})  | departure_date | date(yyyy-mm-dd) | 2017-01-01 | Junan ensimmäisen lähdön päivämäärä Suomen ajassa. Palauttaa lisäksi kulkutietoviestit ilman lähtöpäivämäärää hakuparametria seuraavalta päivältä kello 16:00 asti.

![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen

**Paluuarvo**

Palauttaa [Kulkutietoviestit](#kulkutietoviestit)-tyyppisen vastauksen.

### Raideosuuden seuranta

URL: `/train-tracking/station/<station>/<departure_date>/<track_section>`

Esimerkkejä:
- [/train-tracking/station/PSL/2017-01-01/293](https://rata.digitraffic.fi/api/v1/train-tracking/station/PSL/2017-01-01/293)
- [/train-tracking/station/PSL/latest/293](https://rata.digitraffic.fi/api/v1/train-tracking/station/PSL/latest/293)
- [/train-tracking/station/PSL/latest/293?limit=150](https://rata.digitraffic.fi/api/v1/train-tracking/station/PSL/latest/293?limit=150)

**Kuvaus**

Palauttaa liikennepaikan raideosuuden kulkutietoviestit.

![warning]({{ site.baseurl }}{{ "/img/rata/warn.png" }}) Kyselyyn otetaan mukaan myös kulkutietoviestit, joilla ei ole lähtöpäivämäärää (departureDate) edellisen ja seuraavan vuorokauden rajauksella. Tällöin saattaa palautua "eilisen" kulkutietoviestejä.

**Hakuehdot**

|&nbsp;&nbsp;&nbsp;&nbsp;| Nimi | Formaatti | Esimerkki | Selitys
|---|---|---|--- |--- 
 ![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }})| station | string | "HKI" | Liikennepaikan lyhenne. Lyhennekoodit löytyvät [täältä](https://rata.digitraffic.fi/api/v1/metadata/stations)
 ![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }})| track_section | string | "001" | Liikennepaikan raideosuuden lyhenne. Lyhennekoodit löytyvät [täältä](https://rata.digitraffic.fi/api/v1/metadata/stations)
 ![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }})| departure_date | date (yyyy-mm-dd) | 2017-01-01 | Kulkutietoviestiin liittyvän Junan ensimmäisen lähdön päivämäärä Suomen ajassa. Palauttaa lisäksi kulkutietoviestit ilman lähtöpäivämäärää hakuparametria seuraavalta päivältä kello 16:00 asti. Jos arvo on "latest", palautetaan uusimpia kulkutietoviestejä.
 ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) | limit | positive integer | 100 | Kuinka monta uusinta kulkutietoviestiä kyselyssä palautetaan. Maksimiarvo 1000. Tämä rajaus poissulkee departure_date-rajauksen. Jos departure_date- tai limit-rajoitusta ei anneta, käytetään limit-rajoitusta.

![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen

**Paluuarvo**

Palauttaa [Kulkutietoviestit](#kulkutietoviestit)-tyyppisen vastauksen.

## Kokoonpanotiedot (/compositions)

Kokoonpanotietoja tulee junille 0-5 tuntia ennen junan lähtö tai pysähdystä, jossa kokoonpano muuttuu.

![warning]({{ site.baseurl }}{{ "/img/rata/warn.png" }}) Moottorivaunut (esimerkiksi tyypit Sm3, Sm4, Sm5) on yleisesti ilmoitettu kokoonpanoissa vaunuina.

### Junan kokoonpanohaku

URL: `/compositions/<departure_date>/<train_number>`

Esimerkki: [/compositions/2017-01-01/1](https://rata.digitraffic.fi/api/v1/compositions/2017-01-01/1)

**Kuvaus**

Palauttaa yksittäisen junan kokoonpanotiedot tiettynä päivämääränä.

**Hakuehdot**

|&nbsp;&nbsp;&nbsp;&nbsp;|Nimi|Formaatti|Selitys|
|---|---|---|--- 
 ![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) | train_number | 1-99999 | 1 | Junan numero. Esimerkiksi junan "IC 59" junanumero on 59.
 ![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) | departure_date | date(yyyy-mm-dd) | 2017-01-01 | Lähtöpäivämäärä

![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen

**Paluuarvo**

Palauttaa [Kokoonpanot](#kokoonpanot)-tyyppisen vastauksen.

### Junien kokoonpanohaku

URL: `/compositions/<departure_date>`

Esimerkki: [/compositions/2017-01-01](https://rata.digitraffic.fi/api/v1/compositions/2017-01-01)

**Kuvaus**

Palauttaa junien kokoonpanotiedot halutulta vuorokaudelta.

**Hakuehdot**

|&nbsp;&nbsp;&nbsp;&nbsp;|Nimi|Formaatti|Esimerkki
|---|---|---|--- 
 ![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }})|departure_date|date(yyyy-mm-dd)|2017-01-01

![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen

**Paluuarvo**

Palauttaa [Kokoonpanot](#kokoonpanot)-tyyppisen vastauksen.

### Kaikkien kokoonpanojen seuranta

URL: `/compositions?version=<version>`

Esimerkki: [/compositions?version=12349873459128375](https://rata.digitraffic.fi/api/v1/compositions?version=12349873459128375)

**Kuvaus**

Palauttaa kaikki kokoonpanot, jotka ovat uudempia kuin `version`

**Paluuarvo**

Palauttaa [Kokoonpanot](#kokoonpanot)-tyyppisen vastauksen.

**Hakuehdot**

|&nbsp;&nbsp;&nbsp;&nbsp;| Nimi | Formaatti | Esimerkki | Selitys
  |---|---|---|--- |--- 
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) | version | positive integer | 6403053026 | Versiorajoitus. Palauttaa kaikki kokoonpanot, jotka ovat muuttuneet sitten `version`. Jos versionumeroa ei anneta, palautetaan uusin kokoonpano.

![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen

### Vanhat kokoonpanot zip-paketteina

Vanhat kokoonpanot löytyvät zip-paketteina osoittesta [/api/v1/compositions/dumps/list.html](https://rata.digitraffic.fi/api/v1/compositions/dumps/list.html)

Paketin sisältämä json on saman muotoista kuin muutkin [kokoonpano-vastaukset](#kokoonpanot).

Uusi paketti luodaan joka kuun viides päivä. 

### Kokoonpanojen versiohistoria

URL: `/compositions/history/<departure_date>/<train_number>`

Esimerkki: [/compositions/history/2019-01-01/1](https://rata.digitraffic.fi/api/v1/compositions/history/2019-01-01/1)

**Kuvaus**

Palauttaa junan kokoonpanojen kaikki versiot. Historiatietoa säilytetään tallessa 14 päivää

**Hakuehdot**

|&nbsp;&nbsp;&nbsp;&nbsp;|Nimi|Formaatti|Selitys|
|---|---|---|--- 
 ![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) | train_number | 1-99999 | 1 | Junan numero. Esimerkiksi junan "IC 59" junanumero on 59.
 ![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) | departure_date | date(yyyy-mm-dd) | 2017-01-01 | Lähtöpäivämäärä

![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen

**Paluuarvo**

Palauttaa [versiohistoria](#versiohistoria)-tyyppisen vastauksen, jossa json-kenttä on muotoa [kokoonpanot](#kokoonpanot).

## Kulkutievaraukset (/routesets)

Jotta juna voi kulkea eteenpäin, sen tarvitsee varata edestään turvallinen kulkutie. Kulkutievaraukset kertovat yksityiskohtaisesti (raideosuus/opastin/vaihe-tasolla), millainen kulkutie edestä on varattu.

### Kaikkien kulkutievarauksien seuranta

URL: `/routesets?version=<version>`

Esimerkki: [/routesets?version=12349873459128375](https://rata.digitraffic.fi/api/v1/routesets?version=12349873459128375)

**Kuvaus**

Palauttaa kaikki kulkutievaraukset, jotka ovat uudempia kuin `version`

**Paluuarvo**

Palauttaa [Kulkutievaraukset](#kulkutievaraukset)-tyyppisen vastauksen järjestettynä `version`-kentän mukaan nousevasti.

**Hakuehdot**

|&nbsp;&nbsp;&nbsp;&nbsp;| Nimi | Formaatti | Esimerkki | Selitys
  |---|---|---|--- |--- 
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) | version | positive integer | 6403053026 | Versiorajoitus. Palauttaa kaikki kulkutievaraukset, jotka ovat muuttuneet sitten `version`. Jos versionumeroa ei anneta, palautetaan uusin kulkutievaraus.

![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen

### Yhden junan kulkutievaraukset

URL: `/routesets/<departure_date>/<train_number>`

Esimerkki: [/routesets/2019-05-20/1](https://rata.digitraffic.fi/api/v1/routesets/2019-05-20/1)

**Kuvaus**

Palauttaa yksittäisen junan kulkutievaraukset tiettynä päivämääränä järjestettynä nousevasti `messageTime`- ja `sectionOrder`-kentän mukaan

**Hakuehdot**

|&nbsp;&nbsp;&nbsp;&nbsp;|Nimi|Formaatti|Esimerkki|
|---|---|---|--- 
 ![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) | train_number | 1-99999 | 1 | Junan numero. Esimerkiksi junan "IC 59" junanumero on 59.
 ![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) | departure_date | date(yyyy-mm-dd) | 2017-01-01 | Lähtöpäivämäärä

![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen

**Paluuarvo**

Palauttaa [Kulkutievaraukset](#kulkutievaraukset)-tyyppisen vastauksen.

### Liikennepaikan kulkutievaraukset

URL: `/routesets/station/<station>/<departure_date>`

Esimerkki: [/routesets/station/JY/2019-05-20](https://rata.digitraffic.fi/api/v1/routesets/station/JY/2019-05-20)

**Kuvaus**

Palauttaa liikennepaikan kulkutievaraukset.

**Hakuehdot**

|&nbsp;&nbsp;&nbsp;&nbsp;|Nimi|Formaatti|Esimerkki|
|---|---|---|--- 
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }})| station | string | "HKI" | Liikennepaikan lyhenne. Lyhennekoodit löytyvät [täältä](https://rata.digitraffic.fi/api/v1/metadata/stations)
 ![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }})| departure_date | date (yyyy-mm-dd) | 2017-01-01 | Kulkutievaraukseen liittyvän junan ensimmäisen lähdön päivämäärä Suomen ajassa. Palauttaa lisäksi kulkutievaraukset ilman lähtöpäivämäärää ajalta 00:00 - 24:00.

![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen

**Paluuarvo**

Palauttaa [Kulkutievaraukset](#kulkutievaraukset)-tyyppisen vastauksen järjestettynä nousevasti `messageTime`- ja `sectionOrder`-kentän mukaan

## Ratatyötiedot (/trackwork-notifications) ja liikenteen rajoitetiedot (trafficrestriction-notifications)

Ratatyöilmoitukset ja liikenteen rajoite-ilmoitukset ovat peräisin RUMA-järjestelmästä. Kuvaus tietosisällöstä löytyy [täältä]({{ site.baseurl }}/{{ site.t.railway-traffic.url[page.lang] }}{{ "/ruma" }}).

### Ratatyöilmoitukset

Ratatyöilmoitukset ovat versioituvia. Avoimeen dataan ei tuoda kaikkia tietoje, joten peräkkäiset versiot voivat olla tietosisällöltään identtisiä.  

#### Ratatyöilmoitusten haku aikavälillä

URL: `/trackwork-notifications.{json/geojson}`

Esimerkki: [/trackwork-notifications.json?state=ACTIVE](https://rata.digitraffic.fi/api/v1/trackwork-notifications.json?state=ACTIVE)

**Kuvaus**
Palauttaa ratatyöilmoitukset JSON tai GeoJSON-muodossa. Annettava aikaväli rajaa ilmoituksia version luontihetken eli *modified*-kentän perusteella.
Aikavälille osuvasta ilmoituksesta palautetaan aina uusin versio. 

**Hakuehdot**

|&nbsp;&nbsp;&nbsp;&nbsp;|Nimi|Formaatti|Esimerkki|
|---|---|---|--- 
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})| state | string | "ACTIVE" | Ilmoituksen tila, arvot: SENT, ACTIVE, PASSIVE, FINISHED. Oletusarvo ACTIVE.
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})| schema | boolean | true | Näytetäänkö ilmoituksen kaaviosijainnit? Oletusarvo false.
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})| start | date | 2019-01-01T00:00:00.000Z | Aikavälin alku. Oletusarvo nykyhetki - 7 päivää.
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})| end | date | 2019-02-04T00:00:00.000Z | Aikavälin loppu. Oletusarvo nykyhetki.
 
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen


#### Ratatyöilmoitusten uusimpien versioiden haku aikavälillä

URL: `/trackwork-notifications/status`

Esimerkki: [/trackwork-notifications/status](https://rata.digitraffic.fi/api/v1/trackwork-notifications/status)

**Kuvaus**
Palauttaa aikavälille osuvien ilmoituksen tunnisteet ja uusimmat versiot.

**Hakuehdot**

|&nbsp;&nbsp;&nbsp;&nbsp;|Nimi|Formaatti|Esimerkki|
|---|---|---|--- 
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})| start | date | 2019-01-01T00:00:00.000Z | Aikavälin alku. Oletusarvo nykyhetki - 7 päivää.
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})| end | date | 2019-02-04T00:00:00.000Z | Aikavälin loppu. Oletusarvo nykyhetki.
 
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen

**Paluuarvo**
Palauttaa vastauksena joukon [Ilmoituksen status](#ilmoituksen-status)-tyyppisiä alkioita järjestettynä nousevasti `modified`- ja `id`-kentän mukaan.


#### Ratatyöilmoitusten kaikkien versioiden palautus

URL: `/trackwork-notifications/<id>`

Esimerkki: [/trackwork-notifications/1.2.246.586.7.1.166700](https://rata.digitraffic.fi/api/v1/trackwork-notifications/1.2.246.586.7.1.166700)

**Kuvaus**
Palauttaa tietyn ilmoituksen kaikki versiot. 

**Hakuehdot**

|&nbsp;&nbsp;&nbsp;&nbsp;|Nimi|Formaatti|Esimerkki|
|---|---|---|--- 
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})| schema | boolean | true | Näytetäänkö ilmoituksen kaaviosijainnit? Oletusarvo false.
 
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen


#### Ratatyöilmoitusten tietyn version palautus

URL: `/trackwork-notifications/<id>/<version>`

Esimerkki: [/trackwork-notifications/1.2.246.586.7.1.166700/5](https://rata.digitraffic.fi/api/v1/trackwork-notifications/1.2.246.586.7.1.166700/5)

**Kuvaus**
Palauttaa tietyn ilmoituksen tietyn version. 

**Hakuehdot**

|&nbsp;&nbsp;&nbsp;&nbsp;|Nimi|Formaatti|Esimerkki|
|---|---|---|--- 
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})| schema | boolean | true | Näytetäänkö ilmoituksen kaaviosijainnit? Oletusarvo false.
 
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen


### Liikenteen rajoite-ilmoitukset

Liikenteen rajoite-ilmoitukset ovat versioituvia. Avoimeen dataan ei tuoda kaikkia tietoje, joten peräkkäiset versiot voivat olla tietosisällöltään identtisiä.  

#### Liikenteen rajoite-ilmoitusten haku aikavälillä

URL: `/trafficrestriction-notifications.{json/geojson}`

Esimerkki: [/trafficrestriction-notifications.json?state=SENT](https://rata.digitraffic.fi/api/v1/trafficrestriction-notifications.json?state=SENT)

**Kuvaus**
Palauttaa ilmoitukset JSON tai GeoJSON-muodossa. Annettava aikaväli rajaa ilmoituksia version luontihetken eli *modified*-kentän perusteella.
Aikavälille osuvasta ilmoituksesta palautetaan aina uusin versio. 

**Hakuehdot**

|&nbsp;&nbsp;&nbsp;&nbsp;|Nimi|Formaatti|Esimerkki|
|---|---|---|--- 
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})| state | string | "ACTIVE" | Liikenteen ilmoituksen tila, arvot: SENT, FINISHED. Oletusarvo SENT.
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})| schema | boolean | true | Näytetäänkö ilmoituksen kaaviosijainnit? Oletusarvo false.
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})| start | date | 2019-01-01T00:00:00.000Z | Aikavälin alku. Oletusarvo nykyhetki - 7 päivää.
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})| end | date | 2019-02-04T00:00:00.000Z | Aikavälin loppu. Oletusarvo nykyhetki.
 
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen


#### Liikenteen rajoite-ilmoitusten uusimpien versioiden haku aikavälillä

URL: `/trafficrestriction-notifications/status`

Esimerkki: [/trafficrestriction-notifications/status](https://rata.digitraffic.fi/api/v1/trafficrestriction-notifications/status)

**Kuvaus**
Palauttaa aikavälille osuvien ilmoitusten tunnisteet ja uusimmat versiot.

**Hakuehdot**

|&nbsp;&nbsp;&nbsp;&nbsp;|Nimi|Formaatti|Esimerkki|
|---|---|---|--- 
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})| start | date | 2019-01-01T00:00:00.000Z | Aikavälin alku. Oletusarvo nykyhetki - 7 päivää.
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})| end | date | 2019-02-04T00:00:00.000Z | Aikavälin loppu. Oletusarvo nykyhetki.
 
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen

**Paluuarvo**
Palauttaa vastauksena joukon [Ilmoituksen status](#ilmoituksen-status)-tyyppisiä alkioita järjestettynä nousevasti `modified`- ja `id`-kentän mukaan.


#### Liikenteen rajoite-ilmoituksen kaikkien versioiden palautus

URL: `/trafficrestriction-notifications/<id>`

Esimerkki: [/trafficrestriction-notifications/1.2.246.586.7.2.102883](https://rata.digitraffic.fi/api/v1/trafficrestriction-notifications/1.2.246.586.7.2.102883)

**Kuvaus**
Palauttaa tietyn ilmoituksen kaikki versiot. 

**Hakuehdot**

|&nbsp;&nbsp;&nbsp;&nbsp;|Nimi|Formaatti|Esimerkki|
|---|---|---|--- 
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})| schema | boolean | true | Näytetäänkö ilmoituksen kaaviosijainnit? Oletusarvo false.
 
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen


#### Liikenteen rajoite-ilmoituksen tietyn version palautus

URL: `/trafficrestriction-notifications/<id>/<version>`

Esimerkki: [/trafficrestriction-notifications/1.2.246.586.7.2.102883/1](https://rata.digitraffic.fi/api/v1/trafficrestriction-notifications/1.2.246.586.7.2.102883/1)

**Kuvaus**
Palauttaa tietyn ilmoituksen tietyn version. 

**Hakuehdot**

|&nbsp;&nbsp;&nbsp;&nbsp;|Nimi|Formaatti|Esimerkki|
|---|---|---|--- 
![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }})| schema | boolean | true | Näytetäänkö ilmoituksen kaaviosijainnit? Oletusarvo false.
 
![pakollinen]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Pakollinen ![vapaaehtoinen]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Vapaaehtoinen



## Metatiedot (/metadata)

Palvelun metatietojen hakurajapinta.

### Liikennepaikkatiedot

URL:
* [metadata/stations](https://rata.digitraffic.fi/api/v1/metadata/stations)
* [metadata/stations.geojson](https://rata.digitraffic.fi/api/v1/metadata/stations.geojson)

**Kuvaus**

Palauttaa palvelun liikennepaikkojen tiedot. Tiedot päivittyvät lähdejärjestelmästä päivittäin n. klo 1:00.

**Paluuarvo**

Palauttaa [Liikennepaikat](#liikennepaikat)-tyyppisen vastauksen.

### Operaattoritiedot

URL: [metadata/operators](https://rata.digitraffic.fi/api/v1/metadata/operators)

**Kuvaus**

Palauttaa palvelun operaattoreiden tiedot. Tiedot päivittyvät lähdejärjestelmästä päivittäin n. klo 1:00.

**Paluuarvo**

Palauttaa [Operaattorit](#operaattorit)-tyyppisen vastauksen.

### Syyluokat

URL: [metadata/cause-category-codes](https://rata.digitraffic.fi/api/v1/metadata/cause-category-codes)

**Kuvaus**

Palauttaa listan palvelussa aktiivisesti käytössä olevista syyluokista. Syyluokat ovat yleisiä kategorioita syytiedoille. Kaikki syyluokat julkaistaan AvoinData-palvelun kautta. Jos haluat listaukseen mukaan käytöstä poistuneet tai käyttöön lisättävät syyluokat, lisää osoitteeseen parametri `show_inactive=true`.

**Paluuarvo**

Palauttaa Palauttaa [Syyluokat](#syyluokat)-tyyppisen vastauksen.-tyyppisen vastauksen.

### Syykoodit

URL: [metadata/detailed-cause-category-codes](https://rata.digitraffic.fi/api/v1/metadata/detailed-cause-category-codes)

**Kuvaus**

Palauttaa listan palvelussa käytössä olevista syykoodeista. Jokainen syyluokka on jaettu syykoodeihin eli syykoodi on syyluokan alempi taso. Kaikkia syykoodeja ei julkaista. Jos haluat listaukseen mukaan käytöstä poistuneet tai käyttöön lisättävät syykoodit, lisää osoitteeseen parametri `show_inactive=true`.

**Paluuarvo**

Palauttaa [Syykoodit](#syykoodit)-tyyppisen vastauksen.

### Kolmannen tason syykoodit

URL: [metadata /third-cause-category-codes](https://rata.digitraffic.fi/api/v1/metadata/third-cause-category-codes)

**Kuvaus**

Palauttaa listan palvelussa käytössä olevista kolmannen tason syykoodeista. Kolmannen tason syykoodi on syykoodin alempi taso. Kaikkia kolmannen tason syykoodeja ei julkaista. Jos haluat listaukseen mukaan käytöstä poistuneet tai käyttöön lisättävät kolmannen tason syykoodit, lisää osoitteeseen parametri `show_inactive=true`.

**Paluuarvo**

Palauttaa [Kolmannen tason syykoodit](#kolmannen-tason-syykoodit)-tyyppisen vastauksen.

### Junalajit

URL: [metadata/train-categories](https://rata.digitraffic.fi/api/v1/metadata/train-categories)

**Kuvaus**

Palauttaa listan palvelussa käytössä olevista junalajeista (esim. Cargo, Long-distance, Commuter). 

**Paluuarvo**

Palauttaa [Junalajit](#junalajit-1)-tyyppisen vastauksen.

### Junatyypit

URL: [metadata/train-types](https://rata.digitraffic.fi/api/v1/metadata/train-types)

**Kuvaus**

Palauttaa listan palvelussa käytössä olevista junatyypeista (esim. IC, P, P). Jokaisella junatyypillä on yläkäsitteenä junalaji (esim. lähijuna, kaukojuna, tavarajuna).

**Paluuarvo**

Palauttaa [Junatyypit](#junatyypit-1)-tyyppisen vastauksen.

### Raideosuudet

URL: [metadata/track-sections](https://rata.digitraffic.fi/api/v1/metadata/track-sections)

**Kuvaus**

Palauttaa listan raideosuuksista. Raideosuus on pienin osuus raiteesta, jonka yksittäinen juna voi varata käyttöönsä ja näin muodostaa turvallisen kulkureitin. Raideosuudella voi siis sijaita maksimissaan yksi juna.

Lista ei kata kaikkia kulkutietoviesteissä esiintyviä raideosuuksia. Datan laatua pyritään parantamaan.

**Paluuarvo**

Palauttaa [Raideosuudet](#raideosuudet-1)-tyyppisen vastauksen.

### Herätepisteet

URL: [metadata/train-running-message-rules](https://rata.digitraffic.fi/api/v1/metadata/train-running-message-rules)

**Kuvaus**

Herätepiste kuvaa miten kulkutietoviesti muunnetaan aikataulurivin toteumaksi taustajärjestelmässä.

Esimerkiksi kun saadaan kulkutietoviesti, joka vastaa herätepisteessä määriteltyä liikennepaikkaa, raideosuutta, varautumisen tyyppiä ja seuraavaa liikennepaikkaa, haetaan kulkutietoviestissä määritellyn junan aikataulurivi, joka vastaa herätepisteessä määritelty liikennepaikkaa ja aikataulurivityyppiä. Aikatauluriville kirjataan toteuma, joka on kulkutietoviestin aikaleima lisättynä offset-arvolla.

**Paluuarvo**

Palauttaa [Herätepisteet](#herätepisteet-1)-tyyppisen vastauksen.

### Aikataulukaudet ja muutosajankohdat

URL: [metadata/time-table-periods](https://rata.digitraffic.fi/api/v1/metadata/time-table-periods)

**Kuvaus**

Aikataulukausi kuvaa ajanjaksoa, jolle haetaan säännöllisiä aikatauluja. Junan aikataulu on säännöllinen, jos sen [vastauksessa](#junat) on mukana arvo `timetableType:"REGULAR"`.

Muutosajankohta on ajankohta aikataulukauden sisällä, jolloin jäljellä olevaan aikataulukauteen voidaan hakea muutoksia.

Muutosajankohtia voidaan käyttää arvioimaan junan aikataulun luotettavuutta eli sitä voiko se enää muuttua. Jos juna esimerkiksi lähtee 1.2.2018 ja seuraava muutosajankohta on 1.3.2018, junan aikataulu ei voi enää muuttua.

**Paluuarvo**

Palauttaa [Aikataulukaudet ja muutosajankohdat](#aikataulukaudet-ja-muutosajankohdat-1)-tyyppisen vastauksen.

## WebSocket (MQTT)

Aktiivisen hakemisen sijasta dataa voidaan myös kuunnella. Tähän käytetään MQTT-protokollaa.

### Yleistä MQTT:stä

MQTT vastaa kolmesta portista

1. Suojattu WebSocket osoitteessa rata.digitraffic.fi:443. Esimerkki: [https://jsfiddle.net/m20ocg9f/](https://jsfiddle.net/m20ocg9f/) 
1. Suojaamaton WebSocket osoitteessa rata.digitraffic.fi:80. Esimerkki: [https://jsfiddle.net/5x96Lo7n/](https://jsfiddle.net/5x96Lo7n/)
1. Normaalin TCP-yhteyden kautta osoitteessa rata-mqtt.digitraffic.fi:1883. TCP-porttiin ei voi selaimessa ottaa suoraan yhteyttä, joten esimerkkiä ei ole mahdollista muodostaa

MQTT:ssä kuunneltava data määritellään topicin avulla. Osia topic:sta voidaan korvata wildcard-merkeillä "#" ja "+". Esimerkiksi voidaan kuunnella kaikki kokoonpanoja topicista `compositions/#` tai vain yksittäisen junan kokoonpanoa topicista `compositions/+/5/#`. Lisätietoa [täältä](https://www.hivemq.com/blog/mqtt-essentials-part-5-mqtt-topics-best-practices)

### Junien kuuntelu

Topic: `trains/<departure_date>/<train_number>/<train-category>/<train-type>/<operator>/<commuter-line>/<running-currently>/<timetable-type>`

Esimerkki: [https://jsfiddle.net/t6gjuknf/](https://jsfiddle.net/t6gjuknf/)

**Paluuarvo**

Palauttaa [junat](#junat)-tyyppisiä vastauksia.

### Liikennepaikan junien kuuntelu

Topic: `trains-by-station/<station>`

Esimerkki: [https://jsfiddle.net/hm0wute4/](https://jsfiddle.net/hm0wute4/)

**Paluuarvo**

Palauttaa [junat](#junat)-tyyppisiä vastauksia.

### GPS-sijaintien kuuntelu

Topic: `train-locations/<departure_date>/<train_number>`

Esimerkki: [https://jsfiddle.net/4fae25yw/](https://jsfiddle.net/4fae25yw/)

**Paluuarvo**

Palauttaa [GPS-sijainnit](#gps-sijainnit)-tyyppisiä vastauksia.

### Kulkutietoviestien kuuntelu

Topic: `train-tracking/<departure_date>/<train_number>/<type>/<station>/<track_section>/<previous_station>/<next_station>/<previous_track_section>/<next_track_section>`

Esimerkki: [https://jsfiddle.net/ov94zmep/](https://jsfiddle.net/ov94zmep/)

**Paluuarvo**

Palauttaa [kulkutietoviestit](#kulkutietoviestit)-tyyppisiä vastauksia.

### Kulkutievarauksien kuuntelu

Topic: `routesets/<departure_date>/<train_number>`

Esimerkki: [https://jsfiddle.net/L0v2enb9/](https://jsfiddle.net/L0v2enb9/)

**Paluuarvo**

Palauttaa [kulkutievaraukset](#kulkutievaraukset)-tyyppisiä vastauksia.

### Kokoonpanojen kuuntelu

Topic: `compositions/<departure_date>/<train_number>/<train_category>/<train_type>/<operator>`

Esimerkki: [https://jsfiddle.net/g5x9ywnh/](https://jsfiddle.net/g5x9ywnh/)

**Paluuarvo**

Palauttaa [kokoonpanot](#kokoonpanot)-tyyppisiä vastauksia.

## Vastaustyypit

Kaikki vastaukset ovat JSON-formaattia. Eri vastaustyypit ovat kuvattu alla.

Jokainen tyypin kenttä on kuvattu ikonilla, jotka tarkoittavat:

![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) Ei voi olla tyhjä
![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) Voi olla tyhjä
![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) Komentti, jos tarpeen

### Junat

Järjestetty kenttien `departureDate` ja `trainNumber` mukaisesti nousevaan järjestykseen.

* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) trainNumber: 1-99999 ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junan numero. Esim junan "IC 59" junanumero on 59*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) departureDate: date ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junan ensimmäisen lähdön päivämäärä Suomen ajassa*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) operatorUICCode: 1-9999  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junan operoiman operaattorin UIC-koodi*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) operatorShortCode: vr, vr-track, destia, ... ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Lista operaattoreista löytyy [täältä](https://rata.digitraffic.fi/api/v1/metadata/operators).*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) trainType: IC, P, S, ...
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) trainCategory: lähiliikenne, kaukoliikenne, tavaraliikenne, ...
* ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) commuterLineID: Z, K, N....
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) runningCurrently: true/false ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Onko juna tällä hetkellä kulussa*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) cancelled: true/false ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Totta, jos junan peruminen on tehty 10 vuorokauden sisällä. Yli 10 vuorokautta sitten peruttuja junia ei palauteta rajapinnassa laisinkaan.*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) version: positive integer ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Versionumero, jossa juna on viimeksi muuttunut*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) timetableType: REGULAR tai ADHOC. ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Kertoo onko junan aikataulu haettu säännöllisenä (REGULAR) vai kiireellisenä yksittäistä päivää koskevana (ADHOC).*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) timetableAcceptanceDate: datetime. ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Ajanhetki jolloin viranomainen on hyväksynyt junan aikataulun.*
* ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) deleted: true,false ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Kertoo onko juna poistettu eli peruttu yli kymmenen päivää ennen lähtöä.*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) timeTableRows ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Kuvaa saapumisia ja lähtöjä liikennepaikoilta. Järjestetty reitin mukaiseen järjestykseen.*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) trainStopping true,false ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Pysähtyykö juna liikennepaikalla*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) stationShortCode: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Aseman lyhennekoodi*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) stationcUICCode: 1-9999 ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Aseman UIC-koodi*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) countryCode: "FI", "RU"
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) type: "ARRIVAL" tai "DEPARTURE" ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Pysähdyksen tyyppi*
    * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) commercialStop: boolean ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Onko pysähdys kaupallinen. Annettu vain pysähdyksille, ei läpiajoille. Mikäli junalla on osaväliperumisia, saattaa viimeinen perumista edeltävä pysähdys jäädä virheellisesti ei-kaupalliseksi.*
    * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) commercialTrack: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Suunniteltu raidenumero, jolla juna pysähtyy tai jolta se lähtee. Raidenumeroa ei saada junille, joiden lähtöön on vielä yli 10 päivää. Operatiivisissa häiriötilanteissa raide voi olla muu.*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) cancelled: true/false ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Totta, jos lähtö tai saapuminen on peruttu*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) scheduledTime: datetime  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Aikataulun mukainen pysähtymis- tai lähtöaika*
    * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) liveEstimateTime: datetime  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Ennuste. Tyhjä jos juna ei ole matkalla*
    * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) estimateSource: datetime ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Ennusteen lähde. Lisätietoa lähteistä [täältä](https://rata.digitraffic.fi/api/v1/doc/resources/estimate-sources.html).*
    * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) unknownDelay: boolean ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Jos ennustetta ei voida antaa luotettavasti, liikenteenohjaaja voi kytkeä unknownDelay-bitin päälle. Bitti tarkoittaa, että juna on myöhässä, mutta ei osata kertoa kuinka paljon. Lisätietoa: [https://www.liikennevirasto.fi/-/juna-myohassa-eika-arviota-lahtoajasta-asemien-nayttotaulut-kertovat-taman-pian-uudella-tavalla](https://www.liikennevirasto.fi/-/juna-myohassa-eika-arviota-lahtoajasta-asemien-nayttotaulut-kertovat-taman-pian-uudella-tavalla)*
    * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) actualTime: datetime ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Aika jolloin juna saapui tai lähti asemalta*
    * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) differenceInMinutes: integer ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Vertaa aikataulun mukaista aikaa ennusteeseen tai toteutuneeseen aikaan ja kertoo erotuksen minuutteina*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) causes ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Syytiedot. Kuvaavat syitä miksi juna oli myöhässä tai etuajassa pysähdyksellä. Kaikkia syyluokkia ja -tietoja ei julkaista.*
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) categoryCodeId ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Yleisen syyluokan yksilöivä tunnus. Lista syyluokista löytyy osoitteesta [täältä](https://rata.digitraffic.fi/api/v1/metadata/cause-category-codes)*
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) categoryCode ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Yleisen syyluokan koodi. Huom. ei yksilöivä.*
        * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) detailedCategoryCodeId ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Tarkemman syykoodin yksilöivä tunnus. Lista syykoodeista löytyy [täältä](https://rata.digitraffic.fi/api/v1/metadata/detailed-cause-category-codes)*
        * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) detailedCategoryCode ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Tarkempi syykoodin koodi. Huom. ei yksilöivä*
        * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) thirdCategoryCodeId ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Kolmannen tason syykoodin tunnus. Lista kolmannen tason syykoodeista löytyy [täältä](https://rata.digitraffic.fi/api/v1/metadata/third-cause-category-codes)*
        * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) thirdCategoryCode ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Kolmannen tason syykoodin koodi. Huom. ei yksilöivä*
    * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) trainReady ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junan lähtövalmius. Operaattorin tulee tehdä lähtövalmiusilmoitus liikenteenohjaajalle aina kun junan kokoonpanovaihtuu tai se lähtee ensimmäiseltä pysäkiltään.*
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) source ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Tapa, jolla lähtövalmiusilmoitus on tehty.*
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) accepted ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Onko lähtövalmiusilmoitus hyväksytty.*
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) timestamp ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Aika jolloin lähtövalmiusilmoitus on päätetty.*

### Kokoonpanot

Järjestetty kenttien `departureDate` ja `trainNumber` mukaisesti nousevaan järjestykseen.

* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) trainNumber: 1-99999  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junan numero. Esim junan "IC 59" junanumero on 59*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) departureDate: date  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junan ensimmäisen lähdön päivämäärä Suomen ajassa*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) operatorUICCode: 1-9999  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junan operoiman operaattorin UIC-koodi*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) operatorShortCode: vr, vr-track, destia, ... ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Lista operaattoreista löytyy [täältä](https://rata.digitraffic.fi/api/v1/metadata/operators).*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) trainCategory: lähiliikenne, kaukoliikenne, tavaraliikenne
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) trainType: P, S, IC, IC2, MUS, etc.
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) version: positive integer ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Versionumero, jossa juna on viimeksi muuttunut*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) journeySections ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Kuvaa junan yhtä matkaosuutta, joka ajetaan samalla kokoonpanolla*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) beginTimeTableRow ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Aloitus*
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) stationShortCode: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Aseman lyhennekoodi*
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) stationcUICCode: 1-9999 ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Aseman UIC-koodi*
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) countryCode: "FI" or "RU"
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) type: "ARRIVAL" tai "DEPARTURE" ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Pysähdyksen tyyppi*
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) scheduledTime: datetime ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Aikataulun mukainen pysähtymis- tai lähtöaika*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) endTimeTableRow ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }})
     *Lopetus*
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) stationShortCode: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Aseman lyhennekoodi*
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) stationcUICCode: 1-9999 ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Aseman UIC-koodi*
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) countryCode: "FI" tai "RU"
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) type: "ARRIVAL" tai "DEPARTURE" ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Pysähdyksen tyyppi*
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) scheduledTime: datetime ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Aikataulun mukainen pysähtymis- tai lähtöaika*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) locomotives  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Kokoonpanon veturit*
        * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }})vehicleNumber ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }})  *Veturin yksilöivä kalustoyksikkönumero. Saatavilla vain sarjatunnuksille `Sm1`, `Sm2`, `Sm4` ja `Sm5`. Lisätietoa [https://en.wikipedia.org/wiki/UIC_identification_marking_for_tractive_stock](https://en.wikipedia.org/wiki/UIC_identification_marking_for_tractive_stock)*
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) location: positive integer ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Veturin paikka kokoonpanossa. Pienin numero on junan kärjessä*
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) locomotiveType: SR1, SR2, ...  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Veturin tyyppi*
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) powerType: Diesel, Sähkö, ...  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Veturin vetovoimalaji*
    * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) wagons  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) 
    *Kokoonpanon vaunut*
        * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }})vehicleNumber ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }})  *Vaunun yksilöivä kalustoyksikkönumero. Saatavilla vain sarjatunnuksille `Sm1`, `Sm2`, `Sm4` ja `Sm5`. Lisätietoa [https://en.wikipedia.org/wiki/UIC_identification_marking_for_tractive_stock](https://en.wikipedia.org/wiki/UIC_identification_marking_for_tractive_stock)*
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }})location: integer ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Vaunun paikka kokoonpanossa. Pienin numero on junan kärjessä*
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }})salesNumber: 0-99 ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Vaunun myyntinumero. Lukee esimerkiksi matkustajan junalipussa. 0 jos ei tiedossa.*
        * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }})length: positive integer ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Vaunun pituus senttimetreinä*
        * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }})playground : true ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Onko vaunussa leikkipaikka*
        * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }})pet: true ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Onko vaunussa lemmikkivaunu*
        * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }})catering : true ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Onko vaunussa ravintolavaunu*
        * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }})video : true ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Onko vaunussa videonäyttömahdollisuus*
        * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }})luggage : true ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Onko vaunussa matkatavarasäilytysmahdollisuus*
        * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }})smoking : true ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Saako vaunussa tupakoida*
        * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }})disabled : true ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Onko vaunu invalidiystävällinen*
        * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }})wagonType ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }})  *Suomalainen sarjatunnus vaunulle. Ilmaisee vaunun tyypin sekä vaunun palvelut. Kaikille vaunuille ei välttämättä löydy sarjatunnusta. Lisätietoa [https://fi.wikipedia.org/wiki/Sarjatunnus](https://fi.wikipedia.org/wiki/Sarjatunnus)*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) totalLength: positive integer ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junan kokonaispituus metreissä*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) maximumSpeed: positive integer ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junan kokoonpanolle ilmoitettu maksiminopeus kilometreina tunnissa*
    
### GPS-sijainnit    

* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) trainNumber: string  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junan numero. Esim junan "IC 59" junanumero on 59*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) departureDate: date  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junan ensimmäisen lähdön päivämäärä Suomen ajassa. Voi olla tyhjä tapauksissa, jossa junan aikataulua ei tunneta.*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) timestamp: datetime ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Aikaleima jolloin sijainti on luettu*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) location: geojson ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Sijainti GeoJSON-muodossa*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) speed: number ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junan nopeus*
    
### Kulkutietoviestit

* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) id: positive integer ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Kulkutietoviestin yksilöivä numero.*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) version: positive integer ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Versionumero, jossa kulkutietoviesti on viimeksi muuttunut*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) trainNumber: string  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junan numero. Esim junan "IC 59" junanumero on 59*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) departureDate: date  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junan ensimmäisen lähdön päivämäärä Suomen ajassa. Voi olla tyhjä tapauksissa, jossa junan aikataulua ei tunneta.*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) timestamp: date  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Tapahtuman ajanhetki*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) trackSection: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Tapahtuman raideosuuden tunniste. Lista raideosuuksista löytyy [täältä](https://rata.digitraffic.fi/api/v1/metadata/track-sections).*
* ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) nextTrackSection: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Seuraava raideosuuden tunniste, jolle juna ajaa.*
* ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) previousTrackSection: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Raideosuuden tunniste, jolta juna tuli.*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) station: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Liikennepaikan tunniste, jonka alueella raideosuus on. Lista liikennepaikoista löytyy [täältä](https://rata.digitraffic.fi/api/v1/metadata/stations).*
* ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) nextStation: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Liikennepaikan tunniste, jonka alueella juna aiemmin oli.*
* ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) previousStation: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png"}}) *Liikennepaikan tunniste, jonka alueelle juna ajaa seuraavaksi.*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) type: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Tapahtuman tyyppi. `OCCUPY` tarkoittaa, että juna varasi raideosuuden. `RELEASE` tarkoittaa, että juna vapautti raideosuuden.*

### Kulkutievaraukset

* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) version: positive integer ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Versionumero, jossa kulkutievaraus on viimeksi muuttunut*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) messageTime: datetime ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Aikaleima jolloin kulkutievaraus on luettu*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) trainNumber: string  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junan numero. Esim junan "IC 59" junanumero on 59*
* ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) departureDate: date  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junan ensimmäisen lähdön päivämäärä Suomen ajassa. Voi olla tyhjä tapauksissa, jossa junan aikataulua ei tunneta.*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) routeType: character  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Varauksen tyyppi. `T`=junakulkutie, `S`=vaihtokulkutie ja `C`=kulkutien purkaminen*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) clientSystem: string  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Kulkutievarauksen luoneen kauko-ohjausjärjestelmän nimi*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) routesections ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Lista raideosuuksista/vaihteista/elementeistä, jotka on varattu kulkutieksi*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) sectionId: string  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Varattavan osuuden tunnus*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) stationCode: string  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Liikennepaikka, jossa varaus sijaitsee*
    * ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) commercialTrackId: string  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Raiteen kaupallinen tunnus*        

### Liikennepaikat

* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) passengerTraffic: boolean ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Onko liikennepaikalla kaupallista matkustajaliikennettä*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) countryCode: string  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Liikennepaikan maatunnus*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) stationName: string  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Liikennepaikan nimi*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) stationShortCode: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Liikennepaikan lyhenne*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) stationUICCode: 1-9999  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Liikennepaikan maakohtainen UIC-koodi*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) latitude: decimal  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Liikennepaikan latitude "WGS 84"-muodossa*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) longitude: decimal  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Liikennepaikan longitudi "WGS 84"-muodossa*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) type: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Liikennepaikan tyyppi. `STATION` = asema, `STOPPING_POINT` = seisake, `TURNOUT_IN_THE_OPEN_LINE` = linjavaihde*

### Operaattorit

* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) id: positive integer ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Operaattorin yksilöivä tunnus*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) operatorName: string  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Operaattorin nimi*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) operatorShortCode: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Operaattorin lyhenne*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) operatorUICCode: 1-9999  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Operaattorin UIC-koodi*
* ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) trainNumbers ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Operaattorin käytössäolevat junanumeroavaruudet.*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) id: positive integer  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junanumeroavaruuden yksilöivä tunnus*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) bottomLimit: 1-99999 ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junanumeroiden alaraja*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) topLimit: 1-99999 ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junanumeroiden yläraja*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) trainCategory: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junalaji*

### Syyluokat

* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) id: positive integer  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Syyluokan yksilöivä tunnus*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) categoryCode: string  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Syyluokan tunnus*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) categoryName: string  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Syyluokan suomenkielinen nimi*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) validFrom: string  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Ajanhetki jolloin syyluokka astuu voimaan. Ajanhetki viittaa junan lähtöpäivämäärään.*
* ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) validTo: string  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Ajanhetki jolloin syyluokka poistuu voimasta. Voi olla tyhjä, jolloin syyluokka on toistaiseksi voimassa. Ajanhetki viittaa junan lähtöpäivämäärään.*
* ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) passengerTerm ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Matkustajatiedotustermi. Matkustajaläheinen kuvaus syyluokasta*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) fi: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Suomenkielinen käännös*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) en: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Englanninkielinen käännös*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) sv: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Ruotsinkielinen käännös*

### Syykoodit

* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) id: positive integer  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Syykoodin yksilöivä tunnus*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) detailedCategoryCode: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Syykoodin tunnus*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) detailedCategoryName: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Syykoodin suomenkielinen nimi*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) validFrom: string  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Ajanhetki jolloin syykoodi astuu voimaan. Ajanhetki viittaa junan lähtöpäivämäärään.*
* ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) validTo: string  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Ajanhetki jolloin syykoodi poistuu voimasta. Voi olla tyhjä, jolloin syykoodi on toistaiseksi voimassa. Ajanhetki viittaa junan lähtöpäivämäärään.*
* ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) passengerTerm ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Matkustajatiedotustermi. Matkustajaläheinen kuvaus syykoodista*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) fi: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Suomenkielinen käännös*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) en: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Englanninkielinen käännös*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) sv: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Ruotsinkielinen käännös*

### Kolmannen tason syykoodit

* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) id: positive integer  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Kolmannen tason syykoodin yksilöivä tunnus*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) thirdCategoryCode: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Kolmannen tason syykoodin tunnus*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) thirdCategoryName: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Kolmannen tason syykoodin suomenkielinen nimi*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) validFrom: string  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Ajanhetki jolloin kolmannen tason syykoodin astuu voimaan. Ajanhetki viittaa junan lähtöpäivämäärään.*
* ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) validTo: string  ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Ajanhetki jolloin kolmannen tason syykoodin astuu poistuu voimasta. Voi olla tyhjä, jolloin kolmannen tason syykoodin astuu on toistaiseksi voimassa. Ajanhetki viittaa junan lähtöpäivämäärään.*
* ![Optional]({{ site.baseurl }}{{ "/img/rata/optional.png" }}) passengerTerm ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Matkustajatiedotustermi. Matkustajaläheinen kuvaus kolmannen tason syykoodista*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) fi: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Suomenkielinen käännös*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) en: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Englanninkielinen käännös*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) sv: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Ruotsinkielinen käännös*

### Junalajit

* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) name: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junalajin nimi*

### Junatyypit

* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) name: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junatyypin nimi*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) trainCategory ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junalaji*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) name: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junalajin nimi*

### Raideosuudet

* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) station: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Raideosuuden liikennepaikan lyhenne. Lista liikennepaikoista löytyy [täältä](https://rata.digitraffic.fi/api/v1/metadata/stations).*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) trackSectionCode: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Raideosuuden tunnus. Yksilöivä tieto*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) ranges ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Raideosuuden sijainnit. Raideosuudella voi olla monta sijaintia, jos se sijaitsee usealla eri ratanumerolla.*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) id : positive integer ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Sijainnin yksilöivä numero*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) startLocation: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Sijainnin alkukohta*
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) track: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Ratanumero*
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) kilometres: positive integer ![Info]({{ site.baseurl }}{{"/img/rata/info.png" }}) *Sijainnin kilometri-komponentti. Sijainti kilometreina rataverkon nollapisteestä.*
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) metres: positive integer ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Sijainnin metri-komponentti. Eli ylijäävä osuus kilometreistä.*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) endLocation ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Sijainnin loppukohta*
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) track: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Ratanumero*
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) kilometres: positive integer ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Sijainnin kilometri-komponentti. Sijainti kilometreina radan alkuosasta*
        * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) metres: positive integer ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Sijainnin metri-komponentti. Eli ylijäävä osuus kilometreistä.*

### Herätepisteet

* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) id: positive integer ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Herätepisteen yksilöivä numero*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) trainRunningMessageTrackSection: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Kulkutietoviestin raideosuus*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) trainRunningMessageStationShortCode: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Kulkutietoviestin liikennepaikka*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) trainRunningMessageNextStationShortCode: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Kulkutietoviestin seuraava liikennepaikka*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) trainRunningMessageType ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Kulkutietoviestin tyyppi*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) timeTableRowStationShortCode ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Aikataulurivin liikennepaikka*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) timeTableRowType ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Aikataulurivin tyyppi*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) offset ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Kuinka paljon aikaa sekunteina kulkutietoviestin aikaleimaan lisätään, jotta saadaan aikataulurivin toteuma*

### Aikataulukaudet ja muutosajankohdat

* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) id: positive integer ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Aikataulukauden yksilöivä numero*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) name: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Aikataulukauden nimi*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) effectiveFrom: date ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Aikataulukauden alkupäivämäärä*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) effectiveTo: date ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Aikataulukauden loppupäivämäärä*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) capacityAllocationConfirmDate: date ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Päivämäärä jolloin viranomainen tekee päätöksen kapasiteettihakemukselle*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) capacityRequestSubmissionDeadline: date ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Viimeinen päivämäärä, jolloin operaattorin kapasiteettihakemuksia aikataulukaudelle otetaan vastaan*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) changeDates ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Aikataulukauden muutosajankohdat*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) id: positive integer ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Muutosajankohdan yksilöivä numero*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) effectiveFrom: date ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Muutosajankohdan alkupäivämäärä*
    * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) capacityRequestSubmissionDeadline: date ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Viimeinen päivämäärä, jolloin operaattorin kapasiteettihakemuksia muutosajankohtaan otetaan vastaan*

### Versiohistoria

* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) version: positive integer ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Versionumero*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) id ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Versiotiedon yksilöivät tiedot*
  * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) trainNumber: positive integer ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junanumero*
  * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) departureDate: date ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Junan ensimmäisen lähdön päivämäärä Suomen ajassa*
  * ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) fetchDate: date ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Ajanhetki, jolloin versio on syntynyt*  
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) json: json ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Version json-sisältö*


### Ilmoituksen status

* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) id: string ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Ilmoituksen yksilöivä OID-tunniste*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) version: positive integer ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Uusin versionumero*
* ![Required]({{ site.baseurl }}{{ "/img/rata/required.png" }}) modified: date ![Info]({{ site.baseurl }}{{ "/img/rata/info.png" }}) *Uusimman version muokkausaika*


## Versionumeroiden käyttö

Useissa rajapinnan pyynnöissä parametrina on mukana `version`, joka rajaa vastauksesta pois junat, jotka eivät ole päivittyneet sitten `version` määrittelemän versionumeron.

Esimerkiksi kysely [/live-trains/station/HKI?arrived_trains=5](https://rata.digitraffic.fi/api/v1//live-trains/station/HKI?arrived_trains=5) saattaisi palauttaa seuraavan vastauksen:

```
[
   {
      "trainNumber":44,
      "departureDate":"2017-01-01",
      "operatorUICCode":10,
      "operatorShortCode":"vr",
      "trainType":"S",
      "trainCategory":"Long-distance",
      "runningCurrently":true,
      "cancelled":false,
      "version":3657782905,
      "timeTableRows":...
```      
Jos kyselyyn lisättäisiin versionumero [/live-trains/station/HKI?arrived_trains=5&version=3657782905](https://rata.digitraffic.fi/api/v1/live-trains/station/HKI?arrived_trains=5&version=3657782905), ei junaa 44 palautettaisi vastauksessa ennenkuin se on muuttunut.

Vastaanottajan on siis parsittava vastauksesta suurin versionumero ja käytettävä sitä seuraavassa kyselyssä parametrina.

## Avoimen datan käyttölupa

Rajapinnasta saatavien tietojen käyttölupa on [Creative Commons Nimeä 4.0](https://creativecommons.org/licenses/by/4.0/).

[Käyttöehdot](/kayttoehdot) -sivulla on lisätietoja käyttöluvasta ja ohjeita lähteen nimeämiseen.

[Fintrafficin avoimen datan sivustolla](https://www.fintraffic.fi/fi/fintraffic/kayttoehdot) on lisätietoja käyttöluvasta ja ohjeita lähteen nimeämiseen.

