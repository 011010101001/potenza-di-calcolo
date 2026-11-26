# Dal 4004 al zettaflop

Una pagina HTML statica (un solo file, zero dipendenze) che racconta **cinquantacinque anni di
potenza di calcolo**, dal primo microprocessore al silicio che addestra i modelli.

Tutto è espresso in una sola unità — **operazioni elementari al secondo, di picco** — per poter
mettere sullo stesso asse il 4004 del 1971 (92.000 op/s) e il mondo intero del 2026 (92,8 Zop/s).

**Online:** <https://potenza.apice.me/> — e il seguito, <https://potenza.apice.me/dialogo.html>
**In locale:** doppio clic su `index.html`. Nessun server, nessuna build, nessuna connessione:
funziona offline.

---


## Il modello della sezione 11

Transformer char-level addestrato qui, in casa, in 3 minuti su CPU:

- 3 blocchi, 4 teste, 96 dimensioni, contesto 96, vocabolario 98 caratteri
- **353.184 parametri**, quantizzati int8 per riga (513 KB di pesi nel file)
- corpus: 150 KB di prosa italiana del progetto (pagina, dialogo, README, scaletta, log della chat)
- 1.300 passi, batch 32, AdamW con OneCycle, loss finale 1,56 / validazione 1,76

L'inferenza è scritta a mano in JavaScript (nessuna libreria, nessun CDN): matvec su
Float32Array, KV cache, GELU, softmax con temperatura. Un token qui è un carattere.
Per addestrarlo di nuovo: `scratch/minillm/train.py` + `export_js.py`.

## Come sta online

Il file è servito dal PC con un server statico su `127.0.0.1:8099` e pubblicato su
`potenza.apice.me` da un tunnel Cloudflare dedicato (tunnel `potenza`, config
`C:\Users\UTENTE\.cloudflared\potenza.yml`). Lo avvia `C:\Users\UTENTE\tools\potenza-site\start.ps1`,
richiamato all'accesso dal file `PotenzaSite.vbs` nella cartella Esecuzione automatica.

Se il PC è spento o in sospensione il sito non risponde: la pagina resta comunque nel repo e si
apre in locale con un doppio clic.

## Il dialogo

`dialogo.html` è una pagina a parte, stessa veste tipografica: le sei domande che restano dopo i
numeri — la potenza mondiale come rete peer-to-peer, il silicio e il quarzo, un chip fatto in casa
con un microscopio, il collo di bottiglia di oggi e se la velocità può avere un salto.

## Cosa contiene

| # | Sezione | Cosa mostra |
|---|---|---|
| 01 | **Le unità** | cosa conta come operazione, i prefissi SI da kilo a yotta, e i formati numerici (INT8, FP8, FP16, BF16, FP32, FP64) con la striscia dei bit, la tabella dei range/errori e tre test numerici (⅓, 0,1+0,2, 2²⁴+1) |
| 02 | **Il PC** | potenza del personal computer medio, 1971–2026, con scala log/lineare |
| 03 | **Il pianeta** | potenza di calcolo mondiale installata: serie misurata dal 2008, ricostruzione prima, banda ±40%, toggle picco/usato |
| 04 | **Un chip solo** | quando un singolo chip raggiunge tutto il mondo: due cursori (raddoppio del chip, crescita del mondo) ricalcolano grafico e verdetto |
| 05 | **Riferimenti** | 11 macchine riconoscibili (4004, Commodore 64, NASA Apollo, Cray-2, SGI, Pentium, iPhone 1 e 18, Tesla FSD, RTX 5090, B300) con prezzo d'epoca e costo per Top/s; curva del costo per Top/s 1969–2026; **arena "dieci secondi"** in tempo reale (sei macchine che accumulano operazioni dal vivo, con il verdetto in anni di 4004); riquadro "perché un iPhone 18 non è un terzo di una 5090" |
| 06 | **La memoria** | densità della RAM dal 1970 (1 Kbit → 32 Gbit), banda e latenza dal 1981, il *memory wall* (la banda ×10⁷, la latenza ferma), e perché oggi l'AI è un problema di memoria più che di calcolo |
| 07 | **Chi produce** | quota stimata della potenza mondiale per produttore (NVIDIA 39,4%, Google, Apple, MediaTek, Qualcomm, AMD, Amazon, Intel…), concentrazione della filiera, il calcolo che serve all'AI, Colossus 2 |
| 08 | **La sovranità** | nessuno è indipendente: la matrice delle dipendenze (chi sa fare cosa, paese per paese), la catena dei sette anelli da un chip al 2 nm, il caso Russia a 90 nm, e dove una nazione può davvero essere sovrana |
| 09 | **Il quanto** | perché un computer quantistico non ha un numero di op/s, dove siamo (Willow, Nighthawk), la roadmap al 2029-2033, i qubit necessari per RSA-2048 |
| 10 | **I modelli** | da ELIZA agli LLM di frontiera; cosa vuol dire *quantizzato*; il **metro umano** (GPQA, IMO, FrontierMath, Humanity's Last Exam con l'umano di riferimento accanto); i **problemi aperti risolti dall'AI** (AlphaTensor, FunSearch, IMO 2024-25, la congettura di Erdős 2026); e cosa succederebbe a far girare un 27B nel datacenter della NASA del 1969 |
| 11 | **L'esperimento** | un transformer vero, 353.184 parametri, dentro la pagina: genera nel browser, misura i token al secondo della tua macchina, mostra le 4 teste di attenzione e le probabilità del prossimo carattere |
| 12 | **La rete** | la potenza mondiale come rete P2P: il conto della banda (NVLink 1,8 TB/s contro 7,5 MB/s di fibra), DisTrO e le corse distribuite vere (INTELLECT-1/2, Psyche con Hermes 4.3), i tre muri che restano |
| 13 | **Dieci anni** | 2016 vs 2026 su quattro metriche, più la crescita annua della potenza mondiale |
| 14 | **I dati** | tabella completa 1971–2026 con esportazione CSV |
| 15 | **Metodo** | cosa è misurato, cosa è stima, cosa non va sommato, fonti |

