# IA Botafogo

Projeto de ciência de dados voltado à previsão de variáveis esportivas e financeiras relacionadas aos jogos do Botafogo como mandante. O notebook principal constrói features a partir de dados históricos, aplica tratamentos estatísticos e treina modelos para três alvos: percentual de ocupação do estádio, lucro/prejuízo do evento e resultado da partida. [file:27]

## Objetivo

O projeto busca responder, antes de cada jogo em casa, três perguntas práticas:

- Qual deve ser o percentual de ocupação do estádio. [file:27]
- O jogo tende a gerar lucro ou prejuízo. [file:27]
- Qual tende a ser o resultado esportivo da partida. [file:27]

Essa separação em três problemas diferentes aparece explicitamente no notebook, que cria datasets específicos para cada variável-alvo para evitar usar informações indisponíveis antes do evento. [file:27]

## Escopo analítico

O fluxo usa dados de jogos do Botafogo, com foco nos jogos como mandante, e combina informações de contexto da partida, calendário, adversário, mando, desempenho recente e indicadores operacionais. [file:27] Entre as variáveis derivadas no notebook estão dia da semana, hora e minuto do jogo, mês, dia do mês, sequência recente de resultados, resultado do último jogo, gols do Botafogo nos últimos jogos, percentual de ocupação, flag de mando vendido/compartilhado, flag de lucro/prejuízo e distância em dias desde o último jogo em casa. [file:27]

Também há tratamento específico para adaptar a perspectiva do jogo ao Botafogo, ajustando resultado da partida, gols do time, colocação do Botafogo, colocação do adversário e identificação do oponente conforme o clube seja mandante ou visitante no registro bruto. [file:27]

## Dados de entrada

O notebook lê duas planilhas Excel como base principal do projeto:

- `botafogo_previsao_estadio.xlsx` [file:27]
- `botafogo_valores_previstos.xlsx` [file:27]

Ao longo do processamento, o notebook também gera um arquivo final tratado chamado `dados_modelo_final.csv`, que representa a base já transformada para modelagem. [file:27]

## Engenharia de atributos

A construção das features inclui transformações temporais, variáveis de performance recente e variáveis de contexto do evento. [file:27] Alguns exemplos identificados no notebook:

- `diasemana`, `hora`, `minuto`, `mes`, `diames`. [file:27]
- `resultadosconsecutivos` com janela de cinco jogos. [file:27]
- `resultadosultimojogo`. [file:27]
- `golsbotafogoultimosjogos`. [file:27]
- `percentualocupacao`. [file:27]
- `mandovendidocompartilhado`. [file:27]
- `prejuizolucro`. [file:27]
- `diasultimojogocasa`. [file:27]

O notebook ainda converte variáveis categóricas com `pd.get_dummies`, incluindo colunas como campeonato, estádio, tipo de partida, estado do adversário, time adversário e fase. [file:27]

## Tratamento dos dados

O pipeline aplica limpeza, conversão de tipos, tratamento de ausências e normalização. [file:27] Para um conjunto de colunas numéricas, o notebook usa teste de Shapiro para avaliar normalidade, substituição de outliers por IQR, transformação Box-Cox quando aplicável e normalização com `MinMaxScaler`. [file:27]

Além disso, variáveis booleanas como `anuncioupoupar`, `classico`, `eliminatorio` e `prejuizolucro` são padronizadas para formato lógico antes da etapa de modelagem. [file:27]

## Modelos

O notebook importa algoritmos de regressão e classificação de XGBoost e CatBoost, além de recursos de busca de hiperparâmetros com `GridSearchCV`. [file:27] Os três problemas modelados no projeto são:

1. Regressão para `percentualocupacao`. [file:27]
2. Classificação para `prejuizolucro`. [file:27]
3. Classificação para `resultadopartida`. [file:27]

Pelos resultados exibidos no notebook, há comparação de desempenho entre modelos e registro do algoritmo escolhido em colunas como `modelo1`, `modelo2` e `modelo3`, com presença de XGBoost e CatBoost nas previsões finais. [file:27]

## Métricas avaliadas

O projeto utiliza métricas de regressão e classificação importadas do `scikit-learn`, incluindo `mean_squared_error`, `mean_absolute_error`, `confusion_matrix`, `classification_report` e `accuracy_score`. [file:27] Na tabela de resultados exibida no notebook aparecem medidas como score do modelo, RMSE, MAE e acurácia para diferentes alvos. [file:27]

## Bibliotecas utilizadas

As importações do notebook indicam dependência de:

- `pandas` [file:27]
- `numpy` [file:27]
- `matplotlib` [file:27]
- `seaborn` [file:27]
- `scipy` [file:27]
- `patsy` [file:27]
- `scikit-learn` [file:27]
- `xgboost` [file:27]
- `catboost` [file:27]

Também são usados módulos padrão do Python como `datetime`, `math` e `warnings`. [file:27]

## Estrutura sugerida

A organização atual pode ser melhorada separando notebooks, dados, saídas e documentação. Uma estrutura recomendada para evolução do repositório é:

```text
ia-botafogo/
├─ README.md
├─ requirements.txt
├─ .gitignore
├─ data/
│  ├─ raw/
│  ├─ processed/
│  └─ predictions/
├─ notebooks/
│  ├─ Projeto-Futebol.ipynb
│  └─ Projeto-Futebol-Catboost.ipynb
├─ reports/
│  ├─ analysis/
│  └─ docs/
└─ src/
   ├─ data/
   ├─ features/
   ├─ models/
   ├─ evaluation/
   └─ utils/
```

## Como executar

1. Crie um ambiente virtual Python.
2. Instale as dependências de `requirements.txt`.
3. Garanta que os arquivos Excel usados como entrada estejam no caminho esperado pelo notebook. [file:27]
4. Abra `Projeto-Futebol.ipynb` em Jupyter Notebook ou JupyterLab.
5. Execute as células em ordem para reproduzir a preparação dos dados, os tratamentos e as previsões. [file:27]

Exemplo:

```bash
python -m venv .venv
source .venv/bin/activate  # Linux/Mac
pip install -r requirements.txt
jupyter lab
```

## Próximos passos

Algumas melhorias naturais para o projeto são:

- Extrair funções do notebook para módulos Python reutilizáveis.
- Versionar melhor os dados de entrada e saída.
- Criar pipeline reprodutível de treino e inferência.
- Registrar experimentos por alvo e por algoritmo.
- Adicionar validação temporal para evitar vazamento em séries de eventos esportivos.
- Incluir visualizações finais de importância de variáveis e erros de previsão.

## Observações

Este README foi gerado com base no notebook principal do projeto e descreve o que está explicitamente visível em sua lógica, importações, transformações e saídas. [file:27]
