# VolleyFit — Guida alla pubblicazione come App Installabile (PWA)

Questa cartella contiene VolleyFit trasformata in una **Progressive Web App (PWA)**:
un sito web che può essere **installato su telefono, tablet o computer** come
una vera app (icona sulla home, si apre a schermo intero, funziona offline),
pur restando **gestibile e aggiornabile da web** semplicemente ricaricando i file
sul server — non serve pubblicarla su App Store o Google Play.

## Cosa c'è nella cartella
```
index.html      → l'app (HTML+CSS+JS, un unico file)
manifest.json   → metadati dell'app (nome, icone, colori) richiesti per l'installazione
sw.js           → service worker: abilita l'installazione e il funzionamento offline
icons/          → icone dell'app in varie dimensioni
```

## ⚠️ Requisito fondamentale: serve un hosting HTTPS
L'installazione **non funziona** se apri semplicemente `index.html` con doppio
click dal computer (protocollo `file://`). I browser richiedono che una PWA sia
servita via **HTTPS** da un dominio vero (anche gratuito) perché il service
worker possa registrarsi.

### Il modo più veloce e gratuito: Netlify Drop
1. Vai su **https://app.netlify.com/drop**
2. Trascina l'intera cartella (con dentro `index.html`, `manifest.json`, `sw.js`, `icons/`) nella pagina
3. In pochi secondi ottieni un link pubblico HTTPS (es. `nome-a-caso.netlify.app`)
4. Apri quel link da telefono → il browser proporrà "Installa app" / "Aggiungi a schermata Home"

Altre alternative altrettanto valide e gratuite: **GitHub Pages**, **Vercel**, **Firebase Hosting**, **Cloudflare Pages**.

### Come si "gestisce da web" dopo la pubblicazione
Ogni volta che vuoi aggiornare l'app (nuovi programmi via account admin li vedi
già in automatico — quello è dati, non codice), se invece modifichi il codice
(`index.html`) ti basta ricaricare la nuova versione sullo stesso hosting: chi
ha già installato l'app riceve l'aggiornamento automaticamente al prossimo
avvio (il service worker scarica la nuova versione in background).

## Come si installa (una volta online)
- **Android (Chrome):** appare un banner automatico "Aggiungi a schermata Home", oppure menu ⋮ → "Installa app". C'è anche un pulsante ⬇ dedicato nell'header dell'app.
- **iPhone/iPad (Safari):** Safari non mostra prompt automatici — nell'app trovi lo stesso pulsante ⬇ che apre le istruzioni passo-passo (Condividi → Aggiungi a Home).
- **Desktop (Chrome/Edge):** icona di installazione nella barra degli indirizzi, o lo stesso pulsante ⬇ nell'header.

## Limite importante da conoscere: i dati restano sul dispositivo
Questa versione usa ancora lo storage "demo" (`localStorage` del browser) come
database, spiegato nelle risposte precedenti. Questo significa che, una volta
pubblicata online:
- ogni dispositivo/browser ha **il proprio storage separato**
- un allenatore che assegna un programma da telefono **non** lo vedrà comparire
  sul telefono dell'atleta, perché sono due storage locali diversi (a differenza
  di quando provate l'app insieme dentro Claude, dove lo storage è condiviso)
- i dati non sono sincronizzati né sono al sicuro da eventuali "svuota dati" del browser

**Per un'app realmente multi-utente e multi-dispositivo serve il backend vero**
descritto in `schema.sql` (database + autenticazione reale + API), che sostituirebbe
`localStorage` con chiamate a un server. È il passo successivo naturale una volta
che il prototipo PWA vi convince nell'aspetto e nel flusso.