## Fonti principali

- **Earth Compute** — inventario del silicio installato (2008–2026), unità BF16 denso, quote per
  produttore e per segmento, banda di incertezza ±40%. <https://earthcompute.org>
- **Hilbert & López**, *The World's Technological Capacity to Store, Communicate, and Compute
  Information*, Science 332 (2011) — 6,4·10¹⁸ istruzioni/s nel 2007, +58%/anno.
- **TOP500** — 66ª lista, novembre 2025 (14,99 EFLOP/s aggregati).
- **Epoch AI** — flotte di acceleratori, costo dei datacenter, training compute dei modelli di
  frontiera (GPT-4 ≈ 2,1·10²⁵ FLOP, crescita 4–5×/anno).
- **IBM Quantum** (roadmap Starling 2029, Blue Jay 2033+) e **Google Quantum AI** (*Quantum
  Echoes*, Nature 2025).
- **Schede tecniche** NVIDIA (A100, H100, B200, B300, RTX 5090), Intel 4004, SGI, Cray.
- **Benchmark dei modelli** — GPQA Diamond (Epoch AI, novembre 2026), Artificial Analysis index,
  schede dei modelli open (Qwen3.8, Gemma 4, gpt-oss, Kimi K3.1).

## Avvertenze (le stesse scritte nella pagina)

- La serie mondiale dal 2008 è misurata; **prima del 2008 è extrapolata** col tasso del 58%/anno e
  vale come ordine di grandezza, non come misura.
- La curva del PC medio è una **ricostruzione** da schede tecniche di macchine rappresentative.
- Le quote per produttore della sezione 06 sono un **modello** (potenza per segmento × quota di
  mercato), con un errore dell'ordine del ±30% per riga: nessuno pubblica questa classifica.
- I prezzi sono **d'epoca, non attualizzati**.
- I confronti fra dispositivi sono su **picco dichiarato, alla precisione migliore**: INT8, BF16 e
  FP64 non sono la stessa unità, e il riquadro della sezione 05 spiega perché il numero nominale va
  preso con le molle.
- Le date oltre il 2026 (Starling, Blue Jay, RSA) sono **impegni dei costruttori**, non fatti.

## Struttura

```
potenza-di-calcolo/
├── index.html   # la pagina: CSS e JS incorporati, nessuna dipendenza esterna
└── README.md
```

I dati delle serie principali sono incorporati in `index.html` come JSON (`const DATA = {...}`),
subito prima del codice dei grafici; la tabella della sezione 10 li esporta in CSV dal browser.

## Comandi utili

```bash
# aprire la pagina da terminale (Windows)
start index.html

# controllare che non ci siano risorse esterne
grep -n "src=\"http" index.html   # atteso: nessun risultato
```
