# Credit Card Fraud Detection — Analisi Esplorativa e Qualità del Dato

Analisi di un dataset reale di transazioni con carta di credito, con l'obiettivo di
comprendere la struttura del fenomeno **frode** prima di qualsiasi modellazione.
Il progetto mette al centro la **qualità del dato**: prima di costruire un modello,
si verifica cosa i dati dicono davvero e dove ingannano.

## Il dataset

[Credit Card Fraud Detection — Kaggle](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)

- **284.807** transazioni, di cui **492** frodi (0,17%) - dataset fortemente sbilanciato
- Feature `V1`-`V28` anonimizzate (output di una PCA), più `Time`, `Amount` e la variabile target `Class`

## Struttura del progetto

| Notebook | Contenuto | Stato |
| --- | --- | --- |
| `01_EDA.ipynb` | Analisi esplorativa e qualità del dato (14 sezioni) | ✅ Completo |
| `02_modeling.ipynb` | Modellazione, con confronto dati "sporchi" vs "puliti" | 🔜 In arrivo |
| `03_explainability.ipynb` | Interpretabilità e governance del modello (SHAP) | ⏳ Eventuale |

## Cosa contiene il notebook 1

- Struttura, completezza formale e sostanziale, duplicati
- `Amount`: dominio, range, percentili, coda, rappresentazioni grafiche a confronto
- `Time`: ricostruzione del ciclo giornaliero, variabile *Ora* derivata, tasso di frode per ora
- `V1`-`V28`: confronto tra distribuzioni delle transazioni legittime e fraudolente
- Correlazioni, interazioni, riepilogo

## Risultati principali

- **Duplicati**: 1.081 record duplicati, con tasso circa **10 volte più alto sulle frodi** (3,9% contro 0,37%) - un segnale di qualità del dato che influenza direttamente il training.
- **IQR inapplicabile**: il metodo IQR classifica come outlier l'**11,2%** delle transazioni. Su una distribuzione fortemente asimmetrica come `Amount` è uno strumento fuorviante, non un filtro.
- **Frodi sui micro-importi**: il **44,4%** delle frodi è sotto i 5 € (contro il 23,6% delle transazioni legittime). Ricorrono importi "sonda" - 1 € (105 volte), 99,99 (27), 0 (25).
- **Picco ipotizzato notturno**: nella fascia oraria 2–4 il tasso di frode è **1,45%** contro lo 0,17% medio - quasi **9 volte** superiore.
- **La correlazione nasconde la relazione**: il tasso di frode per fascia di importo descrive una **U**, e questo spiega perché la correlazione lineare con `Amount` è appena 0,006. La relazione esiste, ma non è lineare.
- **Due comportamenti distinti**: le frodi nella fascia 2-4 hanno importo mediano di **1 €**, contro i 19,59 € delle frodi nelle altre ore - profili di attacco diversi, non un unico fenomeno.

## Stack tecnico

- **Notebook 01 - EDA**: Python · pandas · NumPy · Matplotlib · Seaborn · Jupyter Notebook
- **Notebook 02 - modellazione**: scikit-learn · XGBoost
- **Notebook 03 - interpretabilità**: SHAP

## Come eseguire

```bash
pip install pandas numpy matplotlib seaborn jupyter
jupyter notebook 01_EDA.ipynb
```

Il dataset `creditcard.csv` va scaricato da Kaggle e collocato nella cartella del progetto
(non è incluso nel repository per dimensioni e licenza).

---

*Progetto di analisi dati — Claudia Cerasale, 2026*
