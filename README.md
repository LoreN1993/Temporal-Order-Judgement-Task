In questo compito i partecipanti giudicano quale dei due stimoli, audio o visivo, sia arrivato per primo. Ad ogni prova (trial) viene presentata una coppia di stimoli con un differente Stimulus Onset Asynchrony (SOA) che va da –300 ms (suono prima) a +300 ms (visivo prima), con passi di 50 ms e senza condizioni di simultaneità perfetta (SOA = 0).

1. Logica delle risposte
Tasti usati:

‘m’ = “stimolo visivo prima”

‘z’ = “stimolo sonoro prima”

Per i soggetti con ID > 131 la mappatura è invertita (per un errore sperimentale): in quel sotto-gruppo, ‘m’ e ‘z’ vengono scambiate.

2. Struttura temporale di ogni trial
Fixation Cross

Croce centrale mostrata per 700 ms.

Inter‐Stimulus Interval (ISI)

Dopo la croce, schermo vuoto (“blank screen”) di 300 ms

Quindi attesa di un ISI medio di 1200 ms ± jitter (± 300 ms).

Presentazione stimoli

Un orologio ad alta precisione (core.Clock()) viene resettato immediatamente prima della presentazione del primo stimolo.

Se SOA < 0:

viene riprodotto subito il suono;

si aspetta abs(SOA) ms;

viene mostrato il flash visivo (33 ms).

Se SOA > 0:

viene mostrato subito il flash visivo (33 ms);

si aspetta SOA ms;

viene riprodotto il suono.

In tutti i casi misuriamo con soa_clock.getTime() il vero intervallo tra i due stimoli, moltiplicando per 1000 per ottenere i millisecondi effettivi.

La durata del flash visivo è garantita da due cicli di win.flip() (≈ 33 ms totali), sincronizzati con il refresh del monitor per evitare drift.

3. Misurazione dei tempi di reazione (RT)
All’inizio di ogni prova il codice salva start_time = rt_clock.getTime()

Alla pressione del tasto la variabile rt viene calcolata come end_time – start_time

Se non c’è risposta entro 2 s, viene registrato un timeout.

4. Salvataggio dei dati
Per ogni trial vengono registrati in un file CSV:

mathematica
Copia
SOA | Response | RT (s) | Accuracy | ISI | Jitter | Start_time (s) | End_time (s) | Block
Accuracy = 1 se la risposta corrisponde alla vera prima occorrenza dello stimolo (tenendo conto anche dell’inversione mappatura), 0 altrimenti.

5. Perché usare core.Clock()
Precisione: misura in secondi con float, indipendente dal sistema operativo.

Affidabilità: core.sleep() blocca il codice ma non dà tempi di misura, mentre Clock registra esattamente i tempi di evento.

Nota finale
Questo schema assicura che:

ogni intervallo (fixation, blank, SOA) venga misurato e verificato;

la durata visiva (33 ms) coincida con due refresh del monitor;

i tempi di reazione siano calcolati in modo accurato e non distorti da funzioni di sospensione del thread.





