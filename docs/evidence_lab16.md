# Evidence LAB16

## 1. Obiettivo compreso
Questa Pipeline è migliore perchè è divisa in satges(fasi), quindi durante l'esecuzione possiamo notare i vari passaggi che sta eseguendo ed eventualmente capire facilmente dove è avvenuto l'errore

## 2. Repository GitHub
Indico il nome del repository: lab02-devops
Branch: main

## 3. Nuova struttura pipeline
Descrivo i tre stage:
- Build: build image Docker e push sul Registry
- Deploy: creazione azure container istance 
- SmokeTest: test per verificare funzionamento degli endpoint

## 4. Tag immagine
Tag: 46
Corrisponde a numero unico della pipeline corrente.Noi lo usiamo come tag nell'image

## 5. Verifica stage Build
Viene costruita l'image Docker nell'agent pool e pushata nell'ACR

## 6. Verifica stage Deploy
Viene eliminato se è presente l'ACI precedente, dopodichè parte il comando Create

## 7. Verifica stage SmokeTest
Recupera indirizzoIP e Porta, esegue i curl per verificare che gli endpoint rispondano correttamente

## 8. Log del container
```bash
{"timestamp": "2026-04-15T07:59:09.858687+00:00", "level": "INFO", "message": "request_completed", "request_id": "770f10ca-7c09-439d-8244-93b8e92e7fd7", "method": "GET", "path": "/health", "status": 200, "latency_ms": 0.19, "client_ip": "10.92.0.10", "user_agent": "curl/8.5.0"}
10.92.0.10 - - [15/Apr/2026 07:59:09] "GET /health HTTP/1.1" 200 -
{"timestamp": "2026-04-15T07:59:10.035854+00:00", "level": "INFO", "message": "request_completed", "request_id": "d4195c26-5755-4c1b-83a3-a05e16ebfca2", "method": "GET", "path": "/", "status": 200, "latency_ms": 0.13, "client_ip": "10.92.0.10", "user_agent": "curl/8.5.0"}
10.92.0.10 - - [15/Apr/2026 07:59:10] "GET / HTTP/1.1" 200 -
{"timestamp": "2026-04-15T07:59:10.190949+00:00", "level": "INFO", "message": "request_completed", "request_id": "068c9cb8-b0b7-4bd5-a225-73b45ea760bd", "method": "GET", "path": "/time", "status": 200, "latency_ms": 0.13, "client_ip": "10.92.0.11", "user_agent": "curl/8.5.0"}
10.92.0.11 - - [15/Apr/2026 07:59:10] "GET /time HTTP/1.1" 200 -
{"timestamp": "2026-04-15T07:59:10.329605+00:00", "level": "INFO", "message": "request_completed", "request_id": "f01f34d3-013d-4472-b640-0195448e7106", "method": "POST", "path": "/echo", "status": 200, "latency_ms": 0.23, "client_ip": "10.92.0.11", "user_agent": "curl/8.5.0"}
10.92.0.11 - - [15/Apr/2026 07:59:10] "POST /echo HTTP/1.1" 200 -
```

## 9. Differenze rispetto al LAB01
- Divisione della Pipeline in più fasi
- Chiarezza maggiore sull'esecuzione della Pipeline
- Test automatici con i curl

## 10. Problemi incontrati
Ho recuperato i valori ACR_NAME, ACR_PASS, FQDN senza sporcature, ovvero senza eventuali /(a capo)