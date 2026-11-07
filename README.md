# Matéa

Questo repository contiene due cose distinte:

1. **Il sito pubblico di Matéa Skincare Studio** (`index.html`, `chi-siamo.html`, `metodo.html`, `privacy.html`, cartella `immagini/`).
2. **Uno strumento interno per Giorgia**: la *Consulenza e prova trucco* (`consulenza-trucco.html`), descritta in questo documento.

Lo strumento interno **non fa parte del sito**: non è collegato da nessuna pagina, non ha SEO, favicon o tracciamento, ed è marcato `noindex, nofollow`. Usa gli stessi colori e font del sito (Cormorant Garamond + Manrope; `warm-white`, `beige`, `taupe`, `charcoal`, `sage`, `gold`).

> Stato: prima versione completa e funzionante, **salvataggio solo locale** (nel browser del dispositivo). Il passo successivo è il collegamento a Firebase (vedi *Da fare*).

---

## A cosa serve

Giorgia lo usa **dal telefono** (iPhone o Android) per gestire il percorso di una cliente che chiede un trucco, soprattutto per le spose:

1. si fissa l'appuntamento per la **prova trucco** e si crea subito una bozza di scheda;
2. il giorno della prova si compila la scheda in 7 passi (anagrafica, forma del viso, colori, aspettative, linee guida, prova trucco, riepilogo);
3. si **genera un PDF** di riepilogo, con foto della prova;
4. un'**agenda** tiene traccia di che cosa è da fare, da confermare, da incassare, con allarmi per ciò che è scaduto.

Il concetto di fondo: forma del viso, occhi e labbra dicono **dove lavorare con le luci** per ridisegnare il viso; i colori si scelgono tra quelli che valorizzano la pelle, **incrociati con i gusti della cliente**. Non è un'analisi armocromatica completa (niente consigli di abbigliamento): è una valutazione dei colori **per il trucco**.

---

## Come aprirlo

È un unico file HTML, senza build né dipendenze da installare:

- **In locale:** aprire `consulenza-trucco.html` in un browser.
- **Online:** dopo il merge sul branch principale, con GitHub Pages sarà raggiungibile all'indirizzo del sito seguito da `/consulenza-trucco.html`. Attenzione: anche se non è indicizzato né linkato, **l'indirizzo è pubblico** per chi lo conosce. Con il login Firebase (vedi sotto) i dati saranno comunque protetti.

Risorse caricate da internet: i font da Google Fonts e la libreria **jsPDF 2.5.1** da cdnjs (serve connessione per generare il PDF).

---

## Cosa c'è nell'app

### Schermata principale

- **Card "Crea scheda"**: il pulsante "+ Aggiungi scheda" apre un pannello con nome, cognome, cellulare, email, tipo di evento, giorno e ora della prova, data dell'evento. Due azioni: **Salva scheda** (va in agenda come bozza) oppure **Salva e inizia ora** (apre subito la scheda a passi).
- **Card "Agenda"**: elenco a pill con conteggi.
  - **Calendario** (predefinita): prove da fare e trucchi confermati, per data. In cima le schede **scadute in rosso** ("Da aggiornare"); poi i **prossimi 30 giorni**; il resto è in un blocco chiuso **"Più avanti · N schede · fino a …"** che si apre al tocco.
  - **Attesa conferma**, **Fatti**, **Incassi** (tutte le schede con un incasso in sospeso), **Annullati**, **Tutti**.
  - Selettore **Periodo** (per mese) che vale per ogni pill.
- **Card "Schede salvate"**: ricerca per nome, cognome, email o cellulare; sotto, le ultime 5 schede.
- Non esiste nessuna funzione di **cancellazione** delle schede, di proposito.

### Le fasi di una scheda (pannello dal chip di stato)

Toccando il chip di stato si apre un pannello a **due fasi che si sbloccano a cascata**:

