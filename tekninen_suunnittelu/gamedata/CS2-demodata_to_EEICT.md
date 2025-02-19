# CS2 demodatan siirto EEICT-sovellukseen

Backendin rakenne kuvattu kehittäjän näkökulmasta.
## Arkkitehtuurikuvaus

```mermaid

%%{init: {"flowchart": {"defaultRenderer": "elk"}} }%%

flowchart LR

    A@{ shape: doc, label: "$demodata.DEM"}
    A --> bck

    subgraph bck [**BACKEND**]
        direction TB
        B("Demoinfocs-golang (parser)")
        B --> C
        C@{ shape: doc, label: "$demodata.JSON"}
        C --> D
        D("Interval-transmitter (JSON)") 
        D --> E
    end

    E("Websocket-server")
    bck <--"websocket"--> F
    F{{"EEICT (Unity/C#)"}}

```

## Komponenttien vastuut

#### Demoinfocs-golang
- Vastuu: CS2 demodata --> JSON -parseri.
- https://github.com/markus-wa/demoinfocs-golang
- Tallentaa JSON-datan saman nimiseen tiedostoon.
- Mahdollisuus määrittää JSON-tiedostoon tulevien tapahtumien määrä/sekunti.
    - CS2 tukee maksimissaan 64/128 tapahtumaa/sekunti, riippuen CS2 palvelimen asetuksista.
#### Interval-transmitter
- Vastuu: pilkkoo JSON-datan EEICT-sovellukselle siirtoa varten.
- Lukee parserilta saadun JSON-datan muistiin ja siirtää sen objekti kerrallaan, määrätyllä intervallilla, eteenpäin.
#### Websocket-server
- Vastuu: muodostaa websocket protokollaa hyödyntäen yhteyden backendin ja EEICT-sovelluksen välille.