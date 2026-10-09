# Tech Challenge — Fase 2 | POSTECH Data Analytics

## 1. Identificação

| Campo | Valor |
|---|---|
| Turma | 2DTATBB
| Grupo | 48
| Data de entrega | 10/10/2026

### Integrantes

| Nome completo | RM | E-mail |
|---|---|---|
| DAVI LEAL DE ARAÚJO | RM377830 | davilealdearaujo@gmail.com|
| DÉBORA COSTA BRASILINO | RM377831 | deborabrasilino@yahoo.com.br |
| JULIANA DIAS LUBACHESKI |RM377804 |julianalubacheski@gmail.com |
| NANCY LORENA MONTANO RIVERA | RM378100 | nlmrivera@gmail.com |

---

## 2. Links da entrega

Estes três links são **obrigatórios** e devem ser idênticos aos do PDF de submissão.

| Item | Link |
|---|---|
| Repositório | https://github.com/deborabrasilino-bb/tech-challenge-fase2-grupo48 |
| Vídeo executivo (≤ 5 min) | <!-- PREENCHER: YouTube não listado / Drive com acesso liberado --> |
| Apresentação | <!-- PREENCHER: link do arquivo em `docs/` ou Drive --> |

> ⚠️ Repositório privado ou inacessível inviabiliza a avaliação da entrega.
> Confira o acesso em uma janela anônima antes de enviar.

---

## 3. O problema

A concessão de crédito é fundamental para as instituições financeiras, mas envolve riscos associados à inadimplência. O objetivo deste projeto é desenvolver um modelo de aprendizado de máquina capaz de automatizar e otimizar a avaliação de pedidos de cartão de crédito. O modelo busca classificar os clientes como "bons" ou "maus" pagadores utilizando informações pessoais e financeiras, contribuindo para a mitigação de riscos e maior eficiência operacional.

### Variável alvo

A variável alvo (`mau_pagador`) foi criada a partir da coluna de status do histórico de pagamentos (`credit_record.csv`). O limiar adotado considerou como "Mau Pagador" (Classe 1) os clientes com atrasos superiores a 60 dias (estágios de inadimplência grave: 2, 3, 4 e 5). Os demais foram classificados como "Bons Pagadores" (Classe 0). Este limiar foca o modelo nos casos que geram real perda financeira para a instituição. A base é fortemente desbalanceada, com a esmagadora maioria pertencendo à Classe 0.

### Dataset

