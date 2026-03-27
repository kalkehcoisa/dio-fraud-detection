# Detecção de Fraudes com Cartões de Crédito

Este projeto implementa modelos de machine learning para detecção de transações fraudulentas, utilizando o dataset **Credit Card Fraud Detection**, amplamente utilizado na literatura por seu extremo desbalanceamento de classes.

---

## 📊 Dataset

### Origem
[Kaggle - Credit Card Fraud Detection](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)  
Publicado por Andrea Dal Pozzolo et al. no trabalho *"Calibrating Probability with Undersampling for Unbalanced Classification"* (CIDM, IEEE, 2015).

### Estrutura
- **Total de transações:** 284.807
- **Fraudes (Classe 1):** 492 (0,1727%)
- **Legítimas (Classe 0):** 284.315 (99,8273%)

| Coluna       | Descrição |
|--------------|-----------|
| `Time`       | Segundos decorridos desde a primeira transação |
| `V1` … `V28` | Features anonimizadas via PCA |
| `Amount`     | Valor da transação |
| `Class`      | Variável alvo (0 = legítima, 1 = fraude) |

### Por que este dataset?
- **Desbalanceamento extremo** – desafio realista para modelos de detecção de fraude.
- **Features anonimizadas** – foco na modelagem, sem necessidade de interpretação de contexto específico.
- **Dados limpos** – sem valores nulos ou inconsistentes.

---

## 🛠️ Como Reproduzir

1. Clone este repositório
2. Instale as dependências:
   ```bash
   poetry install
   poetry run jupyter notebook```
3. Execute o notebook `analise_crard_fraud.ipynb`

---

## 🧹 Pré‑processamento

### Análise Inicial
- **Valores ausentes:** nenhum.
- **Outliers:** mantidos, pois podem representar padrões de fraude.

### Normalização
Apenas as colunas `Time` e `Amount` foram normalizadas com **`RobustScaler`** (mediana + IQR), escolhido por ser menos sensível a outliers e preservar a distribuição dos dados.

---

## 🤖 Modelos Avaliados

Dois modelos foram treinados para servir como baseline e modelo mais avançado:

1. **Regressão Logística**  
   - Parâmetro `class_weight='balanced'` para compensar o desbalanceamento.
   - Baseline simples e interpretável.

2. **XGBoost**  
   - Parâmetro `scale_pos_weight` ajustado com base na proporção entre classes.
   - Modelo baseado em árvores, capaz de capturar relações não‑lineares.

---

## 📈 Resultados

### Métricas da Classe Fraude (Classe 1)

| Modelo              | Precision | Recall | F1-Score | AUC    |
|---------------------|-----------|--------|----------|--------|
| Regressão Logística | 0.07      | 0.88   | 0.12     | 0.9679 |
| XGBoost             | 0.88      | 0.79   | 0.83     | 0.9705 |

### Matrizes de Confusão

**Regressão Logística**

    Verdadeiros Negativos: 83.482
    Falsos Positivos:       1.813
    Falsos Negativos:          18
    Verdadeiros Positivos:    130

**XGBoost**

    Verdadeiros Negativos: 85.279
    Falsos Positivos:          16
    Falsos Negativos:          31
    Verdadeiros Positivos:    117

---

## 🧠 Principais Aprendizados

- **Tratamento do desbalanceamento** é fundamental – técnicas como `class_weight` e `scale_pos_weight` mostraram‑se eficazes.
- **Manter outliers** pode ser benéfico, pois eles podem representar justamente os padrões anômalos que se deseja detectar.
- **Acurácia é uma métrica enganosa** em cenários desbalanceados – optou‑se por Precision, Recall, F1‑Score e AUC.
- **XGBoost superou a regressão logística** em todos os aspectos, reduzindo drasticamente os falsos positivos (de 1.813 para 16) e mantendo recall elevado (79%).

---

## 🚀 Próximos Passos

- Ajuste de hiperparâmetros no XGBoost (`max_depth`, `learning_rate`, `n_estimators`).
- Exploração de outros modelos como LightGBM, CatBoost ou redes neurais.
- Análise de importância das features (V1 … V28).
- Ajuste do threshold de decisão para balancear precisão e recall conforme necessidade de negócio.
- Implementação em pipeline de streaming para simular ambiente de produção.

---

## 📁 Arquivos do Projeto

- `analise_crard_fraud.html` – notebook completo exportado com todas as etapas de análise, pré‑processamento e modelagem.
- `README.md` – este arquivo.

---

## 📚 Referências

- Dal Pozzolo, A., Caelen, O., Johnson, R. A., & Bontempi, G. (2015). *Calibrating Probability with Undersampling for Unbalanced Classification.* In Symposium on Computational Intelligence and Data Mining (CIDM), IEEE.
- Kaggle Dataset: [Credit Card Fraud Detection](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)

---

## 👨‍💻 Autor

Desenvolvido como parte de um estudo prático sobre detecção de fraudes com machine learning.
