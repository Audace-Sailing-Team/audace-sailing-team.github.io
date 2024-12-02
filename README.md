# Sensoristica - Sito web interno
Sito web interno per il reparto di Sensoristica di Audace Sailing
Team.

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

