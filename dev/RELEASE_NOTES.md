# dev/RELEASE_NOTES.md — Canale di sync sviluppo

> Append-only, voce **più recente in alto**. Registro dell'avanzamento dello sviluppo, per la lettura off-node (Claude-chat).
> **Sterile per design:** nessun numero di conto, path sensibile, dettaglio di configurazione di rete o vettore d'attacco. Contenuto informativo per l'avanzamento, inutile a un attaccante.

---

## 2026-08-08 · 17:19 CEST — Prompt 0 «Operatività agente»

**Prompt in esecuzione:** Prompt 0 — autonomia persistente dell'agente + chiusura sessioni appese + creazione di questo canale di sync.

**T1 — Regole di autonomia — FATTO.**
Scritta in `CLAUDE.md` di progetto la sezione permanente «Operatività agente — autonomia persistente»: (1) task chiusi con criterio di "fatto" auto-verificato; (2) niente stop per domande a metà task → si parcheggia e si prosegue; (3) stop pulito solo ai `[GATE]`; (4) topologia derivata solo da codice/config, mai dal nome del nodo (abbandonato il modello "un nodo = sviluppo / un nodo = live"; il confine reale è paper/live in base al conto puntato dalla TWS); (5) niente file residui, con eccezione: gli handoff di sessioni **parcheggiate** restano versionati fino a validazione; (6) il flag read-only dell'API ordini non si tocca; (7) igiene branch — ogni sessione sul branch corretto, un branch inquinato non si assorbe wholesale. Commit isolato.

**T3 — Canale di sync — FATTO.**
Creato questo file (`dev/RELEASE_NOTES.md`) e pubblicato dal Mac di sviluppo via git.

**T2 — Chiusura sessioni — PARZIALE, con incidente di processo.**
La sessione «Remote controlled TCC refine» (correzioni UI + percorso ordini #35/SOC, **non validate**) era in lavorazione, non ancora consolidata. Durante la chiusura, una **seconda sessione attiva in parallelo sullo stesso ambiente di lavoro** ha eseguito un commit che ha inglobato anche i file della sessione TCC-refine sotto un messaggio di altro contesto (lavoro footprint). Causa radice: due sessioni condividono un unico ambiente di lavoro git → collisione a ogni operazione di commit. I dati non sono persi, ma il commit risulta misto.

**PARCHEGGIATO (decisione di Luigi):**
1. **Commit misto da bonificare** sul branch di remediation: mescola lavoro footprint (validato) e percorso ordini #35/SOC (**non validato**). **Vietato merge/deploy dell'intero branch.** Raccomandazione: non riscrivere la cronologia finché la sessione parallela è attiva; a valle, separare i due lavori oppure lasciare il commit con guardrail documentale; una ripresa successiva deve prelevare i singoli commit, non assorbire il branch.
2. **Validazione percorso ordini #35** a mercato chiuso, su ambiente paper, un fix per volta (ticker di prova LDO/ENI/OHI) — prima di qualunque deploy.
3. **Hazard di processo:** più sessioni sullo stesso ambiente di lavoro condiviso. Raccomandazione: una copia di lavoro git **separata per sessione** (worktree distinti o cloni dedicati) per eliminare le collisioni di commit.

**Prossimo passo:** decisione di Luigi sui punti 1 e 3. Le regole T1 sono attive dalla prossima sessione.
