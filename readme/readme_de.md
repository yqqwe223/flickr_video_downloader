# Flickr Video Downloader 📥

> Leichtgewichtiges Web-Tool, um öffentliche Flickr-Videos lokal zu speichern. Ohne Schnickschnack, ohne Tracking, funktioniert einfach.

## 👋 Warum gibt's das hier?

Mal ehrlich: Manchmal stöbert man auf Flickr und findet ein Video, das man wirklich brauchen kann — ein Foto-Tutorial, ein Behind-the-Scenes-Clip, oder sogar etwas, das man selbst hochgeladen hat und jetzt backuppen möchte. Aber es einfach runterladen? Nicht immer trivial.

Deshalb habe ich dieses Tool geschrieben, erst für mich, dann dachte ich: "Warum nicht teilen?". Keine überflüssigen Features, kein User-Tracking, kein erzwungenes Account-Setup. Einfach einen öffentlichen Flickr-Link einfügen, auf "Analysieren" klicken, und wenn das Video zugänglich ist, erscheinen die Download-Optionen. Mehr nicht.

Die gesamte Verarbeitung läuft serverseitig: Ich protokolliere nicht, was du lädst, speichere keine Historie, sammle keine personenbezogenen Daten. Deine Privatsphäre bleibt bei dir.

## ✨ Was es wirklich kann

- **Unterstützt gängige Flickr-Links**: Funktioniert mit öffentlichen Alben, Nutzer-Video-Seiten und direkten Freigabe-Links — solange sie nicht privat oder passwortgeschützt sind
- **Zeigt verfügbare Qualitäten**: Wenn Flickr mehrere Auflösungen anbietet (Original, Hoch, Standard), kannst du auswählen, welche du laden möchtest
- **Kein Login nötig**: Verarbeitet ausschließlich öffentlich zugängliche Inhalte; fragt niemals nach deinen Flickr-Zugangsdaten
- **Sauberer, responsiver Aufbau**: Sieht auf Smartphone, Tablet und Desktop gut aus, ohne schwere Frontend-Frameworks
- **Einfacher Rate-Limiting-Schutz**: Drosselt automatisch Anfragen pro IP, um Missbrauch zu verhindern und den Dienst für alle stabil zu halten
- **Non-blocking Verarbeitung**: Auch beim Analysieren längerer Videos friert dein Browser-Tab nicht ein, die Bedienung bleibt flüssig

## 🛠 Was steckt dahinter?

| Ebene | Technologien |
|-------|-------------|
| Backend | Python 3.11, Django 4.2 LTS |
| Parsing | httpx, lxml, Regex für Metadaten-Extraktion |
| Frontend | Semantisches HTML5, schlankes CSS3, Vanilla JavaScript |
| Deployment | Gunicorn + Nginx, Docker-kompatibel |
| Utilities | python-dotenv, django-ratelimit, whitenoise |

Null KI-Bibliotheken. Null externe API-Aufrufe, die "nach Hause funken". Nur klassische HTTP-Requests und sorgfältig geschriebenes HTML-Parsing — Code, den man tatsächlich lesen, verstehen und ohne Kopfzerbrechen anpassen kann.

## 🚀 Lokal zum Laufen bringen

### Was du brauchst
- Python 3.10 oder neuer
- pip + venv (oder virtualenv)
- Grundkenntnisse zu Django-Projektstrukturen

### Entwicklungsumgebung einrichten

```bash
# Repository klonen
git clone https://github.com/dein-username/flickr-downloader-ge.git
cd flickr-downloader-ge

# Virtuelle Umgebung erstellen und aktivieren
python -m venv .venv
source .venv/bin/activate  # Linux/macOS
# oder .venv\Scripts\activate  # Windows

# Abhängigkeiten installieren
pip install -r requirements.txt

# Umgebungsvariablen konfigurieren
cp .env.example .env
# .env mit deinen Werten bearbeiten (SECRET_KEY, DEBUG, etc.)

# Migrationen ausführen und Dev-Server starten
python manage.py migrate
python manage.py runserver
```

Danach `http://127.0.0.1:8000` im Browser öffnen.

### Hinweise für den Produktivbetrieb

- `DEBUG=False` setzen und `ALLOWED_HOSTS` korrekt mit deinen Domains konfigurieren
- Hinter Nginx mit Gunicorn (oder uWSGI, wenn preferred) betreiben
- HTTPS auf Proxy-Ebene aktivieren
- Statische Dateien sammeln: `python manage.py collectstatic`
- Bei steigendem Traffic Redis für Caching in Erwägung ziehen

Beispiel Gunicorn-Befehl:
```bash
gunicorn config.wsgi:application \
  --bind 127.0.0.1:8000 \
  --workers 2 \
  --timeout 90
```

Wenn du die Worker-Anzahl erhöhst, behalte den Speicherverbrauch im Auge — Video-Parsing kann etwas ressourcenintensiv sein.

## 📋 So wird's benutzt

1. Eine öffentliche Flickr-Seite mit Video suchen (Album, Nutzerprofil oder geteilter Link)
2. URL kopieren und in das Eingabefeld des Tools einfügen
3. Auf "Analysieren" klicken — der Backend extrahiert die verfügbaren Video-Streams
4. Bei Erfolg erscheinen Download-Buttons mit Auflösungsangaben
5. Gewünschte Option auswählen; der Download startet über den Browser

