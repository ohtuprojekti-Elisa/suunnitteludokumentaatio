# Pelidata: GSI vs. Demo

Ajatuksia pohdittavaksi ennen seuraavaa asiakaspalaveria.
## Sanasto

- Alkuperäinen palvelin = CS2 tai muu taho, joka lähettää pelidataa.  
- Käsittelypalvelin = Meidän toteutuksemme pelidatan käsittelyyn ja suoratoistoon loppukäyttäjälle.  
- Sovellus = Loppukäyttäjän operoima lisätyn todellisuuden sovellus.
## GSI-data

#### Hyödyt

- Käsittelypalvelimen rakentaminen on melko yksinkertaista.
- Yksi dataruutu on hyvin kevyt (~20KB).
- Data voidaan suoratoistaa reaaliaikaisesti alkuperäiseltä palvelimelta käsittelypalvelimelle.

#### Haitat

- Päivitysväli on vain 1/3 sekuntia (~333ms).
- Ei sisällä kaikkia mahdollisia pelaajan tiloja: seisominen/kyykky, juokseminen/käveleminen, ym.
- Yhteys alkuperäisen ja käsittelypalvelimen välillä tapahtuu REST API:n kautta, mikä ei ole kovin tehokas reaaliaikaiseen tiedonsiirtoon.

## Demo-data

#### Hyödyt

- Päivitysväli on 1/64 sekuntia (~15ms).
- Sisältää kaikki mahdolliset pelaajan ja maailman tilat.
- "Tiheämpi" data mahdollistaa sovelluksen loppukäyttäjälle näytettävät sulavammat ja tarkemmat animaatiot.

#### Haitat

- Yhden ottelun datan tiedostokoko on melko suuri (>500MB).
- Dataa täytyy käsitellä (parse) perusteellisemmin ennen lähettämistä sovellukseen.
- Vaatii enemmän verkkolinkiltä enemmän kaistanleveyttä, joten tämä asettaa alkuperäisen ja käsittelypalvelimen väliselle linkille suuremmat vaatimukset.
- Demo-tiedostoja ei voida suoratoistaa reaaliaikaisesti alkuperäiseltä palvelimelta, ellei sen rinnalle luoda erillistä palvelinsovellusta (onko varmasti näin?).

## Linkit

- [https://healeycodes.com/rendering-counter-strike-demos-in-the-browser](https://healeycodes.com/rendering-counter-strike-demos-in-the-browser)
- [https://github.com/markus-wa/demoinfocs-golang](https://github.com/markus-wa/demoinfocs-golang)
- [https://github.com/pnxenopoulos/awpy](https://github.com/pnxenopoulos/awpy)
- [https://www.charlieintel.com/counter-strike/what-is-sub-tick-in-cs2-servers-276995/](https://www.charlieintel.com/counter-strike/what-is-sub-tick-in-cs2-servers-276995/)