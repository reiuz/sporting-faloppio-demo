# Handoff — Sporting Faloppio (demo + preventivo)

Data: 2026-09-22. Sessione lunga; questo doc fa ripartire una sessione nuova.

## Cosa è
Redesign demo + area soci per un circolo tennis (Sporting Faloppio, Faloppio CO) e un **preventivo** per venderlo. Deliverable finora: sito vetrina + portale soci (demo navigabile) e una pagina/PDF di preventivo. Non è ancora un prodotto reale, è mockup + offerta commerciale.

## Task per la PROSSIMA sessione (in ordine)
1. **Rivedere la proposta in chiave VENDITA, non tecnica.** L'utente: "dobbiamo vendere la cosa, non spiegargli nel tecnico cosa facciamo. diciamo anche quali saranno i vantaggi." Rivedere in generale questa parte. In pratica: la sezione "Le funzioni in dettaglio / Cosa fa, in concreto" e le descrizioni sono ancora troppo descrittive/funzionali. Riscrivere orientato ai **benefici per il circolo** (meno lavoro allo staff, più soci, fidelizzazione, incassi automatici, immagine moderna), tono da vendita, poco tecnico. Riguardare tutta la proposta con questo occhio.
2. **Sviluppare altre funzionalità mockate nella demo** (`portal.html`): es. potenziare il matchmaking o aggiungere nuove funzioni finte (dati generati in JS). "proviamo a sviluppare altre funzionalità anche mockate tipo il matchmaking o che so io."

