# 🧠 Previsão de Estoque Inteligente na AWS com SageMaker Canvas

Este projeto foi desenvolvido como parte do desafio da **DIO (Digital Innovation One)** em parceria com a **AWS**. O foco foi a utilização do **Amazon SageMaker Canvas**, uma ferramenta **no-code** que permite criar, avaliar e implantar modelos de Machine Learning (ML) para Séries Temporais sem a necessidade de escrever código.

---

## 🎯 Objetivo do Projeto

O objetivo principal deste projeto foi desenvolver um modelo de **previsão de estoque** para a coluna `QUANTIDADE_ESTOQUE` de um dataset com 500 registros.

O escopo incluiu a aplicação do fluxo completo de ML no SageMaker Canvas: desde a importação do dado até a análise de desempenho do modelo e a geração de previsões (insights de negócio).

---

## 🛠️ Passo a Passo do Desenvolvimento

O desenvolvimento seguiu as quatro fases requeridas pelo desafio de Machine Learning no-code:

### 1. Seleção e Preparação do Dataset

O dataset escolhido e carregado no SageMaker Canvas foi o `dataset-500-curso-sagemaker-canvas-dio.csv`.

#### Estrutura do Dataset

| Coluna | Descrição | Tipo de Dado |
| :--- | :--- | :--- |
| `ID_PRODUTO` | Identificador único do item. | Categórico |
| `DIA` | Variável temporal (Data). | Temporal |
| `FLAG_PROMOCAO` | Indicador de promoção (0 ou 1). | Categórico |
| `QUANTIDADE_ESTOQUE` | **Coluna Alvo** (o que se deseja prever). | Numérico (Série Temporal) |

#### Configuração no Canvas
* **Coluna Alvo (Target):** `QUANTIDADE_ESTOQUE`.
* **Identificador Único (Item ID):** `ID_PRODUTO` (Configurado como categórico/texto para evitar que o modelo infira uma relação matemática pelo valor do ID).
* **Tipo de Problema:** Análise Preditiva de Dados (Séries Temporais).

### 2. Construção e Treinamento do Modelo

Devido ao tamanho do dataset (500 linhas) e para agilizar o processo para fins de demonstração, foi utilizada a opção **Quick Build**.
> **Observação:** A opção *Quick Build* é mais rápida, mas gera um modelo menos preciso em comparação com a *Standard Build*, que utiliza o conjunto completo de dados e mais tempo de processamento.

### 3. Análise do Modelo e Métricas de Performance

Após o treinamento, as métricas de qualidade (Model Status) foram examinadas para validar o desempenho do modelo.

#### Métricas Obtidas
O modelo demonstrou ser altamente performático para o contexto do desafio, conforme as métricas de erro:

| Métrica | Valores |
| :--- | :--- |
| **MASE** | **0.180** |
| MAPE | 0.290 | 
| WAPE | 0.152 | 
| RMSE | 1.535 | 
| Avg. wQL | 0.086 | 

#### Análise de Impacto das Características (Feature Impact)

* A coluna `FLAG_PROMOCAO` apresentou **0% de impacto** na previsão de `QUANTIDADE_ESTOQUE`.

**✅ Insight:** Este resultado sugere que, para o dataset fornecido, o modelo de ML não conseguiu identificar uma correlação estatisticamente significativa entre a ocorrência de promoções e a variação da quantidade de estoque. Esta é uma conclusão importante sobre a **natureza dos dados**, e não um erro do modelo.

### 4. Previsão e Obtenção de Insights

A última fase envolveu o uso do modelo treinado para simular cenários futuros (Análise Hipotética) por meio da funcionalidade **Single Prediction**.

#### Análise Visual das Previsões

As simulações indicaram uma projeção de estoque para um horizonte de curto prazo (Ex: 2024-01-19 a 2024-01-20), com uma forte divergência entre os cenários em percentis:

* **Cenário Otimista (P90):** Representado pela linha ascendente (amarela/dourada), atingindo um Valor Previsto de **0.317**. Este cenário indica a quantidade **máxima** de estoque sugerida para atender a uma demanda alta.
* **Cenário Pessimista (P10):** Representado pela linha constante próxima a zero (cerca de **-0.029**). Este cenário sugere que a demanda pode ser quase nula, o que é crucial para evitar excesso de estoque.

#### Conclusões e Insights de Negócio

A análise das previsões em percentis (P10 vs P90) permite ao gestor tomar decisões estratégicas:

1.  **Gestão de Risco (P10):** O cenário pessimista (P10) atua como um alerta sobre produtos com demanda historicamente baixa, ajudando a evitar custos com **estoque parado**.
2.  **Otimização de Oportunidades (P90):** O cenário otimista (P90) aponta o potencial máximo de estoque a ser mantido, garantindo que o negócio não perca vendas em períodos de alta demanda.
3.  **Necessidade de Enriquecimento de Dados:** A baixa previsibilidade do modelo (exposta pela grande diferença entre P10 e P90, e pelo impacto zero da promoção) indica a necessidade de **enriquecer o dataset** com mais variáveis preditivas (como preço, sazonalidade real ou categoria do produto) para obter previsões mais estáveis e menos divergentes.
