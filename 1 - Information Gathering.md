# Information Gathering
Information Gathering è la prima fase di un penetration test. L'obiettivo di questa fase è quello di ricercare quante più informazioni possibili riguardanti l'applicazione così da riuscire a individuare le tecnologie in utilizzo e riuscire a comprendere la trasmissione dei dati durante l'utilizzo.

Le informazioni spesso sono recuperabili direttamente (all'interno dell'applicazione) o indirettamente (tramite browser web).

## 1.1 Conduct Search Engine Discovery Reconnaissance for Information Leakage

**WSTG v4.2:** `WSTG-INFO-01`

```
site:domain.com
site:domain.com inurl:robots.txt | inurl:sitemap.xml
site:domain.com filetype:pdf | filetype:doc | filetype:docx | filetype:csv | filetype:xlsx | filetype:log
site:domain.com intext:password | intext:admin | intext:username
site:domain.com intitle:"index of /" intext:"resource/"
site:domain.com inurl:"*admin | login" | inurl:.php | .asp
cache:domain.com
```

#### Riferimenti Utili

[Google Hacking Database](https://www.exploit-db.com/google-hacking-database)

## 1.2 Fingerprint Web Server

**WSTG v4.2:** `WSTG-INFO-02`

Intercetta le richieste con un proxy (`Burp Proxy` o `ZAP Proxy`) e verifica il campo `Server: ` nell'header della risposta.
Se il campo è nascosto fai attenzione all'ordine con cui vengono riportate le informazioni.

Utilizza `telnet` per le richieste HTTP o `openssl` per le richieste HTTPS.

Connessione HTTP:
```
nc domain.com 80
HEAD/HTTP/1.0
```

Connessione HTTPS:
```
openssl s_client -connect domain.com:443
```

Fingerprinting:
```
httprint -P0 -h domain.com -s /usr/share/httprint/signatures.txt
nikto -h domain.com
nmap -sV -sC -O -p80,443 domain.com
```

## 1.3 Review Webserver Metafiles for Information Leakage
```
curl -O -Ss https://domain.com/robots.txt && head -n5 robots.txt
curl -O -Ss https://domain.com/sitemap.xml && head -n8 sitemap.xml
```
Controlla tutti i tag `<meta>` all'interno della pagina.

## 1.4 Enumerate Applications on Webserver