1. **Prova trucco**: *Da fare* / *Fatta*. Se fatta compare il pagamento *Da incassare* / *Incassata*.
2. **Trucco definitivo** (bloccato finché la prova non è fatta): *Da confermare* / *Confermato* (con **data del trucco definitivo**, obbligatoria); poi *Da fare* / *Fatto*; poi *Da incassare* / *Incassato*.
3. Opzione **Scheda annullata / non confermata** (ricorda a che punto era arrivata).

Il pulsante in basso è **Chiudi** se non si è toccato nulla, **Salva / Annulla** se ci sono modifiche (nessuna chiusura accidentale).

Da questi dati derivano gli stati mostrati in agenda: *Prova da fare*, *Prova fatta · attesa conferma*, *Trucco confermato*, *Trucco fatto*, *Annullata*. Gli incassi in sospeso compaiono come badge oro **"€ Prova"** / **"€ Saldo"**; una scheda completamente incassata e fatta mostra **"✓ Chiusa"**. I prezzi **non** sono mai registrati.

### Che cosa succede alle date

- **Prova da fare** → nel calendario alla **data della prova**.
- **Prova fatta** → esce dal calendario ed entra in **Attesa conferma**.
- **Confermato** → rientra nel calendario alla **data del trucco definitivo**.
- Una prova da fare con data passata, o un trucco confermato con data passata o senza data, diventa **rossa** con tre azioni rapide: **Fatta**, **Sposta** (nuova data), **Altro…** (apre il pannello fasi).
- Con un trucco confermato a 30 giorni o meno dall'evento e cambiamenti previsti (abbronzatura, capelli, sopracciglia), compare il badge **"↻ Ricontrolla colori"**.
- A fine scheda, **"Salva e aggiorna agenda"**: se la scheda è in *Prova da fare* chiede se la prova è stata fatta (e incassata o da incassare) e la porta in *Attesa conferma*.

### I 7 passi della scheda

Una striscia di passi numerati, sempre visibile e cliccabile, permette di saltare a qualsiasi passo. Indietro/Avanti stanno sempre nello stesso punto. Le schede si salvano da sole.

