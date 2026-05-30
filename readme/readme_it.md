# Flickr Video Downloader - Documentazione del Progetto

> Strumento leggero per estrarre e salvare video pubblici da Flickr. Sviluppato per uso personale e studio tecnico. Si prega di rispettare i termini di servizio della piattaforma e le normative vigenti.

🔗 Prova online: [https://twittervideodownloaderx.com/flickr_downloader_it](https://twittervideodownloaderx.com/flickr_downloader_it)

---

## 📌 Perché l'ho creato?

Diciamocelo chiaro: quando navighi su Flickr alla ricerca di ispirazione visiva, ogni tanto ti imbatti in video davvero interessanti. Timelapse di paesaggi mozzafiato, dietro le quinte di sessioni fotografiche, mini-vlog di viaggio che raccontano più di una singola immagine... Il problema? Nessun pulsante di download in vista. Salvi il link nei preferiti con l'intenzione di "guardarlo dopo" e te ne dimentichi per mesi. Registrare lo schermo? La qualità ne risente, ci vuole un'eternità e il file finale pesa come un macigno.

Così mi sono detto: "E se mi creassi qualcosa di semplice, giusto per me, che faccia il lavoro senza troppi fronzoli?". Ed ecco nato questo progetto. Niente complicazioni, niente interfaccia sovraccarica, solo l'essenziale: incolli un link → ottieni il video. Il codice è pulito, le dipendenze sono ridotte al minimo e l'installazione non dovrebbe farti impazzire. Se può tornare utile anche a te, perfetto. Se vuoi dare un'occhiata al codice o proporre miglioramenti, ancora meglio.

---

## ✨ Cosa fa, nel concreto

- ✅ Estrae video pubblici da Flickr (supporta album incorporati, pagine profilo, link condivisi, ecc.)
- ✅ Rileva automaticamente le diverse qualità disponibili e privilegia la qualità originale quando possibile
- ✅ Tutta l'elaborazione pesante lato backend; il frontend è solo un form minimale → caricamento veloce, zero script di tracciamento
- ✅ CORS già configurato per un'integrazione fluida con altri progetti frontend
- ✅ Log base delle richieste + stato del parsing per un debug più efficiente
- ✅ Meccanismo di limitazione della frequenza integrato per ridurre i rischi di blocco da parte di Flickr

---

## 🛠️ Stack tecnologico

- Linguaggio: Python 3.9+
- Framework: Django 4.x (leggero, modulare, perfetto per questo tipo di progetto)
- Client HTTP: requests come principale, httpx opzionale per la modalità asincrona
- Parsing: regex + BeautifulSoup (solo quando strettamente necessario)
- Deploy: Gunicorn + Nginx raccomandati; supporto Docker per chi vuole fare in fretta
- Gestione config: variabili d'ambiente + settings.py diviso per ambiente (dev/prod)

Ho volutamente limitato le dipendenze esterne per evitare conflitti di versione e rendere l'installazione il più fluida possibile.

---

## 🚀 Avvio rapido

### Opzione 1: Da codice sorgente

```bash
# 1. Clonare il repo
git clone https://github.com/yourname/flickr-downloader.git
cd flickr-downloader

# 2. Creare un venv e installare le dipendenze
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt

# 3. Copiare e personalizzare la config
cp .env.example .env
# Modificare .env: inserire SECRET_KEY, ALLOWED_HOSTS, ecc.

# 4. Eseguire le migrazioni (se vuoi loggare su database)
python manage.py migrate

# 5. Avviare il server di sviluppo
python manage.py runserver 0.0.0.0:8000

# Per la produzione, usa Gunicorn:
gunicorn core.wsgi:application --bind 0.0.0.0:8000 --workers 3
```

### Opzione 2: Docker (per chi ha fretta)

```bash
# Build dell'immagine
docker build -t flickr-dl:latest .

# Avviare il container
docker run -d -p 8000:8000 --env-file .env flickr-dl:latest
```

> 💡 Consiglio da amico: in produzione, configura sempre Nginx come reverse proxy e forza l'HTTPS. La sicurezza non è un optional.

---

## 📋 Esempio di utilizzo dell'API

```bash
# Test veloce con curl
curl -X POST https://tuodominio.com/api/parse \
  -H "Content-Type: application/json" \
  -d '{"url": "https://www.flickr.com/photos/xxx/video/12345678"}'

# Risposta JSON tipica
{
  "code": 200,
  "data": {
    "title": "Timelapse alba sulle Dolomiti",
    "author": "Marco Fotografo",
    "video_url": "https://cdn.flickr.com/video/xxx.mp4",
    "thumbnail": "https://cdn.flickr.com/thumb/xxx.jpg",
    "duration": "04:12"
  }
}
```

Sull'interfaccia web, ho scelto volutamente il minimalismo: incolli il link → clicchi su "Analizza" → ottieni il pulsante di download. Tre click, tutto qui. Niente fronzoli, niente distrazioni.

---

## ⚠️ Da leggere assolutamente prima dell'uso

1. Questo strumento funziona solo con video **pubblici** su Flickr. I contenuti che richiedono login o impostati come privati non verranno elaborati;
2. Vietato usare questo script per scraping massivo, redistribuzione commerciale o qualsiasi azione contraria ai Termini di Servizio di Flickr;
3. I diritti sui video appartengono ai creatori originali o alla piattaforma. Usa i contenuti scaricati solo per uso personale, ricerca o citazioni ragionevoli;
4. Inviare troppe richieste in poco tempo può attivare meccanismi di protezione. Il codice include un sistema di delay base – attivalo, ti eviterà problemi;
5. Questo progetto NON archivia alcun file video. I link restituiti puntano direttamente al CDN ufficiale di Flickr e possono scadere in qualsiasi momento secondo le policy della piattaforma;
6. Lo sviluppatore declina ogni responsabilità legale o tecnica riguardo a problemi derivanti dall'uso di questo strumento. Lo utilizzi a tuo rischio e pericolo.

---

## 🤝 Vuoi contribuire?

Segnalazioni di bug, suggerimenti per miglioramenti, pull request: tutto è benvenuto. Giusto qualche piccola regola prima di inviare:

- Descrivi chiaramente come riprodurre il problema, con il link interessato e il messaggio di errore esatto;
- Le nuove funzionalità devono avere un'utilità generale – evita aggiustamenti troppo specifici al tuo caso personale;
- Rispetta lo stile di codice esistente (PEP8 + .editorconfig del progetto) per mantenere una base coerente;
- Se modifichi la logica di parsing, pensa ad aggiungere test per evitare di rompere ciò che funzionava prima.

Per piccole correzioni, una PR diretta basta e avanza. Per cambiamenti più importanti, apri prima una Issue così possiamo discutere l'approccio e risparmiare tempo a tutti.

---

## 📄 Licenza

MIT License ©   
Puoi usare, modificare e redistribuire questo codice liberamente, a condizione di mantenere l'attribuzione all'autore originale. Per uso commerciale, spetta a te verificare la conformità con le leggi e i regolamenti applicabili.

---

> 🌱 Per essere totalmente trasparente: ho sviluppato questo strumento principalmente per i miei bisogni personali, quindi non è perfetto e non pretende di esserlo. Se ti imbatti in errori "parse failed" o link che non funzionano più, è molto probabile che Flickr abbia modificato la struttura HTML o rafforzato le protezioni anti-bot. Apri una Issue e darò un'occhiata appena possibile – o se te la cavi con il codice, non esitare a proporre una correzione tu stesso. A volte, il modo migliore per capire davvero come funziona un sistema è sporcarsi le mani.  
>   
> Un'ultima cosa, e lo dico con sincerità: rispetta il lavoro dei creatori di contenuti e usa questo tipo di strumenti con giudizio. È l'unico modo per preservare progetti utili come questo e mantenerli accessibili a tutti. Grazie per aver preso il tempo di leggere questo README – spero che questo piccolo strumento ti faccia risparmiare tempo o ti aiuti nelle tue ricerche! 🙏✨

---

## 🔧 Risoluzione problemi rapidi

- **Il parsing si blocca all'improvviso**: Flickr probabilmente ha aggiornato il suo HTML. Controlla le Issue recenti o tira le ultime modifiche dal codice.
- **Errori 403/429**: Hai superato i limiti di richieste. Attiva il delay nella config o riduci il numero di richieste simultanee.
- **Il link video scade troppo in fretta**: È normale – gli URL del CDN Flickr hanno una durata di vita breve. Scarica rapidamente dopo il parsing.
- **Docker non parte**: Verifica la sintassi del tuo file .env e assicurati che la porta 8000 sia libera.

---

## 📦 Struttura del progetto (semplificata)

```
flickr-downloader/
├── core/               # Config principale Django
├── parser/             # Logica di estrazione video
├── static/             # Asset frontend minimali
├── templates/          # Template HTML
├── .env.example        # Modello di configurazione
├── requirements.txt    # Dipendenze Python
├── Dockerfile          # Istruzioni di build container
└── README.md           # Sei qui
```

Mantenere le cose semplici significa mantenerle manutenibili. È tutta la filosofia del progetto.

---

> Ultima chicca: se questo strumento ti è stato utile, bene. Se l'hai migliorato, ancora meglio. Condividi ciò che impari, resta curioso, e buon codice a tutti. 🚀