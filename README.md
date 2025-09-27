# MVP_House_Price_Prediction

Previsão de preço de imóveis usando dados tabulares do conjunto [USA Housing](https://www.kaggle.com/datasets/vedavyasv/usa-housing)
. O objetivo é estimar o preço de venda de uma casa a partir de variáveis agregadas da área como renda média, idade média das casas, número médio de cômodos e quartos e população da área.

---

## Visão geral

* Tipo de problema: regressão supervisionada
* Alvo: `Price`
* Principais resultados no teste:

  * Baseline mediana: RMSE 351.107, MAE 279.276, R2 0,001
  * Regressão Linear: RMSE 100.444, MAE 80.879, R2 0,918
  * Random Forest com tuning: RMSE 122.073, R2 0,879
* Melhor modelo: **Regressão Linear** por desempenho, simplicidade e interpretabilidade

---

## Dataset

* Fonte conceitual: USA Housing dataset
* Colunas utilizadas

  * `Avg. Area Income`
  * `Avg. Area House Age`
  * `Avg. Area Number of Rooms`
  * `Avg. Area Number of Bedrooms`
  * `Area Population`
  * `Price`
  * `Address` foi mantida apenas como identificador e não entra como preditora
* Tamanho: 5.000 linhas e 7 colunas
* Valores ausentes: não há

Observação: o notebook carrega os dados via URL pública.

---

## Estrutura do projeto

* `notebook.ipynb`
  Notebook Colab com todo o fluxo do MVP

  * Escopo, objetivo e definição do problema
  * Reprodutibilidade e ambiente
  * EDA resumida
  * Target, variáveis e divisão dos dados
  * Pipeline de pré-processamento
  * Baseline e modelos candidatos
  * Validação e tuning de hiperparâmetros
  * Avaliação final, erros e limitações
  * Engenharia de atributos
  * Boas práticas e rastreabilidade
  * Conclusões e próximos passos

---

## Metodologia

* Divisão dos dados

  * Hold out 80 por cento treino e 20 por cento teste
  * Validação cruzada `KFold` no tuning do Random Forest
* Pipeline de pré-processamento

  * `SimpleImputer(strategy="median")`
  * `StandardScaler`
  * Encapsulados em `ColumnTransformer` e `Pipeline` para evitar vazamento
* Modelos comparados

  * Baseline `DummyRegressor`
  * `LinearRegression`
  * `Ridge`
  * `RandomForestRegressor`
  * `GradientBoostingRegressor`
* Tuning

  * `RandomizedSearchCV` no Random Forest com busca leve e `KFold`

---

## EDA e achados

* `Price` com um pico principal e simetria razoável
* Correlação mais alta com `Avg. Area Income`
* Relações adicionais com `Avg. Area House Age` e `Area Population`
* Baixa multicolinearidade entre as preditoras
* Gráficos incluídos no notebook

  * Distribuição do alvo
  * Matriz de correlação
  * Real vs predito
  * Resíduos e MAE por decil

---

## Resultados

* Baseline

  * RMSE 351.107, MAE 279.276, R2 0,001
* Modelos

  * LinearRegression

    * RMSE 100.444, MAE 80.879, R2 0,918
  * Ridge

    * Desempenho equivalente ao Linear
  * GradientBoosting

    * RMSE 109.000, R2 0,903
  * RandomForest com tuning

    * RMSE 122.073, R2 0,879

**Conclusão**

* Modelo final: **Regressão Linear**
* Motivos: melhor desempenho, baixo custo e interpretabilidade

---

## Engenharia de atributos

* Remoção de `Address` como identificador
* Imputação mediana e padronização das variáveis numéricas
* Testes adicionais registrados no notebook

  * Log do alvo, interações e polinômios de grau dois
  * Ganhos marginais, mantidos de fora na versão final

---

## Limitações e próximos passos

* Variáveis agregadas por área, sem geolocalização fina nem atributos intrínsecos do imóvel
* Erros absolutos maiores nas caudas do preço
* Próximos passos

  * Incluir CEP e coordenadas e derivar distâncias a pontos de interesse
  * Adicionar características do imóvel como acabamento e terreno
  * Testar regularização mais fina e boosting com parada antecipada
  * Considerar transformação log do alvo e regressão por quantis
  * Ampliar a validação e checar estabilidade por região e ao longo do tempo

---

## Reprodutibilidade

* Dados carregados por URL pública
* Seeds fixas
* Versões do ambiente registradas no início
* Pipeline final pode ser salvo com `joblib` para reuso
