# CS2 demodatan siirto EEICT-sovellukseen (WIP)

Backendin sovellusarkkitehtuuri kuvattuna kehittäjän näkökulmasta.
## Arkkitehtuurikuvaus

```mermaid

%%{init: {"flowchart": {"defaultRenderer": "elk"}} }%%

flowchart TB

    dem@{ shape: doc, label: "$demodata.DEM"}
    dem --> parser

    direction TB
    orch("Orchestrator")
    orch -.-> parser
    orch -.-> server
    
    subgraph parser ["**Demodata parser**"]
        worker("Worker")
        worker <--"subprocess.run"--> B
        B("DemodataToJSON-parser (Demoinfocs-golang)")
    end

    parser --- json
    json@{ shape: doc, label: "$demodata.JSON"}
    json --> server

    subgraph server ["**Demodata Server**"]
        direction LR
        D("JSONchopper") 
        D --> E
        E("Websocket-server (Sanic)")
    end
    
    server <--"Websocket"--> eeict
    eeict{{"EEICT (Unity/C#)"}}
    
```

## Komponenttien vastuut

##### Orchestrator
- Vastuu: demodatan siirtymisestä eri komponenttien välillä.
- Ohjelmointikieli: Python

### Demodata parser
##### Worker
- Vastuu: demodatan siirtymisestä parserin läpi
- Ohjelmointikieli: Python
##### DemodataToJSON-parser
- Vastuu: CS2 \*.dem --> JSON -parseri.
- Ohjelmointikieli: Go
- Demoinfocs-golang: https://github.com/markus-wa/demoinfocs-golang
- Tallentaa JSON-datan saman nimiseen tiedostoon kuin lähdetiedosto.
- Mahdollisuus määrittää JSON-tiedostoon tulevien tapahtumien määrä/sekunti.
    - CS2 tukee enimmillään 64/128 tapahtumaa/sekunti, riippuen CS2 palvelimen asetuksista.
    - Kirjoittaa asetetun intervallin JSON-tiedostoon.

### Demodata Server
##### JSONchopper
- Vastuu: pilkkoo JSON-datan EEICT-sovellukselle siirtoa varten.
- Ohjelmointikieli: Python
- Lukee parserilta saadun JSON-datan ja intervallin muistiin.
- Siirtää JSON-datan objekti kerrallaan, aiemmin määritetyllä tapahtumaa/sekunti intervallilla, eteenpäin seuraavalle komponentille.
##### Websocket-server
- Vastuu: muodostaa websocket protokollaa hyödyntäen yhteyden backendin ja EEICT-sovelluksen välille.
- Ohjelmointikieli: Python
- Sanic: https://sanic.dev/en/

