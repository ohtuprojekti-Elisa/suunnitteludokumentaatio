# Pelidata: GSI vs. Demo

## Sanasto

- Alkuperäinen palvelin = CS2 tai muu taho, joka lähettää pelidataa.  
- Käsittelypalvelin = Meidän toteutuksemme pelidatan käsittelyyn ja suoratoistoon loppukäyttäjälle.  
- Sovellus = Loppukäyttäjän operoima lisätyn todellisuuden sovellus.
## GSI-data

#### Hyödyt

- Data suoraan luettavissa (JSON).
- Käsittelypalvelimen rakentaminen on melko yksinkertaista.
- Yksi dataruutu on hyvin kevyt (~20 KB).
- Data voidaan suoratoistaa reaaliaikaisesti alkuperäiseltä palvelimelta käsittelypalvelimelle.

#### Haitat

- Päivitysväli on vain 1/3 sekuntia (~333 ms).
- Ei sisällä kaikkia mahdollisia pelaajan tiloja: seisominen/kyykky, juokseminen/käveleminen, ym.
- Yhteys alkuperäisen ja käsittelypalvelimen välillä tapahtuu REST API:n kautta, mikä ei ole kovin tehokas reaaliaikaiseen tiedonsiirtoon.

## Demo-data

#### Hyödyt

- Päivitysväli on 1/64 sekuntia (~15 ms) tai 1/128 sekuntia (~8 ms).
- Sisältää kaikki mahdolliset pelaajan ja maailman tilat.
- "Tiheämpi" data mahdollistaa sovelluksen loppukäyttäjälle näytettävät sulavammat ja tarkemmat animaatiot.
- Sisältää kaikki(?) pelaajan eri tilat muun muassa: isWalking, isAirborne ja isDucking.

#### Haitat

- Ei suoraan luettavassa muodossa ilman omaa parseria.
- Yhden ottelun datan tiedostokoko on melko suuri (~400-900 MB).
- Dataa täytyy käsitellä (parse) perusteellisemmin ennen lähettämistä sovellukseen.
- Vaatii enemmän verkkolinkiltä enemmän kaistanleveyttä, joten tämä asettaa alkuperäisen ja käsittelypalvelimen väliselle linkille suuremmat vaatimukset.
- Demo-tiedostoja ei voida suoratoistaa reaaliaikaisesti alkuperäiseltä palvelimelta, ellei sen rinnalle luoda erillistä palvelinsovellusta (onko varmasti näin ja onko edes mahdollista?).

## Linkit

- [gsi_cs2](https://docs.rs/gsi-cs2/latest/gsi_cs2/index.html)
- [Rendering Counter-Strike Demos in the Browser](https://healeycodes.com/rendering-counter-strike-demos-in-the-browser)
- [demoinfocs-golang - CS2 Demo Parser](https://github.com/markus-wa/demoinfocs-golang)
- [CS2 Demo Parsing, Analytics and Visualization in Python (AWPY)](https://github.com/pnxenopoulos/awpy)
	- [Example Parser JSON](https://awpy.readthedocs.io/en/latest/parser_output.html)
- [What is sub-tick in CS2? 64 vs 128 tick rate in CS2](https://www.charlieintel.com/counter-strike/what-is-sub-tick-in-cs2-servers-276995/)
- [HLTV - demoja](https://www.hltv.org/results?content=demo)
- [Compressing CS2 Demos](https://healeycodes.com/compressing-cs2-demos)
- [Demo parser for Counter-Strike 2](https://github.com/LaihoE/demoparser)