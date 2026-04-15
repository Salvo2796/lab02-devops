# Evidence LAB01

## 1. Obiettivo compreso
Spiego con parole mie il flusso:
Preparo la repository con i file su Github
- Collego AzureDevOps attraverso Service Connection sia a Github sia a Azure
- In Azure ho creato ResourceGroup e AZRegistry
- Creo il mio Pool Agent, in questo caso sarà Self-Host
- Creo la Pipeline su AzureDevOps, prendendo il file yaml che ho in GitHub
- Avvio la Pipeline
- Crea immagine Docker e la pusha su AZRegistry
- Se tutto va bene, crea l'ACI in Azure

## 2. Repository GitHub
Indico il nome del repository usato e il branch principale.
Nome repository: app-v3-devops
Branch: main
## 3. Azure DevOps
Indico:
- nome del progetto Azure DevOps: Observability-DevOps
- nome della pipeline: Salvo2796.app-v3-devops
- tipo di connessione usata verso GitHub: Salvo2796
- nome della Azure Resource Manager service connection: sc-obs-azure-rg

## 4. Build e push immagine
- docker build -t obsacr11696.azurecr.io/obsapp-v3:41 .

- docker push obsacr11696.azurecr.io/obsapp-v3:41

## 5. Deploy su ACI
Indico:
- Resource Group: rg-observability-dev
- nome del container: obsapp-v3-aci
- FQDN finale: http://obsappv3-11696.westeurope.azurecontainer.io:8000

## 6. Test applicativi
Incollo l’output di:
- GET / {"app":"obsapp","status":"running","timestamp":"2026-04-14T10:30:07.078450+00:00","version":"v3"}
- GET /health -> {"status":"ok","timestamp":"2026-04-14T10:29:22.735364+00:00"}
- GET /time -> {"time":"2026-04-14T10:29:38.253478+00:00"}
- POST /echo -> {"received":{"lab":"01"},"status":200}
- GET /error -> {"error":"simulated_error","status":500}

## 7. Log del container
10.92.0.9 - - [14/Apr/2026 10:30:07] "GET / HTTP/1.1" 200 -
{"timestamp": "2026-04-14T10:30:07.078551+00:00", "level": "INFO", "message": "request_completed", "request_id": "bd7a6697-f716-42ee-b908-9f1cf299f295", "method": "GET", "path": "/", "status": 200, "latency_ms": 0.14, "client_ip": "10.92.0.9", "user_agent": "curl/8.5.0"}

## 8. Problemi incontrati
Creazione di Agent Pool(self-hosted) e comando Azure create. Vedi file Troubleshooting