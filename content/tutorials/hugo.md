---
title: 'Guida rapida alla modifica del sito'
date: 2026-07-21
author: 'Iacopo Ricci, Zio GPT'
weight: 1
---
# Le Basi
## Guida all'uso 
Per modificare i contenuti del sito, segui questi passaggi:

1. **Ottieni l'accesso**: contatta gli autori del sito fornendo il tuo
   username GitHub per avere l'accesso al repository
2. **Clona il repository**:
   ```bash
	   git clone git@github.com:Audace-Sailing-Team/audace-sailing-team.github.io.git
	   cd audace-sailing-team.github.io
   ```

### Modificare una pagina del sito
Per modificare i contenuti del sito, è necessario conoscere le basi del
Markdown. Nella cartella `contents/` sono presenti i contenuti per le
varie pagine del sito (*e.g.* `contents/dashboard/_index.md` per la *Dashboard*,
`contents/dashboard/todos.md` per i *To-do*, `contents/docs/_index.md` per la
*Documentazione*).  
Prima di fare il push su github è consigliato tramite il comando ``` hugo server ``` visualizzare il sito in locale sul proprio device in modo da controllare che tutto funzioni. Per problemi con hugo leggere la sezione [Problemi comuni](#problemi-comuni) 

### Commits
Una volta modificati i contenuti del sito, crea un commit con i file
che hai appena modificato.

Ad esempio, se la modifica riguarda `contents/homepage.md`
```bash
	git add contents/_index.md
	git commit -m "Update deadlines"
	git push
```

Segui questa [guida](https://cbea.ms/git-commit/) per scrivere messaggi di commit chiari e leggibili.

# Stuttura

Le cose relative all'anno passato vanno spostate nella cartella archivio divise secondo le varie cartelle.
 - ordini: tutti gli ordini di componenti hardware fatti per il reparto
 - Reports e Debrief: tutti i report sui lavori, debrief pre/post sumoth, ecc


# Problemi comuni

## Versione di Hugo

La versione di hugo richiesta per eseguire live sul proprio device la pagina è la 0.119.0 che potete trovare al seguente link:  
https://github.com/gohugoio/hugo/releases/tag/v0.119.0  
Non aggiornatela se no entra in conflitto con quella specificata nei file del sito.  
Una volta installato la versione di hugo coretta andate nella cartella `audace-sailing-team.github.io/themes/hugo-whisper-theme` se la trovate vuota tornate indietro nella cartella `audace-sailing-team.github.io/themes` ed eseguite il comando `git submodule update --init --recursive`