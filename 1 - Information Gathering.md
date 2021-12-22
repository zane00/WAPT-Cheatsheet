# Information Gathering
Information Gathering è la prima fase di un penetration test. L'obiettivo di questa fase è quello di ricercare quante più informazioni possibili riguardanti l'applicazione così da riuscire a individuare le tecnologie in utilizzo e riuscire a comprendere la trasmissione dei dati durante l'utilizzo.

Le informazioni spesso sono recuperabili direttamente (all'interno dell'applicazione) o indirettamente (tramite browser web).

## 1.1 Conduct Search Engine Discovery Reconnaissance for Information Leakage

**WSTG v4.2:** `WSTG-INFO-01`

Utilizza il browser web per la ricerca di documenti e/o informazioni sensibili riguardo l'applicazione in esame.

```
site:<target domain>
site:<target domain> inurl:robots.txt | inurl:sitemap.xml
site:<target domain> filetype:pdf | filetype:doc | filetype:docx | filetype:csv | filetype:xlsx | filetype:log
site:<target domain> intext:password | intext:admin | intext:username
site:<target domain> intitle:"index of /" intext:"resource/"
site:<target domain> inurl:"*admin | login" | inurl:.php | .asp
cache:<target domain>
```

## 1.2 Fingerprint Web Server

**WSTG v4.2:** `WSTG-INFO-02`

Web Server fingeriprinting consiste nell'individuare il tipo e la versione del web server che contiene l'applicazione. Server che utilizzano versioni vecchie potrebbero essere vulnerabili ad exploit conosciuti.

Intercetta le richieste con un proxy (Burp Proxy o ZAP Proxy) e verifica il campo `Server:` all'interno dell'header della risposta.
Se il campo è nascosto fai attenzione all'ordine con cui vengono riportate le informazioni dell'header: dall'ordine potrebbe essere comprensibile il servizio utilizzato.

Questa tecnica è conosciuta come **banner grabbing**.

Utilizza `telnet` per le richieste HTTP o `openssl` per le richieste HTTPS.

Connessione HTTP:
```
nc <target domain> 80
HEAD/HTTP/1.0
```

```
nc <target domain> 80
GET / SANTA CLAUS/1.1
```

Connessione HTTPS:
```
openssl s_client -connect <target domain>:443
```

Fingerprinting:
```
httprint -P0 -h <target ip> -s /usr/share/httprint/signatures.txt
nikto -h <target ip>
nmap -sV -O -p80,443 <target ip>
```

## 1.3 Review Webserver Metafiles for Information Leakage
**WSTG v4.2:** `WSTG-INFO-03`

```
curl -O -Ss https://<target domain>/robots.txt && cat robots.txt
curl -O -Ss https://<target domain>/sitemap.xml && cat sitemap.xml
```
Controlla tutti i tag `<meta>` all'interno della pagina.

## 1.4 Enumerate Applications on Webserver
**WSTG v4.2:** `WSTG-INFO-04`

```
nmap -sV -O -p80,443 <target ip>
```

## 1.5 Review Webpage Content for Information Leakage
**WSTG v4.2:** `WSTG-INFO-05`

Controlla il codice sorgente dell'applicazione per ricercare commenti che possono essere stati lasciati dai programmatori.

Controlla gli script in JavaScript.

## 1.6 Identify Application Entry Points
**WSTG v4.2:** `WSTG-INFO-06`

Enumera l'applicazione per identificare le superfici di attacco.

Annota e controlla tutti i parametri passati all'interno delle chiamate GET o POST.

## 1.7 Map Execution Paths Through Application
**WSTG v4.2:** `WSTG-INFO-07`

## 1.8 Fingerprinting Web Application Framework
**WSTG v4.2:** `WSTG-INFO-08`

```
whatweb <target domain>
```

Wappalyzer browser plugin.

## 1.9 Fingerprinting Web Application
**WSTG v4.2:** `WSTG-INFO-09`

## 1.10 Map Application Architecture
**WSTG v4.2:** `WSTG-INFO-10`



