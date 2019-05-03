---
layout: traffictype
permalink: /ohjeita/
section: Tietol�hteet
searchable: true
lang: fi
ref: instructions
title: Ohjeita
intro: Ohjeita ohjelmoijille
---

T�lle sivulle on ker�tty ohjeistusta ja tietoa rajapinnoista ja niiden k�yt�st�.

## Cache

Suurin osa rajapintojen kutsuista on cachetettu edustapalvelimilla.  T�m�n takia palveluita ei ole hy�ty� kutsua liian usein, koska 
cachesta palautuva vastaus ei muutu.  Suurimmassa osassa palveluita cachen ik� on yksi minuutti.

T�m� saattaa my�s aiheuttaa omituiselta tuntuvia aikaleimoja, kun samaa palvelua kutsuu eri parametreilla.  Esimerkiksi:

[```https://tie.digitraffic.fi/api/v1/data/tms-data?lastUpdated=true```](https://tie.digitraffic.fi/api/v1/data/tms-data?lastUpdated=true)
[```https://tie.digitraffic.fi/api/v1/data/tms-data?lastUpdated=false```](https://tie.digitraffic.fi/api/v1/data/tms-data?lastUpdated=false)

N�ist� voi tulla eri _dataUpdatedTime_, koska vastaukset ovat menneet cacheen eri aikoina.

## Pakkaus

Pakkausta kannattaa k�ytt��, sill� data on hyvin pakkautuvaa ja t�ll� s��st�� kaistaa ja aikaa. Pakkauksen k�ytt��notto riippuu hieman 
k�ytetyst� tekniikasta.  

### curl

```
curl -H 'Accept-Encoding: gzip' -H 'Connection: close' --compress https://tie.digitraffic.fi/api/v1/data/tms-data -o data.json
```

### wget

```
wget --header='Accept-Encoding: gzip' --header='Connection: close' https://tie.digitraffic.fi/api/v1/data/tms-data -O data.json
```

### Java RestTemplate

```
final HttpComponentsClientHttpRequestFactory clientHttpRequestFactory = new HttpComponentsClientHttpRequestFactory(HttpClientBuilder.create().build());
final RestTemplate restTemplate = new RestTemplate(clientHttpRequestFactory);

final String output = restTemplate.getForObject("https://tie.digitraffic.fi/api/v1/data/tms-data?testi=testi", String.class);
```