1. **Anagrafica**: contatti; **foto iniziale** (a viso struccato, etichettata "Prima"); giorno e ora della prova; tipo di evento, data, luogo del trucco, fascia oraria (inizio previsto / fine massima: per le spose il trucco si fa di solito il giorno stesso, quindi l'orario massimo è un vincolo); note.
2. **Forma del viso**: 7 geometrie a linee (ovale, rotondo, quadrato, rettangolare, a cuore, a pera, diamante); tipo di pelle, forma degli occhi, labbra; note.
3. **Colori per il trucco**:
   - guide a tendina per riconoscere **sottotono**, **intensità** e **profondità**;
   - famiglia di colori suggerita (12 sottostagioni, usate come supporto) e **palette consigliata** solo per occhi, labbra e blush, più il metallo;
   - **Fino all'evento**: abbronzatura prevista, colore dei capelli (e nuovo colore previsto), sopracciglia previste, fondotinta usato alla prova e **previsto per il giorno**; avviso sull'abbronzatura con il suggerimento di simulare la carnagione prevista (fondotinta più scuro o terra) e di prevedere un controllo prima dell'evento;
   - **Scelta con la cliente**: colori che le piacciono e colori scelti per il trucco.
4. **Aspettative**: stile, finish, intensità occhi, labbra, resistenza, **abito** (secondario: serve solo ad armonizzare il trucco; per la sposa bianco, avorio, champagne, colorato), riferimenti, cose da evitare, allergie.
5. **Linee guida**: per la forma del viso scelta, uno **schema con luci, contouring e blush** già posizionati e i consigli scritti (obiettivo, contouring, blush, punti luce, in più).
6. **Prova trucco**: 12 passaggi modificabili e ampliabili (prodotti, tonalità, tecnica), note, cosa cambiare per il giorno, **foto** (scatta o galleria, con etichette Prova / Prima / Dopo / Riferimento).
7. **Riepilogo**: tutta la scheda in card, stato e **pulsante "Genera PDF"**.

### PDF

Formato A4, impaginato nello stile del brand:

- **Pagina 1**: anagrafica e viso su due colonne; sotto, i colori per il trucco con la palette.
- **Pagina 2**: aspettative e linee guida del viso (schema + consigli).
- **Pagine successive**: prova trucco, note e foto.

Titoli in grassetto con evidenziatore salvia. Il PDF si può condividere o salvare dal telefono. Nel PDF si usano i caratteri standard (non Cormorant/Manrope).

### Dove stanno i dati

Per ora **solo nel browser del dispositivo** (IndexedDB, database `matea-consulenze`, tabella `schede`). Le foto sono ridimensionate (lato lungo 1600 px) e salvate dentro la scheda. **Se si cancellano i dati del browser, le schede si perdono**: finché non c'è Firestore non esiste un backup (importa/esporta è stato tolto di proposito).

---

## Note tecniche

- Un solo file HTML con CSS e JavaScript inclusi, senza framework.
- Mobile-first: `viewport-fit=cover`, **safe area generose** in alto e in basso (notch/dynamic island/barra home e barra gesti Android), campi a 16 px (niente zoom automatico su iPhone), pulsanti ad almeno 44 px.
- Campi principali di una scheda: `anag` (contatti, date, luogo, orari), `viso`, `colori`, `asp`, `prova` (passaggi e note), `photos`, `stato`, `fase` (prova fatta / confermato / fatto), `pag` (prova e saldo incassati). Lo `stato` è derivato dalle fasi e alimenta agenda e filtri.
- **Testo non selezionabile né copiabile** (tranne dentro i campi di inserimento), menu contestuale e trascinamento delle immagini disattivati. È una comodità d'uso, non una protezione: chi ha accesso alla pagina può comunque leggerne il codice.
- **Cellulare a 10 cifre**, senza prefisso: il campo accetta solo cifre, ne permette al massimo 10 e mostra "Servono 10 cifre (n/10)"; il pannello di creazione non salva con un numero incompleto. **Email**: controllo del formato con avviso "Controlla l'indirizzo email".
- Il marchio si scrive **MATéA** (lettere maiuscole con la é accentata minuscola), come nel logo del sito.
- I contenuti tecnici (linee guida per forma del viso, palette delle sottostagioni, guide a tendina) sono in oggetti dentro lo script (`FACES`, `SD`, `HOW`) e sono facili da correggere.
- Testato in un browser headless con viewport da iPhone: flussi, agenda, PDF. **Non è stato provato su un telefono vero.**

### Limiti noti

- Fotocamera: il pulsante "Scatta una foto" usa `capture="environment"`, che su telefono apre direttamente la fotocamera posteriore. Dentro il visualizzatore di claude.ai la scelta del file può essere bloccata: va verificata sulla versione ospitata.
- Il salvataggio/condivisione del PDF e la fotocamera vanno provati davvero solo online (GitHub Pages), non nel visualizzatore.

---

## Da fare

### Prima di usarlo con clienti vere

- [ ] **Merge sul branch principale** (e GitHub Pages) per avere un indirizzo stabile. Ad oggi il lavoro è sul branch `claude/makeup-consultation-tool-1y1lf2`.
- [ ] **Provare su telefono vero**: fotocamera, caricamento foto, generazione/condivisione del PDF, safe area, tastiera.
- [ ] **Revisione di Giorgia** dei contenuti: linee guida per forma del viso, palette delle sottostagioni, guide a tendina, testi dell'abbronzatura e dei capelli. Sono indicazioni generali scritte come punto di partenza.
- [ ] **Informativa privacy e consenso**: nome, contatti e foto del viso sono dati personali; con il cloud va aggiunta l'informativa e il consenso della cliente per dati e foto.

### Firebase (salvataggio nel cloud)

- [ ] Creare il **progetto Firebase** e attivare **login Google** e **Firestore**.
- [ ] **Accesso limitato a una sola email** (quella di Giorgia) tramite le regole di sicurezza del database, non solo nella pagina.
- [ ] Salvare le **schede** in Firestore; le **foto** compresse dentro Firestore (sottodocumenti) per restare sul piano gratuito, senza Firebase Storage.
- [ ] Campi **indicizzati** per stato e date (agenda, filtri) e per la ricerca per nome, cognome, email e cellulare.
- [ ] Persistenza offline, sincronizzazione tra telefono e PC, **migrazione** delle schede locali eventualmente presenti.
- [ ] Per l'accesso servono: l'**email Gmail di Giorgia** e la **configurazione web** del progetto (è pubblica, non è un segreto).

### Possibili miglioramenti

- [ ] Esportare l'appuntamento nel **calendario del telefono** (file `.ics`).
- [ ] **Promemoria con notifica** (richiede Firebase).
- [ ] Blocco **"Consigli per la cliente"** nel PDF (sole con moderazione e crema solare, niente autoabbronzante sul viso senza dirlo, niente trattamenti nuovi negli ultimi giorni, foto di controllo 2–3 settimane prima).
- [ ] **Consigli di trucco** dipendenti anche da occhi e labbra (per esempio occhi incappucciati o ravvicinati).
- [ ] Vista **mese** dell'agenda.
- [ ] Font del brand nel PDF (oggi caratteri standard).
- [ ] Se un giorno si volesse **rivendere il modello** ad altri consulenti: account e dati separati per utente, più impostazioni di personalizzazione.

---

## Attrezzatura consigliata

Il servizio usa i colori **per il trucco**, non per l'abbigliamento: il kit può essere piccolo. Sono indicazioni generali; i prodotti e i prezzi vanno verificati. Se Giorgia ha seguito un corso, conviene seguire il suo metodo e il suo kit di riferimento.

**Indispensabile**
1. **Luce**: luce naturale vicino a una finestra, non diretta; per le uscite una **lampada portatile a luce diurna** (circa 5000–6000 K) con **resa del colore alta (CRI 90 o più, meglio 95)**. Le luci calde o i led colorati falsano i colori.
2. **Mantella o telo neutro bianco o grigio** e **fascia per capelli** bianca o grigia: servono a neutralizzare l'ambiente per giudicare il sottotono.
3. **Pochi teli, circa 8–10**, in tessuto opaco a tinta unita, 50×50 o 60×60 cm:
   - bianco puro e avorio (sottotono);
   - oro e argento (sottotono);
   - pesca o corallo contro rosa o fucsia (caldo contro freddo);
   - un rosso puro e un tono attenuato come salvia o malva (intensità).
4. **I prodotti di Giorgia come campioni**: la prova più affidabile è mettere vicino al viso **rossetti e blush veri** (uno caldo e uno freddo, uno acceso e uno spento).
5. **Specchio grande** e sfondo neutro (parete o telo bianco/grigio).
6. Struccante e dischetti: l'analisi si fa a viso struccato.

**Utile**
- **Cartoncino grigio 18%** per le foto, perché il telefono corregge i colori da solo; foto sempre con la stessa luce e senza filtri.
- **Fondotinta in più tonalità vicine** (o modo di mescolarle) e una **terra/bronzer**, per simulare la carnagione prevista e per correggere il giorno dell'evento.
- Custodia o borsa per i teli, puliti e senza pieghe.

**Non servono** per questo servizio: il set completo delle quattro stagioni, palette stampate da lasciare alla cliente, colori per l'abbigliamento.

**Dove cercare**: "kit teli armocromia" o "color analysis drapes". Le guide parlano sia del sistema a **12** sia di quello a **16** stagioni: l'app usa le 12 sottostagioni, quindi controllare quale sistema segue il kit. Diffidare dei kit economici con colori stampati male: meglio pochi teli di buona qualità che molti sbagliati.

---

## Come proporre una modifica

Ogni modifica richiesta viene portata direttamente sul branch principale (`main`), quindi il file ufficiale è sempre aggiornato.

Il flusso, i testi e i colori sono tutti in `consulenza-trucco.html`. Per le modifiche ai contenuti (testi delle guide, palette, stati) basta indicare cosa cambiare e dove compare nell'app.
