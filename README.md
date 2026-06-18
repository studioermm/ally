# Ally — PWA (Progressive Web App)

Il tuo alleato quotidiano. Progetto React standard, esportabile, senza alcuna dipendenza da Claude.

## Struttura
- `src/App.jsx` — l'intera app Ally (UI, AI, persistenza)
- `src/main.jsx` — avvio React + registrazione Service Worker
- `public/manifest.webmanifest` — manifest PWA (fullscreen, icone, splash)
- `public/sw.js` — Service Worker (offline + avvio rapido)
- `public/icons/` — icone app generate (192, 512, maskable, Apple)
- `api/analizza.js` — funzione serverless Vercel: proxy sicuro verso l'API Anthropic
- `netlify/functions/analizza.js` + `netlify.toml` — equivalente per Netlify
- `firebase.json` — hosting Firebase (per l'AI servono le Cloud Functions, vedi nota)

## Pubblicazione su Vercel (consigliata, 5 minuti)
1. Carica questa cartella su un repository GitHub (oppure usa `npx vercel` dalla cartella).
2. Su vercel.com → "Add New Project" → importa il repository. Vercel riconosce Vite da solo.
3. In **Settings → Environment Variables** aggiungi:
   - `ANTHROPIC_API_KEY` = la tua chiave API presa da console.anthropic.com
4. Deploy. L'app è online su `https://tuonome.vercel.app`.

## Pubblicazione su Netlify
1. Importa il repository su netlify.com (build già configurata da `netlify.toml`).
2. Aggiungi la variabile d'ambiente `ANTHROPIC_API_KEY`.

## Firebase Hosting
`npm run build` poi `firebase deploy`. Nota: il proxy AI (`/api/analizza`) va replicato come Cloud Function.

## Installazione come app sul telefono
- **Android (Chrome):** apri il sito → comparirà "Aggiungi a schermata Home" / "Installa app" → l'icona Ally appare come una vera app, a schermo intero, con splash screen.
- **iPhone (Safari):** apri il sito → Condividi → "Aggiungi a schermata Home".

## Sviluppo locale
```bash
npm install
npm run dev
```
Nota: in locale `/api/analizza` non esiste (è una funzione Vercel). Usa `npx vercel dev` per avere anche l'AI in locale, oppure l'app userà il messaggio di errore + analisi semplificata.

## Cosa è incluso / cosa manca
✅ Persistenza reale (localStorage): profilo, piano, progressi, streak, storico — sopravvive per 30+ giorni e oltre
✅ AI reale con chiave protetta lato server
✅ PWA installabile, fullscreen, offline-ready, icone e splash
⚠️ I dati vivono sul dispositivo: per sincronizzarli tra più dispositivi serve un database cloud (es. Supabase/Firebase) — prossimo passo
⚠️ Login Google/Apple: predisposto nella UI, da collegare a Firebase Auth o Supabase Auth
⚠️ Notifiche push: richiedono FCM/Web Push — prossimo passo
