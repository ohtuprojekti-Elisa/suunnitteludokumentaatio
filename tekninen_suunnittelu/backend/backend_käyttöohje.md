## Backendin käyttöohje (16.03.2025)

Ohjeet backendin käyttöön (WIP).
### Huomiot

Jotta backend puolella asiat rullaavat huoletta, tulee ottaa huomioon, että demodata-parseri täytyy kääntää uudelleen jokaisen siihen tehdyn muutoksen jälkeen. Vanhaa `demoparser.so` tiedostoa ei tarvitse poistaa, kääntäjä ylikirjoittaa sen.

Vanhalla parserilla käännetyt `demodata.json` tiedostot taas täytyy poistaa ja kääntää tarvittaessa uudelleen, jotta parseriin tehdyt json-datan tulostukseen liittyvät muutokset saadaan myös niihin.
### Parserin kääntäminen

Go-kielellä ohjelmoitu demodata-parseri on käännettävä ennen käyttöönottoa. Tässä on ohjeet Windows ympäristöä varten (Linux tulee myöhemmin).
#### Windows

1. Lataa ja asenna viimeisin versio Go\:sta (64Bit): https://go.dev/doc/install
    - Toimintaan saattamiseksi tarvitaan mahdollisesti uudelleenkäynnistys/-kirjautuminen.
2. Lataa viimeisin versio `mingw-w64` -kääntäjästä: https://github.com/niXman/mingw-builds-binaries/releases
    1. Suositeltu: `x86_64-$VERSION-release-win32-seh-ucrt-rt_v12-rev$.7z```
    2. Pura ladattu paketti ja siirrän sen sisältä löytyvä `mingw64` kansio johonkin valitsemaasi paikkaan.
    3. Lisää `mingw64/bin` kansion sijainti käyttöjärjestelmän ympäristömuutujaan `PATH`.
        - https://stackoverflow.com/questions/44272416/how-to-add-a-folder-to-path-environment-variable-in-windows-10-with-screensho
        - Ympäristömuuttujan toimintaan saattamiseksi, tarvitaan mahdollisesti uudelleenkäynnistys/-kirjautuminen.
    4. Testaa, että GCC löytyy suorittamalla se valitsemassasi konsolissa komennolla `gcc`.
3. Navigoi konsolilla parserin kansioon `./backend/demodata_parser/`.
4. Käännä parseri komennolla `python build.py`.
5. Sormet ristiin.
6. Jos ja kun parseri on käännetty, löytyy tiedosto `demoparser.so` kansiosta `./backend/demodata_parser/`.

### Prosessin käynnistäminen

Kuinka saada "linjasto rullaamaan".
#### Windows

1. Sijoita valitsemasi demotiedosto kansioon `./backend/demofiles/`.
2. Käynnistä Powershell tai CMD.
3. Navigoi `./backend/` kansioon.
4. Luo uusi Python Virtual Environment (venv) komennolla: `python -m venv .venv`.
5. Aktivoi venv komennolla:
  - Powershell: `.venv\Scripts\activate.ps1`
  - CMD: `.venv\Scripts\activate`
    - Jos venv:n aktivointi onnistui, pitäisi komentorivillä näkyä (Powershell) `(.venv) PS {hakemisto}`
6. Asenna riippuvuudet komennolla `pip install -r requirements.txt`.
7. Vihdoin voit käynnistää EEICT-backendin `./backend/` kansiosta komennolla `python eeict.py -f demotiedosto.dem`
8. Kun WebSocket-palvelin on käynnissä ja odottaa uutta yhteyttä, voit yhdistää siihen EEICT-sovelluksella.

Jos jokin kohta ei toiminut tai jäit jumiin, niin kysy `#backend` kanavalla apuja.

## Toteutuksen komennot ja niiden esimerkit

Toteutus ottaa syötteenä minkä tahansa CS2:n tuottaman .dem päätteisen tiedoston ($).
#### Help

Tätä kautta saa ajankohtaisimman listauksen ominaisuuksista.

```sh
python eeict.py -h
```

#### File

Syöttää toteutukselle annetun CS2-demotiedoston.

```sh
python eeict.py -f $.dem
```

#### Loop

Toistaa demotiedostoa silmukassa, kunnes backendin prosessi lopetetaan.

```sh
python eeict.py -l -f $.dem
```

#### Overwrite

Ylikirjoittaa syötetyn CS2-demotiedoston nimeen sidotut ja aiemin parsitut .json tiedostot. Tämä on tarpeellista esimerkiksi uuden parseriversion kääntämisen jälkeen. On myös mahdollista, että ylikirjoittamisen käyttäminen ratkoo myös backendin toimintaan liittyviä ongelmia, jos niitä kehitystyön aikana ilmenee.

```sh
python eeict.py -o -f $.dem
```