| Campo | Valor |
|---|---|
| Fonte | [Link dos Dados](https://drive.google.com/file/d/1z4yEyiCE_CGCWbvAAZQZSz-5-E5T5eYd/view?usp=sharing) |
| Linhas × colunas | 36.457 linhas e 18 colunas principais (antes do One-Hot Encoding) |
| Período / versão | |
| Licença de uso | Uso Acadêmico |

Descrição das principais variáveis:

| Variável | Tipo | Descrição |
|---|---|---|
| `ID` | Numérico | Número de identificação único do cliente. |
| `DAYS_BIRTH` | Numérico | Idade do cliente (contada em dias retroativos a partir de hoje). |
| `DAYS_EMPLOYED` | Numérico | Tempo de emprego do cliente (contado em dias retroativos). |
| `AMT_INCOME_TOTAL` | Numérico | Rendimento total anual do solicitante. |
| `NAME_INCOME_TYPE` | Categórico | Categoria de renda (ex: Assalariado, Servidor Público, Pensionista). |
| `NAME_EDUCATION_TYPE` | Categórico | Nível de escolaridade do cliente. |
| `NAME_FAMILY_STATUS` | Categórico | Estado civil do solicitante. |
| `FLAG_OWN_CAR_Y` | Numérico binário | Indica se o cliente possui carro próprio (1 = Sim, 0 = Não). |
| `FLAG_OWN_REALTY_Y` | Numérico binário | Indica se o cliente possui imóvel próprio (1 = Sim, 0 = Não). |
| `mau_pagador` | Numérico binário | Variável alvo criada (0 = Bom Pagador, 1 = Mau Pagador com atrasos graves). |

---

## 4. Como reproduzir

```bash
git clone https://github.com/deborabrasilino-bb/tech-challenge-fase2-grupo48
cd tech-challenge-fase2-grupo48

python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

pip install -r requirements.txt
jupyter notebook
```

Baixe o dataset e coloque o arquivo bruto em `data/raw/` (os dados **não** são versionados —
veja `data/README.md`).

Depois execute os notebooks nesta ordem:

| # | Notebook | O que faz |
|---|---|---|
| 1 | `notebooks/01_eda_grupo48.ipynb` | Análise exploratória |
| 2 | `notebooks/02_preprocessamento_grupo48.ipynb` | Limpeza, escala e feature engineering |
| 3 | `notebooks/03_modelagem_grupo48.ipynb` | Treino e comparação dos modelos |
| 4 | `notebooks/04_avaliacao_grupo48.ipynb` | Métricas, importância de variáveis e conclusões |

**Semente fixa:** `RANDOM_STATE = 42`, declarada na primeira célula de cada notebook.
Rodar os notebooks na ordem acima, a partir de um ambiente limpo, deve reproduzir
exatamente os números da seção 5.

---

## 5. Resultados

| Modelo | Acurácia | Precisão | Recall | F1 | AUC-ROC |
|---|---|---|---|---|---|
| Random Forest | 0.96 | 0.19 | 0.38 | 0.26 | 0.82 |


**Modelo escolhido:** Random Forest. A escolha se deu pela capacidade do algoritmo, baseado em árvores de decisão, de lidar nativamente com variáveis que possuem escalas muito diferentes e relações não-lineares com o risco de crédito.

**Métricas priorizadas:** O Recall da Classe 1 (Maus Pagadores) e o AUC-ROC foram priorizados. No contexto de risco de crédito, a penalidade financeira (perda de capital) ao aprovar um mau pagador (falso negativo) é substancialmente maior do que o custo de oportunidade de negar um cartão a um bom pagador (falso positivo).

---

## 6. Principais conclusões

**1. A maturidade é decisiva:** A idade do solicitante (DAYS_BIRTH) provou ser o fator de maior peso na aprovação do crédito, indicando que a maturidade pessoal tem forte correlação com a responsabilidade e o comportamento de pagamento.

**2. Estabilidade e capacidade são os pilares de risco:** O tempo no atual emprego (DAYS_EMPLOYED) e a Renda Total (AMT_INCOME_TOTAL) fecham o Top 3 de variáveis decisivas, validando matematicamente as práticas comuns de esteiras de crédito do mercado financeiro.

**3. Fatores secundários importam menos:** Variáveis patrimoniais periféricas, como possuir carro próprio ou tamanho da família, demonstraram um nível de importância secundário na discriminação entre bons e maus pagadores.
   
---

### Limitações e próximos passos

A limitação principal do projeto é o extremo desbalanceamento dos dados reais (~1,7% de inadimplentes na base de teste), que resultou em um grande volume de Falsos Positivos e um Recall moderado para os maus pagadores. Como próximos passos para evoluir a esteira de crédito, recomenda-se aplicar técnicas de reamostragem sintética (como o SMOTE) antes do treinamento ou testar modelos de Gradient Boosting (como XGBoost ou LightGBM).

---

## 7. Estrutura do repositório

```
.
├── data/          dados brutos (raw) e tratados (processed) — não versionados
├── notebooks/     análise em ordem numerada
└── docs/          apresentação executiva
```

Detalhes e convenções em [`ESTRUTURA.md`](ESTRUTURA.md).
Antes de enviar, percorra o [`CHECKLIST.md`](CHECKLIST.md).

---

## 8. Tecnologias

* **Python:** Linguagem base de todo o projeto.
* **Pandas:** Utilizada para a ingestão, cruzamento (merge), manipulação e limpeza da base de dados.
* **Scikit-Learn:** Empregada na aplicação do One-Hot Encoding, divisão das bases (treino e teste), construção dos pipelines (escalonamento) e treinamento do modelo de Random Forest, bem como na extração das métricas de avaliação.
* **Matplotlib & Seaborn:** Utilizadas em conjunto para a criação das análises visuais, incluindo histogramas da EDA, plotagem da matriz de confusão e extração gráfica da importância das variáveis (Feature Importance).
