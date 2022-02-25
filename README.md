

# Detectando e evitando churn de clientes em um banco

O TopBank é uma empresa fictícia que oferece uma variedade de serviços financeiros, com o produto principal sendo uma conta bancária, onde serviços como investimentos e seguros são oferecidos. Quando um cliente está ativo, a empresa consegue oferecer os serviços e obter receita recorrente. No entanto, a empresa identificou um problema em que alguns clientes não estão mais contratando serviços, resultando em perda de receita.

A empresa que minimizar ou evitar esse fenômeno e gostaria que as seguintes perguntas fossem respondidas:

- **Quais são as principais características dos clientes em churn?**
- **Quem são os prováveis clientes a entrarem em churn?**
- **Como evitar que um cliente entre em churn?**

## Features

Para responder as perguntas, a empresa disponibilizou um conjunto de dados com cerca de 10.000 clientes diferentes entre França, Espanha e Alemanha. As features originais são:

| Feature  | Descrição |
| ------------- | ------------- |
| CustomerID  | Identificador único de cliente |
| Surname  | Sobrenome do cliente  |
| CreditScore  | Nota de crédito do cliente |
| Geography | País do cliente|
| Gender | Gênero do cliente |
| Age | Idade do cliente |
| Tenure | Idade da conta bancária |
| Balance | Saldo |
| NumOfProducts | Número de produtos ativos |
| HasCrCard | Se o cliente possui cartão |
| IsActiveMember | Se o cliente é ativo |
| EstimatedSalary | Estimativa do salário anual |
| Exited | Se o cliente entrou em churn |


## Solução

O problema foi identificado como um problema clássico de classificação binária e pode ser solucionado ao usar algoritmos de aprendizado supervisionado de máquina. A solução foi desenvolvida em ciclos, possibilitando melhorias incrementais nos resultados ao longo do tempo. A solução completa junto com o código criado [pode ser vista neste arquivo](https://github.com/pedropscf/2-churn-prediction/blob/bd1346f98c4d54d71cf4fdefa8c829d0afabe58b/m03_churn_prediction.ipynb), enquanto um resumo da solução foi dividido nos seguintes passos:

Neste ponto, foi realizada uma divisão 80/20 dos dados de forma estratificada (mantendo o churn rate de 20% em ambos conjuntos), onde 80% dos dados foram utilizados para treino e teste do modelo, enquanto os outros **20% foram separados especialmente para a avaliação da performance do modelo no problema de negócio.**

### Limpeza e manipulação dos dados

Inicialmente, foi realizada uma investigação no conjunto de dados em busca de valores nulos, outliers e/ou valores estranhos nas diferentes features, com o objetivo de realizar a limpeza dos dados. No entanto, foi constatado que os dados estavam limpos e prontos para o uso.

### Análise exploratória de dados (EDA)

Então, foi iniciada uma análise em busca de uma resposta para a primeira pergunta: **Quais são as principais características dos clientes em churn?** Apesar da análise não trazer uma resposta imediata à pergunta, ela nos trás alguns pontos de atenção, como:

- **Clientes na Alemanha têm maior taxa de churn que a média;**
- **Clientes com saldo 0 na conta apresentam maior taxa de churn que a média;**
- **Clientes mais jovens apresentam taxa maior de churn que a média;**
- **Clientes com mais de 2 produtos apresentam uma taxa de churn maior que a média.**

Estes pontos abrem a discussão do porquê este fenômeno está ocorrendo. Com uma investigação mais profunda o time pode agir de modo a resolver este problema, que pode estar sendo causado por: um novo competidor focado na Alemanha e/ou ou no público mais jovem, problemas de experiência do cliente para clientes com muitos produtos, ou ainda, uma melhor oferta para outra conta, resultando em saldo 0.

### Feature Engineering

Não foram criadas novas features, apenas as features já existentes foram transformadas, onde as numéricas com o MinMaxScaler e features categóricas foram transformadas com o método OneHotEncoding. Foi utilizado o algoritmo do Boruta para avaliar a importância de cada feature disponível e nenhuma feature foi removida.

### Criação do modelo

Nesta etapa, diferentes modelos foram criados tanto com os dados balanceados, quanto para os dados desbalanciados. Os modelos avaliados foram: regressão logística, random forest e XGBoost, e o rebalanciador de dados utilizado foi o SMOTE.

Após o tuning, **o modelo de Random Forest treinado apresentou níveis superiores a 90% para as métricas de acurácia, precisão recall, F1 e AUC.** O modelo e todos os scalers foram salvos no formato Pickle para uso futuro.

### Performance do modelo

A curva lift mostra que para os primeiros 20% dos dados, a probabilidade de classificar corretamente se um cliente entrou ou não em churn é 3 vezes maior que ao acaso. A curva ROC, indica ainda que com 20% dos dados, é possível classificar aproximadamente 60% dos clientes que entrarão em churn. Além disso, foi feita uma análise com o limite de probabilidade em que um cliente é considerado como churn e foi observado que para cliente com probabilidade maior que 64% serão considerados como clientes em risco. Com estes resultados a segunda pergunta é respondida **Quem são os prováveis clientes a entrarem em churn?** Uma lista contendo todos os clientes e suas respectivas probabilidade de churn foi criada e enviada à empresa.

### Otimização (performance de negócios)

A empresa informou que possui uma estimativa de receita anual gerada por cliente a partir do salário anual estimado

- Clientes com salário acima da média trazem uma receita anual de 5% do seu salário;
- Enquanto clientes na média ou abaixo da média trazem uma receita de 3% do salário estimado.

Com essa informação, a solução para a terceira pergunta (**Como evitar que um cliente entre em churn?**) pode ser respondida com um programa de cupons/presentes para clientes em risco de churn. Estes cupons poderiam ser utilizados para adiquirir serviços da empresa ou em parcerias com serviços de streaming. O time de marketing forneceu um **aporte inicial de $20.000** para o programa.

Baseado na receita estimada, foram criadas diferentes categorias de cupons, indo de $20,00 para clientes com receita menor que $400,00 até $250,00 para clientes com receita superior a $5000,00.

Com isso, foraam selecionados os clientes com maior probabilidade de churn e com o aporte, foram obtidos os seguintes resultados:

- **O conjunto com 2.000 clientes, continha 407 churners, $9,17M de receita anual estimada e $1.78M de receita perdida com os churners;**
- **Clientes com probabilidade superior a 64$, representam 312 clientes e possuiam 209 churners e mais de 80% da receita perdida;**
- **Com o aporte, 129 clientes receberam o cupom, onde 109 destes teriam entrado em churn, o que representa uma recuperação de $460.000 de receita perdida com um investimento de $20.000;**
- **Dobrando o aporte para $40.000 é possível aumentar o impacto para 250 clientes, onde 179 destes teriam saído da empresa, recuperando $800,000 em receita a partir de um investimento de $40.000 (2000% ROIC).**


## Ferramentas utilizadas

**Data manipulation and cleaning:** pandas, numpy

**Data visualization:** matplotlib, seaborn

**Machine learning:** Classificação (scikit-learn and xgboost), rebalanceamento de dados(imblearn), métricas de classificação (yellowbrick)



## Sobre mim
Entusiasta de ciência de dados, aprendendo sobre aplicações de Machine Learning, IA e Ciência de dados em geral para a solução de diversos problemas de negócios.


## Autor

- Pedro Fernandes [@pedropscf](https://www.github.com/pedropscf)
