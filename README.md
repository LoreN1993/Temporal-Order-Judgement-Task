 Questo task misura i giudizi di ordine temporale (TOJ) su coppie audiovisive.
# Ad ogni trial vengono presentati un suono e un flash visivo a SOA variabile
# (da –300 ms a +300 ms, escluso 0), e il partecipante risponde quale
# dei due è apparso per primo (m = “visivo primo”, z = “suono primo”).
#
# 1) Struttura dei trial e timing generale
#    • Ogni trial inizia con una croce di fissazione (700 ms) seguita da:
#         – un ISI medio di 1200 ms ± jitter di ±300 ms
#         – uno schermo vuoto (“blank”) di 300 ms
#      Questo garantisce che il fixation-cross e il primo stimolo non si
#      sovrappongano mai.
#
#    • L’orologio RT viene avviato una sola volta all’inizio dell’esperimento
#      (rt_clock = core.Clock()), così da avere misure di RT affidabili senza
#      ricreare un nuovo clock a ogni trial.
#
# 2) Controllo di precisione di SOA e durata degli stimoli
#    • Per misurare l’effettivo intervallo tra le due modalità (SOA) si usa
#      core.Clock() (“soa_clock”) e non un semplice sleep(). Sleep() di Open
#      Sesame / Psychopy è dipendente dal SO, mentre soa_clock.getTime()
#      fornisce una misura in millisecondi totalmente indipendente.
#
#    • Nella condizione SOA < 0 (suono primo):
#        soa_clock.reset()
#        my_sampler.play()
#        clock.sleep(abs(soa))           # attende il SOA programmato
#        soa_duration = soa_clock.getTime() * 1000
#        visual_canvas.show()
#        … (misura flip e stim_duration come descritto di seguito)
#
#    • Invece per SOA > 0 (visivo primo) l’ordine di play() e show() è invertito.
#
# 3) Durata controllata degli stimoli (33 ms)
#    • Per garantire esattamente 33 ms di esposizione visiva su un monitor
#      a 60 Hz, si ricorre a:
#         flip_clock = core.Clock()
#         win.flip()                     # sync con refresh del display
#         core.wait(2 * (1/60))          # circa 33.3 ms (2 refresh)
#      win.flip() “blocca” il frame finché il monitor non lo disegna, evitando
#      ritardi di sistema.
#
# 4) Logica di risposta e salvataggio
#    • Al termine della presentazione avviamo un timeout di 2000 ms su
#      my_keyboard.get_key(), raccogliendo key e rt.
#    • Calcoliamo accuracy = 1 se la chiave corrisponde all’ordine reale
#      (m per visivo primo quando soa>0, z per suono primo quando soa<0).
#    • I dati di ogni trial (SOA, key, RT, accuracy, ISI, jitter, start/end time,
#      block) vengono salvati in un CSV dedicato per ciascun soggetto.
#
# 5) Suddivisione in fasi
#    • PREPARE_PHASE: inizializzazione trial_counter, blocchi, caricamento
#      stimoli, creazione Canvas/Sampler/Keyboard.
#    • RUN_PHASE: presentazione fixation / blank / stimoli, gestione SOA,
#      raccolta risposta, salvataggio dati, pulizia schermo.
