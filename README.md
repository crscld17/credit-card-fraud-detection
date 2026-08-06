# Credit Card Fraud Detection

Progetto di analisi e modellazione su un dataset reale di transazioni con carta di credito, dall'analisi esplorativa alla costruzione di un modello di rilevamento frodi. Il filo conduttore è la **qualità del dato** e il **metodo**: ogni scelta viene verificata sui dati e motivata, invece di essere assunta.

## Il dataset

[Credit Card Fraud Detection — Kaggle](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)

- **284.807** transazioni, di cui **492** frodi (0,17%) - dataset fortemente sbilanciato
- Feature `V1`-`V28` anonimizzate (output di una PCA), piu `Time`, `Amount` e la variabile target `Class`

## Struttura del progetto

| Notebook | Contenuto | Stato |
| --- | --- | --- |
| `01_EDA.ipynb` | Analisi esplorativa e qualità del dato (14 sezioni) | ✅ Completo |
| `02_modeling.ipynb` | Modellazione: baseline, pipeline, confronto tra modelli, soglia | ✅ Completo |
| `03_data_quality_impact.ipynb` | Impatto della qualità del dato: modello su dati puliti vs con duplicati | 🔜 In arrivo |
| `04_explainability.ipynb` | Interpretabilità del modello con SHAP | ⏳ Pianificato |
| `05_tuning.ipynb` | Ottimizzazione degli iperparametri | ⏳ Pianificato |

## Notebook 01 — Analisi esplorativa

Esplorazione del dataset con focus sulla qualità del dato, prima di qualsiasi modellazione.

**Risultati principali:**
- **Duplicati**: 1.081 record duplicati, con tasso circa **10 volte piu alto sulle frodi** (3,9% contro 0,37%) - un segnale di qualità del dato che influenza direttamente il training.
- **IQR inapplicabile**: il metodo IQR classifica come outlier l'**11,2%** delle transazioni. Su una distribuzione fortemente asimmetrica come `Amount` e uno strumento fuorviante.
- **Frodi sui micro-importi**: il **44,4%** delle frodi e sotto i 5 € (contro il 23,6% delle transazioni legittime). Ricorrono importi "sonda" - 1 € (105 volte), 99,99 (27), 0 (25).
- **Concentrazione in una fascia oraria**: nella fascia a basso volume (ore 2-4 del ciclo derivato) il tasso di frode è **1,45%** contro lo 0,17% medio - quasi **9 volte** superiore. L'ora è derivata da `Time` e la fascia non è databile con certezza a un orario reale.
- **La correlazione nasconde la relazione**: il tasso di frode per fascia di importo descrive una **U**, e questo spiega perchè la correlazione lineare con `Amount` è appena 0,006. La relazione esiste, ma non e lineare.
- **Due comportamenti distinti**: le frodi nella fascia 2-4 hanno importo mediano di **1 €**, contro i 19,59 € delle frodi nelle altre ore - profili di attacco diversi.

## Notebook 02 — Modellazione

Costruzione di un modello di rilevamento frodi, un esperimento alla volta, misurando ogni scelta.

**Percorso:**
- Baseline con regressione logistica, gestione dello sbilanciamento (`class_weight`), split stratificato
- Taratura della soglia decisionale con analisi del costo marginale (frodi guadagnate vs falsi allarmi)
- Rifattorizzazione in pipeline (con verifica anti-leakage) e test "scalare solo Amount vs tutte le feature"
- Esperimenti: trasformazione logaritmica di `Amount`, codifica ciclica di `Ora` (seno/coseno), confronto con **XGBoost**

**Risultato:** XGBoost si e rivelato nettamente superiore alla regressione logistica (PR-AUC da 0,678 a 0,800). Il modello finale (XGBoost, soglia 0,2) individua l'83% delle frodi con una precisione del 48%, contro il 5,5% del baseline - una riduzione dei falsi allarmi da migliaia a poche decine.

## Stack tecnico

- **Notebook 01 (EDA)**: Python · pandas · NumPy · Matplotlib · Seaborn · Jupyter
- **Notebook 02 (modellazione)**: scikit-learn · XGBoost

## Come eseguire

```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost jupyter
jupyter notebook
```

Il dataset `creditcard.csv` va scaricato da Kaggle e collocato nella cartella del progetto (non e incluso nel repository per dimensioni e licenza).

---

*Progetto di analisi dati - Claudia Cerasale, 2026*
