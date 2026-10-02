# Previsão de Churn — IBM Telco Customer Churn

**Status: em desenvolvimento.**

Neste projeto, desenvolvi modelos de Machine Learning para identificar clientes com risco de cancelar os serviços de uma empresa de telefonia. Trabalhei desde a análise exploratória e preparação dos dados até a comparação de algoritmos e o ajuste de hiperparâmetros.

O objetivo foi conectar a previsão de churn a uma decisão de negócio: **quais clientes priorizar em ações de retenção?**

[Ver notebook](projeto_churn_IBM.ipynb) · [Meu portfólio](https://lucasffreitasds.github.io/portfolio_projetos/)

## Dados

Utilizei o **IBM Telco Customer Churn**, uma base de exemplo de uma empresa fictícia de telecomunicações, com **7.043 clientes e 33 colunas**. Os dados incluem perfil dos clientes, serviços contratados, tempo de relacionamento, tipo de contrato e informações de cobrança.

A variável resposta é `Churn Value`: **1 para cancelamento e 0 para permanência**. Na base, 26,54% dos clientes cancelaram os serviços. Esse desbalanceamento foi importante na avaliação, porque uma acurácia alta, sozinha, não indica que o modelo identifica bem os cancelamentos.

## Desenvolvimento

Organizei o trabalho nas seguintes etapas:

1. Inspeção dos dados e tratamento de tipos e valores ausentes.
2. Formulação de hipóteses e análise exploratória, com correlação de Pearson e V de Cramér.
3. Codificação de variáveis categóricas, transformação de `Total Charges` e padronização dos atributos numéricos.
4. Divisão estratificada em **80% para treino e 20% para teste**.
5. Seleção de **14 atributos** com base na importância das variáveis de um Random Forest.
6. Comparação dos modelos com validação cruzada e ajuste de hiperparâmetros com **Optuna**.

Testei **KNN, Decision Tree, Random Forest, XGBoost e Logistic Regression**. Na preparação, retirei dos preditores campos como `Churn Label`, `Churn Score` e `Churn Reason`, além de identificadores e outras variáveis descartadas da modelagem.

## Resultados

Aprofundei os testes com **Random Forest**, usando duas buscas de hiperparâmetros: uma para maximizar F1 e outra para maximizar recall. A ideia foi comparar o equilíbrio entre precisão e identificação dos cancelamentos com uma abordagem que prioriza encontrar mais clientes da classe de churn.

As duas versões foram avaliadas no mesmo conjunto de teste, com **1.409 clientes**, sendo **374 com churn**. As métricas abaixo foram calculadas a partir das matrizes de confusão registradas no notebook.

| Métrica | Otimização por F1 | Otimização por recall |
| --- | ---: | ---: |
| Acurácia | 77,43% | 74,17% |
| Precision — churn | 55,04% | 50,79% |
| Recall — churn | 81,82% | **85,83%** |
| F1 — churn | **0,6581** | 0,6382 |
| Cancelamentos identificados | 306 de 374 | **321 de 374** |

A versão otimizada por recall identificou **15 cancelamentos a mais**, mas também gerou **61 falsos positivos adicionais**. Para uma campanha de retenção, essa escolha depende do custo de abordar um cliente, do custo de perder esse cliente e da capacidade da equipe.

Esses resultados mostram o desempenho na base de teste. O efeito de uma campanha sobre a retenção precisaria ser avaliado na operação.

## Ferramentas utilizadas

Python, Pandas, NumPy, SciPy, Matplotlib, Seaborn, Scikit-learn, XGBoost, Optuna, OpenPyXL e Jupyter Notebook.

## Como executar

Utilizei **Python 3.13**. Com Python e Git instalados, clone o repositório:

```bash
git clone https://github.com/lucasffreitasds/churn_IBM.git
cd churn_IBM
```

Se usar Pyenv, ajuste `.python-version` para uma versão ou ambiente instalado na sua máquina; o arquivo atual referencia meu ambiente local, `virtualenv_p_churn`.

Crie e ative um ambiente virtual:

**Linux / macOS:**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

**Windows — Prompt de Comando:**

```bat
py -3.13 -m venv .venv
.venv\Scripts\activate.bat
```

Instale as dependências e abra o JupyterLab:

```bash
python -m pip install -r requirements.txt
python -m pip install xgboost optuna jupyterlab
python -m jupyterlab
```

Abra `projeto_churn_IBM.ipynb`. Na seção de carregamento dos dados, substitua o caminho absoluto pela leitura relativa:

```python
df = pd.read_excel("dataset/Telco_customer_churn.xlsx")
```

Execute o notebook a partir da raiz do repositório, usando o kernel do ambiente criado e seguindo a ordem das células.

## Limitações e próximos passos

O projeto está em desenvolvimento. Já concluí uma primeira rodada de análise e modelagem, mas ainda preciso revisar alguns pontos e melhorar a estrutura dos experimentos:

- **Revisar a comparação dos modelos:** corrigir a atribuição dos resultados da regressão logística, que atualmente recebe os resultados do XGBoost, e alinhar os hiperparâmetros encontrados nas buscas às configurações usadas na comparação.

- **Melhorar a validação cruzada:** colocar a preparação dos dados e a seleção de variáveis em uma `Pipeline`. Atualmente, essas etapas são ajustadas antes da validação cruzada, o que pode deixar suas métricas otimistas.

- **Separar a avaliação final:** as duas versões de Random Forest foram avaliadas no mesmo conjunto de teste. Para os próximos experimentos, preciso separar a escolha do modelo da avaliação final em dados ainda não utilizados nessa decisão.

- **Facilitar a execução:** substituir o caminho local do dataset e completar o `requirements.txt`, incluindo as dependências e suas versões.

- **Aproximar a avaliação do problema de negócio:** testar diferentes limiares de classificação e considerar os custos de abordagem e perda de clientes. Isso permitirá avaliar qual equilíbrio entre recall e precision faz mais sentido para uma campanha de retenção.

## Autor

**Lucas Ferreira de Freitas** — Cientista de Dados e Engenheiro Agrônomo.

[GitHub](https://github.com/lucasffreitasds) · [Portfólio](https://lucasffreitasds.github.io/portfolio_projetos/)

**Fonte dos dados:** [IBM — Telco Customer Churn](https://community.ibm.com/community/user/blogs/steven-macko/2018/11/26/new-base-samples-for-ibm-cognos-analytics-1111).
