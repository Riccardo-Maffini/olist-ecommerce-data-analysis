# Olist E-commerce Analysis

Analisi end-to-end del dataset e-commerce brasiliano [Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) (9 tabelle, 2016-2018): pulizia dei dati in Python, analisi esplorativa e dashboard Power BI su vendite, logistica e clienti.

## Pipeline

| Fase | File | Contenuto |
|---|---|---|
| 1. Cleaning | `cleaning.ipynb` | Controllo di tipi, valori mancanti e anomalie su ogni tabella; output in `dfs_clean.pkl` |
| 2. EDA | `eda_fixed.ipynb` | 9 blocchi (vendite, categorie, ordini, pagamenti, recensioni, venditori, logistica, geografia, clienti) ed export dei CSV puliti |
| 3. Dashboard | `dashboard/olist.pbix` | 3 pagine Power BI: vendite, logistica e geografia, clienti e recensioni |

## Dashboard

File `dashboard/olist.pbix` (da aprire con Power BI Desktop), 3 pagine:

- **Panoramica vendite:** fatturato, ordini, valore medio, andamento mensile, top 10 categorie.
- **Logistica e geografia:** tempi di consegna e ritardi, confronto tra stati.
- **Clienti e recensioni:** clienti unici, riacquisto, distribuzione dei voti.

## Risultati principali

- **Fatturato** 13,59 mln R$ su 98.666 ordini (valore medio ordine ≈ 137,75 R$); crescita forte nel 2017, picco a novembre 2017.
- **Concentrazione:** le prime 5 categorie valgono il 40% del fatturato; il 10% dei venditori genera il 67,5%.
- **Logistica:** consegna mediana di 10 giorni, solo il 6,8% degli ordini in ritardo; São Paulo ~8 giorni contro 23-28 giorni per gli stati del Nord.
- **Distanza:** mediana di 432 km in linea d'aria tra cliente e venditore; correlazione con i giorni di consegna 0,39, quindi spiega solo in parte i tempi.
- **Clienti:** 93.358 clienti unici, riacquisto solo del 3,0%.
- **Soddisfazione:** score medio 4,09; il 58% dei voti è 5, l'11,5% è 1.

## Scelte metodologiche

- Join con `validate=` e controllo delle righe, per evitare duplicazioni.
- Recensioni aggregate per ordine prima di ogni join (547 ordini ne hanno più di una).
- Date impossibili non corrette ma escluse dai soli calcoli di durata.
- Unica eliminazione di righe: 42 coordinate fuori dal Brasile (non correggibili).
- Mesi con meno di 50 ordini e categorie con meno di 30 recensioni esclusi dai confronti.
- Coordinate per CAP calcolate con la media (una mediana sarebbe più robusta ai punti geocodificati male).

## Limiti

- Il tasso di riacquisto è misurato su circa un anno di dati vicino al lancio, quindi è probabilmente sottostimato.
- La mappa di Power BI non era utilizzabile: è sostituita da un grafico a dispersione per stato.
- La segmentazione RFM e la distanza tra cliente e venditore sono presenti solo nell'EDA, non nella dashboard.

## Come riprodurlo

1. Scarica i 9 CSV da [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) e mettili in `data/raw/`.
2. Crea l'ambiente: `conda env create -f datawork.yml`
3. Esegui in ordine `cleaning.ipynb` ed `eda_fixed.ipynb`: l'ultima sezione dell'EDA esporta i CSV in `data/processed/csv`.
4. Apri `dashboard/olist.pbix` con Power BI Desktop e, se serve, punta l'origine dati a quella cartella.

## Strumenti

Python (pandas, matplotlib), JupyterLab, Power BI (DAX, Power Query), Git.