> Hinweis: Nur öffentlich zugängliche Videos funktionieren. Private Alben, nur-für-Freunde-Inhalte, passwortgeschützte Videos oder regional eingeschränkte Inhalte liefern einen Fehler zurück. Das ist Absicht — das Tool respektiert Flickrs Privatsphäre-Einstellungen.

## ⚠️ Bitte unbedingt lesen

Dieses Tool ist **ausschließlich für den privaten, nicht-kommerziellen Gebrauch** konzipiert. Beispiele für sinnvolle Nutzung:
- Eigene auf Flickr hochgeladene Videos sichern
- Öffentlich geteilte Bildungs- oder Referenzinhalte zum Offline-Lernen archivieren
- Forschung oder Barrierefreiheit im Rahmen von Fair Use

**Du bist verantwortlich für**:
- Die Einhaltung der [Flickr-Nutzungsbedingungen](https://www.flickr.com/help/terms)
- Die Achtung von Urheberrechten und Creative-Commons-Lizenzen der Ersteller
- Die Beachtung geltender Gesetze in deinem Land zum Umgang mit digitalen Inhalten

Ich überwache keine Downloads und übernehme keine Verantwortung bei Missbrauch. Bitte nicht verwenden für:
- Massenhaftes Scraping oder automatisierte Datenerfassung
- Weitergabe urheberrechtlich geschützter Inhalte ohne ausdrückliche Erlaubnis
- Umgehung von Privatsphäre-Einstellungen oder Zugriffskontrollen
- Kommerzielle Dienste oder Re-Hosting ohne vorherige Absprache

Wenn du unsicher bist, ob dein Use Case passt: Wahrscheinlich passt er nicht. Im Zweifel zuerst den Content-Ersteller fragen.

## 🤝 Lust, mitzumachen?

Bug gefunden? Parser könnte robuster sein? Idee für eine bessere UI? Beiträge sind willkommen — ohne Hürden.

### So kannst du beitragen
1. Repository forken und Feature-Branch anlegen (`git checkout -b fix/mobile-layout`)
2. Änderungen in kleinen, logischen Commits mit klaren Nachrichten umsetzen
3. Lokal testen — sicherstellen, dass bestehende Funktionen weiterhin laufen
4. Pull Request mit knapper, nachvollziehbarer Beschreibung öffnen

### Code-Stil-Empfehlungen
- Backend: PEP 8 einhalten, Type-Hints dort einsetzen, wo sie die Lesbarkeit erhöhen
- Frontend: JavaScript minimal halten; progressive Enhancement statt schwerer Frameworks
- Commits: Konventionelle Präfixe nutzen (`feat:`, `fix:`, `docs:`, `chore:`, etc.)

### Fehler melden
Bei einem Bug-Report bitte folgendes angeben:
- Die betroffene Flickr-URL (falls teilbar)
- Browser + Version, Betriebssystem, Gerätetyp
- Schritt-für-Schritt-Anleitung zur Reproduktion
- Erwartetes vs. tatsächliches Verhalten

Screenshots oder Console-Logs helfen besonders bei Frontend-Problemen.

## 🔧 Konfigurationsoptionen

| Variable | Zweck | Beispiel |
|----------|-------|----------|
| `DEBUG` | Django-Debug-Modus an/aus | `False` |
| `SECRET_KEY` | Django-Sicherheitsschlüssel | `deine-sichere-zufallszeichenkette` |
| `MAX_VIDEO_SIZE_MB` | Dateien über X MB ablehnen | `500` |
| `RATE_LIMIT_PER_MIN` | Max. Anfragen pro IP pro Minute | `10` |
| `ALLOWED_HOSTS` | Erlaubte Domains (kommagetrennt) | `.deinedomain.de` |

Alle Einstellungen werden über `python-dotenv` geladen; keine Secrets im Quellcode hardcoded. In Production die `SECRET_KEY` regelmäßig rotieren.

## 📄 Lizenz

MIT-Lizenz — vollständiger Text in [LICENSE](./LICENSE).  
Du darfst diese Software frei nutzen, ändern und weiterverteilen, solange der ursprüngliche Copyright-Hinweis erhalten bleibt.

## 📬 Kontakt & Support

- Fehlerberichte & Feature-Ideen: GitHub-Issues-Tab verwenden
- Allgemeine Fragen: support@twittervideodownloaderx.com
- Sicherheitslücken: Bitte vor öffentlicher Bekanntgabe direkt per E-Mail melden

Ich versuche, Issues innerhalb weniger Tage zu beantworten. Falls länger nichts kam: Einfach nochmal nachhaken — manchmal rutscht was durch.

---

*Dieses Projekt steht in keiner Verbindung zu Flickr / SmugMug, Inc., wird nicht von ihr unterstützt und ist nicht mit ihr assoziiert. Alle Marken, Logos und Inhaltsrechte liegen bei ihren jeweiligen Inhabern.*

*Letzte Aktualisierung: Mai  | Version 1.2.0*

*Live-Demo: https://twittervideodownloaderx.com/flickr_downloader_ge*

*Von einem Menschen für Menschen geschrieben. Keine KI war an der Erstellung dieses README oder des Codes beteiligt.*