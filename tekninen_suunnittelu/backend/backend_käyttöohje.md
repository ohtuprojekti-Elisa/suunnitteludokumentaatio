## Backendin käyttöohje (12.03.2025)

Ohjeet backendin käyttöön (WIP).
### Huomiot

Jotta backend puolella asiat rullaavat huoletta, tulee ottaa huomioon, että demodata-parseri täytyy kääntää uudelleen jokaisen siihen tehdyn muutoksen jälkeen. Vanhaa `demoparser.so` tiedostoa ei tarvitse poistaa, kääntäjä ylikirjoittaa sen.

Vanhalla parserilla käännetyt `demodata.json` tiedostot taas täytyy poistaa ja kääntää tarvittaessa uudelleen, jotta parseriin tehdyt json-datan tulostukseen liittyvät muutokset saadaan myös niihin.
### Parserin kääntäminen

Go-kielellä ohjelmoitu demodata-parseri on käännettävä ennen käyttöönottoa. Tässä on ohjeet Windows ympäristöä varten (Linux tulee myöhemmin).
#### Windows

1. Lataa viimeisin versio `mingw-w64` -kääntäjästä: https://github.com/niXman/mingw-builds-binaries/releases
    1. Suositeltu: `x86_64-14.2.0-release-win32-seh-ucrt-rt_v12-rev1.7z`
    2. Pura ladattu paketti ja siirrän sen sisältä löytyvä `mingw64` kansio johonkin valitsemaasi paikkaan.
    3. Lisää `mingw64/bin` kansion sijainti käyttöjärjestelmän ympäristömuutujaan `PATH`.
        - https://stackoverflow.com/questions/44272416/how-to-add-a-folder-to-path-environment-variable-in-windows-10-with-screensho
        - Ympäristömuuttujan toimintaan saattamiseksi, tarvitaan mahdollisesti uudelleenkäynnistys/-kirjautuminen.
    4. Testaa, että GCC löytyy suorittamalla se valitsemassasi konsolissa komennolla `gcc`.
2. Navigoi konsolilla parserin kansioon `./backend/demodata_parser/`.
3. Käännä parseri komennolla `python build.py`.
4. Sormet ristiin.
5. Jos ja kun parseri on käännetty, löytyy tiedosto `demoparser.so` kansiosta `./backend/demodata_parser/`.

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
7. Vihdoi, käynnistä EEICT-backend `./backend/` kansiosta komennolla `python eeict.py -f demotiedosto.dem`
8. Kun WebSocket-palvelin on käynnissä ja odottaa uutta yhteyttä, voit yhdistää siihen EEICT-sovelluksella.

Jos jokin kohta ei toiminut tai jäit jumiin, niin kysy `#backend` kanavalla apuja.
