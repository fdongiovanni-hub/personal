# Passive reconnaissance

Gather information never directly attacking the target, only using public registers and OSINT instruments.

🧰 1. Gli strumenti per imparare in autonomia

Per non impazzire con i manuali enciclopedici (man), hai scoperto tre scorciatoie fondamentali per capire la logica dei comandi al volo:
• L'opzione --help o -h: Mostra una guida rapida con le opzioni principali direttamente nel terminale.
	• Sintassi: nome_comando --help (es. jq --help)
• tldr: Il bignami della community che sostituisce il manuale con i 5 esempi pratici più usati dagli umani.
	• Sintassi: tldr nome_comando (es. tldr jq)
• cheat.sh: Un cheat sheet online consultabile via terminale senza installare nulla.
	• Sintassi: curl cheat.sh/nome_comando (es. curl cheat.sh/jq)

🌐 2. Comandi di Rete e Ricerca DNS

Il DNS è l'elenco telefonico di internet. Hai imparato a interrogarlo per mappare l'infrastruttura di un obiettivo:

Tipo Record	Cosa contiene?	Utilità per l'Hacker / Difensore
A	Indirizzo IPv4 (i numeri del server)	Trovare la macchina fisica su cui gira il sito.
AAAA	Indirizzo IPv6 (versione moderna dell'IP)	Stessa utilità dell'IP, ma per reti moderne.
MX	Server di gestione delle Email	Capire la piattaforma usata (es. se vedi outlook.com sai che usano Office 365, ottimo per il phishing).
TXT	Testo libero (note e configurazioni)	Spesso usato nei laboratori per nascondere le Flag segrete.


Sintassi dei comandi DNS:

• dig (Consigliato): Cerca i record in modo pulito ed efficiente.
	• Sintassi generica: dig dominio.com TIPO_RECORD
	• Esempio per Flag TXT: dig thmlabs.com TXT
	• Forzare un server specifico (es. Cloudflare): dig @1.1.1.1 tryhackme.com A
	• Risposta secca senza fronzoli: Aggiungi +short alla fine.
• nslookup (Alternativa):
	• Sintassi generica: nslookup -type=TIPO_RECORD dominio.com
	• Esempio per Email: nslookup -type=MX disney.com
	• Interrogare direttamente il Name Server ufficiale (Risposta Autorevole): nslookup -type=A disney.com ns5.twdcns.co.uk

🗂️ 3. Analisi e Filtro dei Dati (JSON e WHOIS)

Internet si sta modernizzando: il vecchio protocollo di testo WHOIS (porta 43) [RFC 3912] è stato sostituito da RDAP (porta 443/HTTPS), che risponde in formato JSON.
• nc (Netcat): È un tubo vuoto per testo grezzo. Ti ha permesso di capire che i server web (porte 80/443) e i server WHOIS (porta 43) sono macchine separate [RFC 3912]. Se bussi alla porta sbagliata, ti sbattono la porta in faccia.
	• Scansione rapida di una porta: nc -zv dominio.com NUMERO_PORTA (la -z fa solo il test senza inviare dati, la -v spiega cosa succede).
	• Parlare a mano con il WHOIS: nc ://verisign-grs.com 43 e poi premere INVIO dopo aver scritto il dominio (es. disney.com) [RFC 3912].
• curl: È un assistente intelligente che gestisce da solo i protocolli complessi e la crittografia dell'HTTPS.
	• Sintassi: curl -s URL (la -s nasconde la barra di caricamento).
• jq: Il chirurgo dei file JSON. Ragiona a sentieri (punti .) e serve a ripulire il "casino" di dati grezzi rimettendoli in ordine o filtrandoli.
	• Sintassi per vedere le categorie principali: curl -s URL | jq 'keys'
	• Sintassi per estrarre una chiave esatta (occhio a maiuscole e plurali!): curl -s URL | jq '.nome_chiave' (es. .events o .ldhName).
	• Sintassi per scorrere una lista: Usa le parentesi quadre [] (es. .nameservers[].ldhName).

🕵️ 4. Piattaforme OSINT Esterne

Hai capito come sfruttare l'intelligence collettiva tramite database pubblici:
• crt.sh (e l'alternativa ctlogs.dev): Spiano i registri dei certificati HTTPS per scoprire tutti i sottodomini storici e nascosti di un'azienda.
• DNSDumpster: Genera la mappa visuale di tutti i server e sottodomini, mostrandoti il Fingerprinting/Banner (le "impronte digitali") delle tecnologie usate (es. quante macchine usano Nginx, quante Cloudflare, ecc.). Le versioni esatte dei banner servono agli hacker per cercare vulnerabilità note (CVE).
• Shodan.io: Il motore di ricerca dei dispositivi connessi. Tramite la Faceted Search (la colonna dei filtri a sinistra) ti permette di vedere le statistiche globali (quali sono i Paesi con più server Apache o le porte più usate) usando filtri come product:"nginx".

🔒 5. Privacy del Traffico (Come non farsi spiare)

Infine, hai capito come proteggere la tua navigazione dall'amministratore del Wi-Fi del dormitorio o dal router di casa:
1. DoH / DoT (DNS criptato): Criptano la richiesta del nome del sito (es. dentro Chrome/Firefox usando il server 1.1.1.1), impedendo al router di registrare la lista dei siti che cerchi.
2. VPN / Tor Browser: La protezione totale. Criptano sia i DNS sia gli indirizzi IP dei server con cui parli, rendendoti un muro nero impenetrabile per chi gestisce il Wi-Fi locale.
Questo è il tuo arsenale completo per la ricognizione! Se hai salvato questi appunti sul tuo computer, sei pronto a procedere. Vuoi che iniziamo una nuova stanza di TryHackMe (magari sulla Scansione delle Reti con Nmap) o c'è un comando di questo riassunto che vuoi testare ancora?
