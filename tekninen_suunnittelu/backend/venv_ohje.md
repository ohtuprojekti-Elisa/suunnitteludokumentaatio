# venv ohje

- Siirrä ladattu demodata.json `./backend/demodata_server/data/` kansioon.
- Käynnistä Powershell tai CMD.
- Navigoi `./backend/` kansioon.
- Luo uusi Python Virtual Environment (venv) komennolla: `python -m venv .venv`.
- Aktivoi venv komennolla:
  - Powershell: `.venv\Scripts\activate.ps1`
  - CMD: `.venv\Scripts\activate`
- Jos venv:n aktivoin onnistui, pitäisi komentorivillä näkyä (Powershell) `(.venv) PS {hakemisto}`
- Asenna riippuvuudet komennolla `pip install -r requirements.txt`.
- Navigoi kansioon `./backend/demodata_server/`.
- Käynnistä serveri komennolla: `python ws_server.py`.
- Jos käynnistys onnistui, pitäisi komentoriville tulla siitä ilmoitus.
  - Jos ei, niin kysy `#backend` kanavalla lisää.
