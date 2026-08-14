# 42-NetPractice

# miniRT — Piano di Studio e Suddivisione del Lavoro (2 persone)

*Basato sul subject v10.0 — parte mandatoria + bonus consigliati*

---

## 1. TEORIA DA STUDIARE

### 1.1 Matematica vettoriale (fondamentale, va capita bene prima di scrivere codice)
- Vettori 3D: somma, sottrazione, moltiplicazione per scalare
- Prodotto scalare (dot product) → usato per: angolo tra vettori, proiezioni, lighting (Lambert), test di intersezione
- Prodotto vettoriale (cross product) → usato per: normali, sistemi di coordinate locali (per camera e cilindro/cono)
- Normalizzazione di un vettore
- Distanza tra due punti, lunghezza di un vettore (norma)
- Riflessione di un vettore rispetto a una normale (necessario per specular/Phong, bonus)
- Cambio di base / costruzione di una base ortonormale (u, v, w) a partire da un solo vettore direzione (serve per orientare camera, cilindro, cono)

### 1.2 Geometria analitica / equazioni di intersezione raggio-oggetto
- Equazione parametrica di una retta (il raggio): `P(t) = O + t*D`
- **Sfera**: equazione quadratica (delta/discriminante), le due soluzioni t1/t2, quale scegliere (la più piccola positiva), gestione del caso "dentro la sfera"
- **Piano**: intersezione raggio-piano tramite prodotto scalare tra normale e direzione del raggio, caso raggio parallelo al piano (denominatore ~0)
- **Cilindro** (il più complesso):
  - Cilindro infinito (equazione quadratica proiettata sul piano perpendicolare all'asse)
  - Limitazione dell'altezza (i due "cap" superiore/inferiore, dischi)
  - Calcolo normali diverse per superficie laterale vs cap
- **Cono** (bonus): stessa logica del cilindro ma con raggio che varia linearmente lungo l'asse
- Calcolo delle normali in ogni punto di intersezione (per sfera, piano, cilindro, cono)
- Gestione di intersezioni multiple: trovare la `t` minima positiva tra tutti gli oggetti della scena (hit più vicino alla camera)

### 1.3 Trasformazioni geometriche
- Traslazione (semplice somma vettoriale)
- Rotazione 3D: matrici di rotazione attorno agli assi X, Y, Z, oppure rotazione attorno a un asse arbitrario (Rodrigues' rotation formula — utile per orientare cilindro/cono/camera secondo il vettore dato nel file .rt)
- Come "orientare" un oggetto sapendo solo il suo vettore direzione (serve per camera, cilindro, cono, plane)

### 1.4 Modello della camera / generazione dei raggi
- Concetto di viewport / piano immagine virtuale
- Field of View (FOV) orizzontale → calcolo dimensioni del viewport
- Mappatura pixel (x,y) dello schermo → punto sul viewport → direzione del raggio
- Aspect ratio (larghezza/altezza immagine)
- Costruzione della base ortonormale della camera (forward, right, up) a partire da direzione + FOV

### 1.5 Illuminazione (lighting model)
- **Ambient light**: colore costante applicato a tutti gli oggetti (oggetti mai completamente al buio)
- **Diffuse (Lambertian) reflectance**: `intensità = max(0, dot(normal, light_dir)) * light_brightness`
- **Specular / Phong reflection** (bonus): componente `reflect(light_dir, normal)` confrontata con la direzione verso la camera, elevata a un esponente (shininess)
- Come combinare ambient + diffuse (+ specular) e clampare il colore finale in [0,255]
- **Hard shadows**: shadow ray dal punto di intersezione verso la luce, se interseca un altro oggetto prima di raggiungere la luce → il punto è in ombra (attenzione al bias/epsilon per evitare "shadow acne")

### 1.6 Texture e pattern (bonus)
- Checkerboard pattern: come mappare coordinate UV su sfera/piano/cilindro per alternare due colori
- Bump mapping: concetto di normal perturbation tramite una heightmap/texture

### 1.7 Parsing e gestione file
- Lettura file `.rt`: tokenizzazione riga per riga
- Validazione rigorosa degli input (range validi: colori 0-255, ratio 0.0-1.0, vettori normalizzati in [-1,1], ecc.)
- Gestione errori pulita: nessun crash, sempre "Error\n" + messaggio, libera tutta la memoria allocata fino a quel momento
- Regole del formato: elementi A/C/L unici, ordine libero, spazi/linee multiple ammesse

### 1.8 MiniLibX
- Inizializzazione: `mlx_init`, `mlx_new_window`
- Creazione immagine in memoria: `mlx_new_image`, `mlx_get_data_addr` (per scrivere pixel direttamente nel buffer, molto più veloce di `mlx_pixel_put`)
- Loop eventi: `mlx_hook` / `mlx_key_hook` per ESC, `mlx_hook` per il tasto "chiudi finestra" (evento 17), `mlx_loop`
- Come mettere l'immagine a schermo: `mlx_put_image_to_window`
- Gestione del refresh fluido (ridisegnare solo quando serve, non bloccare durante resize/minimize)

### 1.9 C / Norma / Project management 42
- Norma 42 (anche per file bonus!)
- Gestione della memoria: no leak, `free` sistematico, uso di `valgrind` / verifica con strumenti simili
- Gestione robusta di malloc falliti, file non apribili, ecc. (niente segfault/bus error/double free)
- Struttura del Makefile: regole `$(NAME)`, `all`, `clean`, `fclean`, `re`, `bonus`, no relink inutili, flags `-Wall -Wextra -Werror -lm`
- Se si usa libft: cartella `libft` con proprio Makefile richiamato dal Makefile principale

### 1.10 Bonus opzionali (solo se il mandatory è perfetto)
- Phong reflection completo (specular)
- Checkerboard pattern
- Luci colorate e multi-spot (gestire una lista di luci invece di una sola, sommare i contributi)
- Un secondo oggetto quadrico: cono, iperboloide o paraboloide
- Bump mapping da texture

---

## 2. SUDDIVISIONE DEL LAVORO (2 persone)

Divisione consigliata per **competenza trasversale**, non per "un modulo a testa isolato": in un progetto così matematico è meglio che entrambi capiscano TUTTA la teoria (par. 1.1–1.5), ma che il lavoro pratico sia diviso per non pestarvi i piedi sullo stesso file.

**Persona A → "Motore matematico / rendering core"**
**Persona B → "Parsing / struttura dati / finestra e display"**

### Fase 0 — Studio teorico condiviso (entrambi, insieme)
| Attività | Tempo stimato |
|---|---|
| Ripasso vettori, dot/cross product, normali, riflessione | 3h |
| Studio formule intersezione sfera/piano/cilindro (derivazione su carta) | 4h |
| Studio modello camera + generazione raggi + FOV | 2h |
| Studio modello di illuminazione (ambient/diffuse/hard shadow) | 2h |
| Lettura Norma 42 + regole Makefile/bonus | 1h |
| **Totale Fase 0** | **~12h (in coppia)** |

### Fase 1 — Setup e fondamenta (parallelo)
| Chi | Compito | Tempo stimato |
|---|---|---|
| A | Libreria matematica: `t_vec3` (add, sub, scale, dot, cross, normalize, length, reflect, rotazione attorno ad asse arbitrario) + test unitari | 6-8h |
| B | Struttura dati scena (`t_scene`, `t_camera`, `t_light`, `t_object` come union/tagged struct), Makefile, setup libft/minilibx, scheletro progetto | 5-7h |

### Fase 2 — Parsing (Persona B, con supporto di A per validazione)
| Compito | Tempo stimato |
|---|---|
| Tokenizer riga per riga (split su spazi/newline multipli) | 3h |
| Parser per A, C, L (con validazione range) | 3h |
| Parser per sp, pl, cy (con validazione range) | 4h |
| Gestione errori completa ("Error\n" + msg, free su ogni errore) + set di file .rt di test validi/invalidi | 4h |
| **Totale Fase 2** | **~14h** |

### Fase 3 — Camera e generazione raggi (Persona A)
| Compito | Tempo stimato |
|---|---|
| Costruzione base ortonormale camera da direction vector | 2h |
| Calcolo viewport da FOV + aspect ratio | 2h |
| Generazione raggio primario per ogni pixel | 2h |
| **Totale Fase 3** | **~6h** |

### Fase 4 — Intersezioni raggio-oggetto (Persona A, cuore del progetto)
| Compito | Tempo stimato |
|---|---|
| Ray-sphere (+ normale) | 3h |
| Ray-plane (+ normale) | 2h |
| Ray-cylinder infinito + cap (+ normali) — la parte più ostica | 8-10h |
| Funzione "closest hit" su tutta la scena | 2h |
| Debug visivo (render oggetti con colore piatto, senza luce, per validare le geometrie) | 3h |
| **Totale Fase 4** | **~20h** |

### Fase 5 — Finestra e display (Persona B, in parallelo alla Fase 4)
| Compito | Tempo stimato |
|---|---|
| Setup mlx (init, window, image buffer, put pixel via `mlx_get_data_addr`) | 3h |
| Hook ESC + hook chiusura finestra + loop | 2h |
| Collegamento render loop → buffer immagine → finestra | 2h |
| **Totale Fase 5** | **~7h** |

### Fase 6 — Illuminazione (Persona A e B insieme, è il collante tra le due parti)
| Compito | Tempo stimato |
|---|---|
| Ambient + diffuse lighting | 3h |
| Shadow ray + hard shadows (con epsilon bias) | 4h |
| Clamping colori e color mixing corretto | 1h |
| **Totale Fase 6** | **~8h** |

### Fase 7 — Integrazione, debug, scene di test
| Compito | Tempo stimato |
|---|---|
| Creare un set completo di scene .rt (una per feature, come richiesto per la difesa) | 3h |
| Debug incrociato (ognuno testa il codice dell'altro) | 4h |
| Check memory leak (valgrind), edge case (camera dentro un oggetto, oggetti sovrapposti, ecc.) | 4h |
| Check Norma su tutti i file | 2h |
| **Totale Fase 7** | **~13h** |

### Fase 8 — README + refactor finale
| Compito | Tempo stimato |
|---|---|
| README.md (descrizione, istruzioni, risorse, uso IA dichiarato) | 2h |
| Refactor/pulizia codice, commenti dove serve | 3h |
| **Totale Fase 8** | **~5h** |

**TOTALE PARTE MANDATORIA: ~85h di lavoro (combinate tra le 2 persone), circa 45-50h a testa se ben parallelizzato.**

---

### Fase 9 — Bonus (solo dopo mandatory perfetto)
| Chi | Compito | Tempo stimato |
|---|---|---|
| A | Specular reflection (Phong completo) | 4h |
| A | Cono come secondo oggetto quadrico | 6-8h |
| B | Multi-spot light + luci colorate (somma contributi di più luci) | 4h |
| B | Checkerboard pattern (mapping UV su piano/sfera) | 5h |
| Entrambi | Bump mapping (se c'è tempo) | 8h+ |

**TOTALE BONUS (senza bump mapping): ~19-21h**
**TOTALE BONUS (con bump mapping): ~27-29h**

---

## 3. NOTE PRATICHE PER LAVORARE IN 2

- **Definite subito le interfacce** (header condivisi: struct `t_vec3`, `t_ray`, `t_object`, prototipi funzioni) prima di scrivere codice, così potete lavorare in parallelo su file diversi senza conflitti continui su Git.
- Usate branch Git separati per Fase 4 (A) e Fase 2/5 (B), merge dopo che entrambe le parti passano i test base.
- Committate spesso, con scene .rt di test versionate nel repo (non fanno male al norm check se in cartella dedicata, es. `scenes/`).
- Per la difesa: **preparatevi entrambi a spiegare TUTTO**, non solo la parte che avete scritto — è il rischio principale del lavoro in coppia su 42 (vedi Capitolo III del subject sull'uso dell'IA: la stessa logica vale per il lavoro col partner).
- Iniziate a testare la geometria (Fase 4) con colori piatti, SENZA luce: se le forme sono giuste, l'illuminazione è molto più facile da debuggare dopo.