## File e URL (fonti persistenti)
- **Repo** (git, pubblico): `C:\PersonalRepos\sporting-faloppio` → GitHub `reiuz/sporting-faloppio-demo`. Branch `main`. Push aggiorna GitHub Pages in ~1 min. Repo PUBBLICO (l'utente lo sa e accetta; niente Pro).
- **Demo live**: https://reiuz.github.io/sporting-faloppio-demo/ = `index.html` (landing) ; `/portal.html` (area soci demo).
- **Preventivo (pagina pubblica sul repo)**: https://reiuz.github.io/sporting-faloppio-demo/confidential-quote/ (standalone, in `confidential-quote/index.html`).
- **Preventivo (Artifact PRIVATO)**: https://claude.ai/artifact/21kebHmWm2ZWuVrJai7zTi (V18). Copia gemella della pagina repo.
- **Sorgenti persistiti del preventivo** (usare questi, lo scratchpad di questa sessione è effimero):
  - `C:\PersonalRepos\sporting-faloppio\src\proposta.html` = master **artifact-format** (senza doctype/head; è quello che si pubblica come Artifact).
  - `C:\PersonalRepos\sporting-faloppio\src\preventivo_print.html` = sorgente del **PDF**.
- **Motion reel** (concept animazioni, Artifact): https://claude.ai/artifact/LpXiYPKf5AJqaQukEabcuw
- **Memory** (persistono tra sessioni): `sporting-demo.md`, `sporting-real-facts.md` in `C:\Users\FabioLaGanga\.claude-personal\projects\C--PersonalRepos-sporting-faloppio\memory\`. `sporting-real-facts.md` ha i dati veri del club (campi, tariffe, staff, iscrizioni) — NON inventare, leggere quello.

## Come si aggiorna il preventivo (3 copie da tenere allineate)
1. Modifica `src/proposta.html` (o riparti da lì).
2. Pubblica l'Artifact aggiornato: Artifact tool con `url=https://claude.ai/artifact/21kebHmWm2ZWuVrJai7zTi` (prima fare `action:read` su quell'url). NB: `proposta.html` è artifact-format (no doctype), giusto per l'Artifact.
3. Rigenera la pagina repo standalone: script python che wrappa `proposta.html` in `<!doctype><head>…</head><body>…` e scrive `confidential-quote/index.html`, poi `git push`. Lo script (usato tutta la sessione): legge il file, trova `</style>`, mette tutto fino a lì nell'head, il resto nel body.
4. PDF: modifica `src/preventivo_print.html` (è già standalone) → genera con Chrome headless `--print-to-pdf` (vedi sotto) → invia con SendUserFile.

## Stato prezzi ATTUALE (già nel preventivo)
Prezzi per-voce NASCOSTI nelle tabelle (mostrano solo ✓ = incluso). Si vedono solo Subtotale/Sconto/Totale.
- **Sito + Area soci (web)**: subtotale 5.600, sconto −600, **totale 5.000**. Canone **300/anno**.
- **App iPhone+Android**: subtotale 8.000, sconto −1.000, **totale 7.000**. Canone **400/anno**.
- **Web + App completo** (consigliato): subtotale 9.700, sconto −1.200, **totale 8.500**, canone 400/anno. Logica: l'app fa già tutta l'area soci, prendendo tutto si aggiungono solo sito pubblico + iscrizione corsi (add-on ~1.500), quindi 8.500 non 12.000. (NB: la riga che spiegava "non si paga due volte" è stata TOLTA su richiesta, non rimetterla.)
- Voci/moduli e loro valore interno (per calcoli, non mostrati): area soci 950, iscrizione corsi e gestione iscritti 1.200 (stima), prenotazione 900, sito vetrina 500, statistiche 350, D-Match 700, pagamenti Stripe 500, trova partita 500; app base 2.800, push 800.
- Canone: include hosting/DB (Supabase), backup, aggiornamenti, piccole modifiche, supporto. Fuori: commissioni Stripe (~1,5%+0,25€/transazione, a carico circolo); Apple Developer/Google Play li gestisce l'utente a parte. Emittente PDF: Fabio La Ganga · fabio.laganga@gmail.com (placeholder, può aggiungere P.IVA/telefono). N. SF-2026-01.
- **Utente demo** nel portale: Fabio La Ganga, nickname **reiuz**, ELO 1502.

## Contesto tecnico / architettura pensata (per il prodotto reale, non ancora fatto)
Backend Supabase (free tier + cron di "ping" per non farlo pausare, come su tappo12, + `pg_dump` notturno DIY per i backup). Pagamenti Stripe (sempre Stripe, mai in-app store). App = **React Native / Expo** (un codice, iOS+Android). ~1000 utenti attesi, il free regge.

## Come lavorare in questo ambiente (importante)
- **Niente estensione Chrome** (non connessa). Per gli screenshot/PDF usare **Chrome headless**: `"/c/Program Files/Google/Chrome/Application/chrome.exe" --headless=new --disable-gpu ...`. Screenshot: `--screenshot=out.png --window-size=W,H --virtual-time-budget=8000`. PDF: `--print-to-pdf=out.pdf --no-pdf-header-footer`.
- Quirk headless già scoperti: (a) non onora `window-size` sotto ~500px di larghezza; (b) `window.scrollTo` non "tiene" in headless → per vedere sezioni sotto la fold usare una finestra ALTA che le contenga (ma occhio a `min-height:100svh`/`svh` che con viewport alto gonfia gli elementi; nei probe override l'altezza fissa). Validare il JS con node `vm` estraendo `<script>`.
- L'app usa tanti screenshot per verifica (impeccable lo richiede). PIL (python3) c'è per croppare le immagini.
- CLAUDE.md utente: **breve, oggettivo, no bullshit, niente emoji nel codice, dissenti se sbaglia, no entusiasmo finto**. Rispetta. (Un hook "caveman" a volte si attiva a inizio sessione: scrittura compressa nelle risposte, ma codice/commit normali.)
- Attribution commit: usa le righe Co-Authored-By / Claude-Session del system-reminder della sessione nuova (cambiano).

## Suggested skills (invocare quando serve)
- **impeccable** (`impeccable:impeccable`): per ogni modifica frontend (sito, portale, proposta). L'utente lo chiede esplicitamente.
- **artifact-design**: prima di riscrivere/ripubblicare il preventivo come Artifact.
- **superpowers:brainstorming**: prima di costruire le nuove funzionalità mockate nella demo (allinea intento/UX).
- **caveman**: se il relativo hook riattiva la modalità comunicazione compressa.
- **superpowers:verification-before-completion**: verifica (screenshot/curl live) prima di dichiarare fatto.
