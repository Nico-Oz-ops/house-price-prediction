# 🏠 House Price Prediction — Kaggle Dataset

> 🇮🇹 [Leggi in Italiano](#-descrizione-del-progetto) | 🇬🇧 [Read in English](#-project-description)

---

## 🇮🇹 Descrizione del Progetto

Progetto di Machine Learning per la previsione del prezzo di vendita di immobili residenziali, sviluppato nell'ambito del percorso **Professional Data Analyst** presso **ITS ICTAccademy** (Roma).

L'obiettivo è costruire un modello di regressione in grado di stimare il valore di mercato di un'abitazione a partire dalle sue caratteristiche principali, simulando un **Automated Valuation Model (AVM)** — lo stesso strumento usato da portali immobiliari e agenzie per valutare automaticamente gli immobili.

---

## Obiettivo

Prevedere il prezzo di vendita (`SalePrice`) di un immobile residenziale a partire da variabili come superficie, qualità, quartiere ed età della casa, con l'obiettivo di supportare:

- **Agenti immobiliari** — nella definizione del prezzo di listino ottimale
- **Acquirenti** — per valutare se un immobile è sopra o sotto mercato
- **Portali online** — come feature di stima automatica del valore

---

## Struttura della Repository

```
house-price-prediction/
│
├── README.md
└── HOUSEPRICE.zip    # Export del Flow Dataiku
```

### Come importare il Flow in Dataiku

1. Scarica il file `HOUSEPRICE.zip`
2. Apri **Dataiku DSS**
3. Clicca **New Project** → **Import project**
4. Carica il file zip → il Flow si ricostruisce automaticamente

---

## Dataset

| Caratteristica | Dettaglio |
|---|---|
| Fonte | [House Prices: Advanced Regression Techniques — Kaggle](https://www.kaggle.com/c/house-prices-advanced-regression-techniques) |
| Righe | 1.460 immobili |
| Colonne totali | 81 variabili |
| Variabili utilizzate | 17 selezionate + 2 create tramite feature engineering |
| Variabile target | `SalePrice` (prezzo di vendita in $) |

### Variabili principali utilizzate

| Categoria | Variabili |
|---|---|
| **Dimensione** | `GrLivArea`, `TotalBsmtSF`, `LotArea`, `TotalSF` ✦ |
| **Qualità e condizione** | `OverallQual`, `OverallCond`, `BsmtQual` |
| **Caratteristiche** | `BedroomAbvGr`, `FullBath`, `GarageCars`, `GarageType` |
| **Posizione e tempo** | `Neighborhood`, `HouseAge` ✦, `YearBuilt` |
| **Target** | `SalePrice` → trasformato in `log_SalePrice` |

> ✦ Variabile creata tramite feature engineering

---

## Tool e Tecnologie

| Tool | Utilizzo |
|---|---|
| **Dataiku DSS** | Pipeline ML end-to-end, preprocessing, modellazione, scoring |

---

## Pipeline del Progetto

```
house_raw
    ↓ [Prepare Recipe — Pulizia & Feature Engineering]
house_clean
    ↓ [Prepare Recipe — Encoding]
house_features
    ↓ [Visual ML — Ridge Regression & Gradient Boosted Trees]
    ↓                        ↓
Ridge Regression     Gradient Boosted Trees 🏆
    ↓
[Score Recipe]
    ↓
house_predictions
```

---

## Preparazione dei Dati

### Gestione valori mancanti

| Variabile | Strategia |
|---|---|
| `LotFrontage` (17.7% mancanti) | Imputazione con media per quartiere (`Neighborhood`) |
| `GarageType` (5.5% mancanti) | Sostituzione con categoria `"Nessuno"` |
| `BsmtQual` (2.5% mancanti) | Sostituzione con categoria `"Nessuno"` |

### Feature Engineering

| Nuova variabile | Formula | Significato |
|---|---|---|
| `HouseAge` | `YrSold - YearBuilt` | Età della casa al momento della vendita |
| `TotalSF` | `TotalBsmtSF + 1stFlrSF + 2ndFlrSF` | Superficie totale dell'immobile |
| `log_SalePrice` | `ln(SalePrice + 1)` | Target trasformato per ridurre l'asimmetria |
| `LogTotalSF` | `ln(TotalSF + 1)` | Superficie totale in scala logaritmica |
| `LogLotArea` | `ln(LotArea + 1)` | Dimensione lotto in scala logaritmica |
| `LogGrLivArea` | `ln(GrLivArea + 1)` | Superficie zona giorno in scala logaritmica |

### Encoding

One-hot encoding applicato a: `Neighborhood` (25 categorie), `GarageType` (7 categorie), `BsmtQual` (5 categorie)

---

## Modelli

### Confronto tra i due modelli

| Metrica | Gradient Boosted Trees 🏆 | Ridge Regression |
|---|---|---|
| **R² Score** | **0.872** | 0.836 |
| **RMSE** | **0.1356** | 0.1534 |
| **MAPE** | **0.79%** | 0.78% |
| **MAE** | **0.09463** | 0.09316 |
| **Pearson** | **0.936** | 0.921 |
| **Skewness residui** | -1.54 | -4.205 |

Il **Gradient Boosted Trees** è il modello selezionato per il deployment, grazie a un R² più alto e residui significativamente più stabili.

---

## Insight Principali

L'analisi della feature importance e i valori SHAP mostrano che le variabili più influenti sul prezzo sono:

1. **LogTotalSF** (25%) — più la casa è grande, più il prezzo sale
2. **OverallQual** (17%) — più alta è la qualità delle finiture, più il prezzo aumenta
3. **HouseAge** (12%) — più la casa è vecchia, più il prezzo scende
4. **OverallCond** (9%) — più la casa è in buono stato, più il prezzo cresce
5. **LogLotArea** (7%) — più grande è il lotto, più il prezzo sale

> *"Più grande, più nuova, più curata = vale di più. I dati confermano l'intuizione degli esperti."*

---

## Presentazione del Progetto

[Visualizza la presentazione su Gamma](https://gamma.app/docs/Quanto-vale-casa-tua-3or725m2inyj29u)

---

## Autore

**Nico Valenzuela**
Studente Professional Data Analyst — ITS ICTAccademy, Roma
[GitHub](https://github.com/Nico-Oz-ops)
---
---

## 🇬🇧 Project Description

Machine Learning project for predicting residential property sale prices, developed as part of the **Professional Data Analyst** program at **ITS ICTAccademy** (Rome).

The goal is to build a regression model capable of estimating the market value of a property based on its main characteristics, simulating an **Automated Valuation Model (AVM)** — the same tool used by real estate portals and agencies for automated property valuation.

---

## Goal

Predict the sale price (`SalePrice`) of a residential property using variables such as area, quality, neighborhood, and house age, to support:

- **Real estate agents** — in setting the optimal listing price
- **Buyers** — to assess whether a property is above or below market value
- **Online portals** — as an automated property valuation feature

---

## Dataset

| Feature | Detail |
|---|---|
| Source | [House Prices: Advanced Regression Techniques — Kaggle](https://www.kaggle.com/c/house-prices-advanced-regression-techniques) |
| Rows | 1,460 properties |
| Total columns | 81 variables |
| Variables used | 17 selected + 2 created via feature engineering |
| Target variable | `SalePrice` (sale price in $) |

---

## Models & Results

| Metric | Gradient Boosted Trees 🏆 | Ridge Regression |
|---|---|---|
| **R² Score** | **0.872** | 0.836 |
| **RMSE** | **0.1356** | 0.1534 |
| **MAPE** | **0.79%** | 0.78% |
| **Pearson** | **0.936** | 0.921 |

The **Gradient Boosted Trees** model was selected for deployment, explaining **87.2% of the variation in house prices** with an average error of less than 1%.

---

## Key Insights

The top 5 most important features according to SHAP analysis:

1. **LogTotalSF** (25%) — larger houses are worth more
2. **OverallQual** (17%) — higher finish quality increases price significantly
3. **HouseAge** (12%) — older houses are worth less
4. **OverallCond** (9%) — better condition means higher price
5. **LogLotArea** (7%) — larger lots increase property value

---

## Project Presentation

[View the presentation on Gamma](https://gamma.app/docs/Quanto-vale-casa-tua-3or725m2inyj29u)

---

## Author

**Nico Valenzuela**
Professional Data Analyst Student — ITS ICTAccademy, Rome
[GitHub](https://github.com/Nico-Oz-ops)

