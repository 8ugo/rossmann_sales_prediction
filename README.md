#  Sales Prediction - Rossmann Drugstore
 ![](img/rossmann_view.png)

 A Dirk Rossmann GmbH, comumente referida como Rossmann, é uma das maiores redes de drogarias da Europa, com cerca de 56.200 funcionários e mais de 4.000 lojas.

 A rede de farmacias disponibilizou seus dados atráves do site [Kaggle](https://www.kaggle.com/competitions/rossmann-store-sales/overview). Foram disponibilizados 1.017.209 registros das vendas realizadas pelas filias da empresa, contendo 18 características únicas para cada venda realizada.

 # 1. Problema de Negócio
O objetivo deste projeto é desenvolver um sistema automatizado para prever as receitas das lojas da rede de farmácias, a fim de auxiliar o CFO na decisão de reformar as lojas. Atualmente, as previsões são feitas manualmente pelos gerentes de cada loja, levando em conta fatores como promoções, competição por clientes, feriados e sazonalidade. No entanto, os resultados dessas previsões variam muito devido à abordagem manual e às particularidades de cada loja.

Para resolver esse problema, propõe-se a criação de um sistema que utilize técnicas de análise de dados e aprendizado de máquina para processar informações históricas das lojas, como vendas passadas, promoções realizadas e dados sobre concorrentes. Com base nesses dados, o sistema gerará previsões automatizadas de receita para as próximas 6 semanas de cada loja.

Para facilitar o acesso e consulta dessas previsões, será desenvolvido um Bot no aplicativo Telegram. O CFO poderá utilizar esse Bot para consultar as previsões de receita de forma conveniente e intuitiva, permitindo uma análise mais eficiente e embasada na tomada de decisão em relação ao investimento na reforma de cada loja.

Com a implementação desse sistema automatizado, espera-se que o CFO tenha acesso a previsões mais precisas e confiáveis, o que auxiliará na definição dos recursos a serem investidos em cada loja. Dessa forma, será possível melhorar a estrutura das lojas, oferecer um melhor atendimento ao público e impulsionar o sucesso da rede de farmácias.

 # 2. Premissas de Negócio
 Para a construção da solução, foram consideradas as seguintes premissas:
 * A consulta da previsão de vendas estará disponível 24/7, e será acessível via aplicativo do Telegram, onde o CFO digitará o código da loja, e como resposta, receberá o valor da previsão para as próximas 6 semanas.
 * Foram consideradas para a previsão apenas as lojas que possuiam o valor de vendas superior a 0 na base de dados.
 * Os dias em que as lojas estavam fechadas foram descartadas na realização da previsão.
 * Lojas que não possuíam dados de competidores próximos tiveram o valor da distância fixada em 200.000 metros.

 ## 2.1. Descrição dos Dados
 | Atributo                          | Descrição                                                                                                                                             |
 | :-------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------- |
 | Store                             | Identificador único de cada loja                                                                                                                      |
 | Date                              | Data em que ocorreu o evento de venda                                                                                                                 |
 | DayOfWeek                         | Variável numérica que representa o dia da semana                                                                                                      |
 | Sales                             | Valor de vendas do dia                                                                                                                                |
 | Customers                         | Quantidade de clientes na loja no dia                                                                                                                 |
 | Open                              | Indicador para loja aberta = 1 ou fechada = 0                                                                                                         |
 | StateHoliday                      | Indica se o dia é feriado de estado. a = Feriado público, b = Feriado de páscoa, c = Natal, 0 = Não há feriado                                        |
 | SchoolHoliday                     | Indica se a loja foi ou não fechada durante o feriado escolar                                                                                         |
 | StoreType                         | Indica o modelo de lojas. Pode variar entre a, b, c, d                                                                                                |
 | Assortment                        | Indica o nível de variedade de produtos: a = básico, b = extra, c = estendido                                                                         |
 | CompetitionDistance               | Distância (em metros) para o competidor mais próximo                                                                                                  |
 | CompetitionOpenSince [Month/Year] | Indica o ano e mês em que o competidor mais próximo abriu                                                                                             |
 | Promo                             | Indica se a loja está com alguma promoção ativa no dia                                                                                                |
 | Promo2                            | Indica se a loja deu continuidade na promoção: 0 = loja não está participando, 1 = loja participando                                                  |
 | Promo2Since [Year/Week]           | Descreve o ano e semana de quando a loja começa a a promoção extendida                                                                                |
 | PromoInterval                     | Descreve os meses em que a loja iniciou a promo2, ex.: "Feb,May,Aug,Nov" significa que a loja iniciou as promoções estendidas em cada um desses meses |

 # 3. Estratégia da Solução
 Para fazer a entrega da primeira solução de maneira o mais rápido possível, entregando valor para a empresa e possibilitando que o CFO tome decisões com mais agilidade, foi utilizado o método CRISP-DS

 ![](img/crisp_ds.png)

**Passos do CRISP-DS**:

O método CRISP-DS consiste em nove passos cíclicos para desenvolver uma solução de forma iterativa e com entregas rápidas e de qualidade. Os passos são:

**1. Problema de Negócio:** Nesta etapa, o objetivo é receber e compreender o problema de negócio que será abordado. É fundamental entender as perguntas e necessidades do cliente ou dono do problema.

**2. Entendimento de Negócio:** Nesta etapa, busca-se obter uma visão mais aprofundada do problema e das necessidades do cliente. Protótipos e validações podem ser utilizados para assegurar o alinhamento com o cliente.

**3. Coleta de Dados:** Aqui, os dados relevantes para a solução são coletados de diversas fontes, como bancos de dados internos ou externos.

**4. Limpeza dos Dados:** Nesta etapa, os dados são preparados e tratados para garantir sua qualidade e consistência. Dados ausentes, inconsistentes ou incorretos são tratados adequadamente.

**5. Exploração dos Dados:** O objetivo é analisar os dados coletados e identificar padrões, tendências e insights relevantes. Visualizações e análises estatísticas são aplicadas para entender melhor os dados.

**6. Modelagem dos Dados:** Nesta etapa, os dados são transformados e preparados para serem utilizados por algoritmos de Machine Learning. Variáveis de entrada e saída são definidas e modelos são treinados e validados.

**7. Aplicação de Algoritmos de Machine Learning:** Aqui, os modelos treinados são aplicados aos dados para gerar resultados e previsões. O desempenho dos modelos é monitorado e ajustes podem ser realizados.

**8. Avaliação de Performance:** Nesta etapa, a performance dos modelos é avaliada em relação aos objetivos de negócio estabelecidos. Os resultados são comparados com métricas de referência ou outras abordagens existentes.

**9. Publicação da Solução:** Por fim, a solução é documentada, apresentada e disponibilizada para uso. Os resultados são comunicados ao cliente ou stakeholders relevantes.

Esse ciclo se repete, permitindo melhorias contínuas com base no feedback e na validação dos resultados.

 ## 3.1. Produto Final
 Foi combinado com o CFO que seria entregue um Bot dentro do aplicativo Telegram, facilitando assim que o CFO verifique a previsão das lojas independente do local em que ele esteja.

 Além disso, no processo de criação do produto final, será criado uma API que será utilizada para retornar as previsões das lojas. Essa API irá utilizar o modelo de Machine Learning desenvolvido para realizar a previsão.

 ## 3.2. Ferramentas Utilizadas
 Para criar a solução, foram utilizadas as seguintes ferramentas:
 - Linguagem de Programação Python versão 3.8.13
 - Versionador de código Git
 - Aplicação Jupyter Notebook para prototipar a solução
 - Serviço de Hospedagem em Nuvem Heroku
 - Técnicas de manipulação de dados utilizando a linguagem de programação Python
 - Técnicas de redução de dimensionalidade e seleção de atributos
 - Algoritmos de Machine Learning da biblioteca [scikit-learn](https://scikit-learn.org/stable/) da linguagem de programação Python

 # 4. Top 3 Insights
 Foi criado um Mindmap para ajudar na construção de Hipóteses, a fim de facilitar e delimitar o escopo da etapa de análise exploratória.

 ![](/img/mind_map_hypothesis.png)

 Através desse Mindmap, foram geradas 19 hipóteses de negócio, das quais 12 foram escolhidas para serem validadas, confirmando assim  premissas da equipe de negócios e do dono do problema, além da geração de insights para ambos.

 Dessas 12 hipóteses verificadas, os 3 principais insights gerados foram:

 ## Insight 1: Lojas Abertas durante o feriado de Natal deveriam vender mais
 **Hipótese Falsa:** Lojas que ficam abertas durante o feriado de natal não vendem mais em comparação a outros feriados

 ![](img/hip_1.png)


 ## Insight 2: Lojas Deveriam vender mais depois do dia 10 de cada mês
 **Hipótese Verdadeira:** Lojas realmente vendem mais depois do dia 10 de cada mês

 ![](img/hip_2.png)

 ## Insight 3: Lojas deveriam vender mais no segundo semestre do ano
 **Hipótese Falsa:** Lojas vendem MENOS no segundo semestre do ano

 ![](img/hip_3.png)

 # 5. Modelos de Machine Learning
 Para o primeiro ciclo do projeto foram selecionados 5 algoritmos para teste, a fim de escolher o algoritmo que tivesse a melhor perfomance e o melhor custo de implementação. Foi optado pela simplicidade nessa etapa inicial, visto que era o primeiro ciclo do projeto e o objetivo principal era entregar uma solução que fosse mínimamente utilizável para a equipe de negócios e pelo CFO.

 Os algotitmos selecionados foram:
 - Avarege Model
 - Linear Regression
 - Linear Regression - Lasso
 - Random Forest Regressor
 - XGBRegressor

 Após a escolha dos algoritmos, foram realizados treinamentos e testes com cada um deles, a fim de verificar qual deles teria a melhor perfomance.

 Além disso, foi utilizado o método de seleção de *features* [Boruta](https://mbq.github.io/Boruta/) para auxiliar na escolha das *features* mais importantes e impactantes da base de dados.

 # 6. Seleção do Modelo de Machine Learning
 ## 6.1. Esolha da Métrica
 Foi utilizado a métrica ***MAPE (Mean Absolute Percentage Error)*** como parâmetro de escolha entre os algoritmos, porque esta métrica é mais fácil de ser consumida pela equipe de negócio e pelo CEO, visto que ela representa a porcentagem do erro em relação ao valor médio.

 ## 6.2. Métricas dos Algoritmos
 Após os testes inicias, obtivemos os seguintes resultados:


 | Nome do Modelo            |         MAE |     MAPE |        RMSE |
 | :------------------------ | ----------: | -------: | ----------: |
 | Avarege Model             | 1354.800353 | 0.455051 | 1835.135542 |
 | Linear Regression         | 1867.089774 | 0.292694 | 2671.049215 |
 | Linear Regression - Lasso | 1891.704880 | 0.289106 | 2744.451735 |
 | Random Forest Regressor   |  679.893818 | 0.099967 | 1011.038517 |
 | XGBRegressor              |  865.126662 | 0.124905 | 1278.187883 |

 ## 6.3. Métricas dos Algoritmos - *Cross Validation*
 Após os testes com os algoritmos selecionados, foi utilizado a técnica de ***Cross Validation*** para validar os resultados e garantir a performance real de cada uma dos modelo utilizados. Como o problema se tratava de um série temporal, foi utilizada a técnica de ***Cross Validation*** específica para esse problema, respeitando assim a linha do tempo no treinamento dos algoritmos.

 ![](img/cross_validation.png)

 Com esse método de validação, foram obtidas as seguintes performances:

 | Nome do Modelo                |             MAE CV |       MAPE CV |            RMSE CV |
 | :---------------------------- | -----------------: | ------------: | -----------------: |
 | Linear Regression             | 2081.73 +/- 295.63 |  0.3 +/- 0.02 | 2952.52 +/- 468.37 |
 | Linear Regression Regularized |  2116.38 +/- 341.5 | 0.29 +/- 0.01 | 3057.75 +/- 504.26 |
 | Random Forest Regressor       |  837.66 +/- 218.22 | 0.12 +/- 0.02 | 1255.81 +/- 318.76 |
 | XGBoost Regressor             | 1045.83 +/- 182.93 | 0.14 +/- 0.02 |  1509.2 +/- 260.07 |

 ## 6.4. Escolha do Modelo
 Embora o algoritmo ***Random Fores Regressor*** tenha sido o algoritmo que melhor performou, foi optado pelo algoritmo ***XGBosst Regressor*** nesta etapa. 
 - Primeiro, porque o erro entre esses dois algoritmos é pequeno.
 - segundo porque o tempo de treinamento do ***XGBoost Regressor*** é mais rápido se comparado ao algoritmo ***Random Fores Regressor***. 
 - Terceiro porque o modelo final treinado pelo algoritmo ***XGBoost Regressor*** ocupa menos espaço que o algoritmo ***Random Fores Regressor***, deixando assim o uso de servidores em nuvem mais baratos.


 ## 6.5. Ajuste de Hiperparâmetros
 Foi o utilizado a técnica de ***Random Search*** para fazer a busca dos melhores hyperparâmetros. Os testes realizados foram os seguintes:

 | Tentativa |             MAE CV |       MAPE CV |            RMSE CV |
 | :-------: | -----------------: | ------------: | -----------------: |
 |  Teste 1  | 1298.59 +/- 145.92 | 0.18 +/- 0.01 | 1876.77 +/- 183.35 |
 |  Teste 2  |  805.51 +/- 136.41 | 0.11 +/- 0.01 | 1178.01 +/- 207.34 |
 |  Teste 3  |   1047.9 +/- 138.9 | 0.15 +/- 0.01 | 1509.67 +/- 188.21 |
 |  Teste 4  |  806.21 +/- 129.75 | 0.11 +/- 0.01 | 1173.31 +/- 200.39 |
 |  Teste 5  | 1028.34 +/- 126.16 | 0.14 +/- 0.01 | 1472.95 +/- 162.73 |
 |  Teste 6  | 1521.97 +/- 165.69 | 0.21 +/- 0.01 | 2204.28 +/- 210.19 |
 |  Teste 7  |  1307.5 +/- 132.12 | 0.18 +/- 0.01 | 1896.64 +/- 182.21 |
 |  Teste 8  |  889.96 +/- 123.96 | 0.12 +/- 0.01 | 1278.13 +/- 179.34 |
 |  Teste 9  |  797.98 +/- 155.47 | 0.11 +/- 0.01 |  1157.82 +/- 228.1 |
 | Teste 10  |  1081.47 +/- 115.4 | 0.15 +/- 0.01 | 1546.74 +/- 159.11 |

 Sendo que os parâmetros do **Teste 4** foram os selecionados como os melhores parâmetros para o treinamento do modelo.

 ## 6.6. Performance do Modelo
 Como podemos observar nos dois primeiro gráficos abaixo, o modelo selecionado obteve uma performance aceitável, visto que conseguiu entender e reproduzir o padrão de vendas ao longo dos anos estudados.

 Já nos dois últimos gráficos, podemos observar uma distribuição normal a cerca das previsões, o que nos inidica um bom resultado do modelo selecionado, além de podermos verificar a distribuição dos erros do modelo.

 ![](img/performance.png)

 # 7. Resultado de Negócio
 Com o modelo selecionado e treinado, obtivemos a seguinte performance de negócio para as 5 melhores lojas:

 | ID da Loja |     Previsões |  Pior Cenário | Melhor Cenário |       MAE |   MAPE |
 | :--------- | ------------: | ------------: | -------------: | --------: | -----: |
 | 259        | \$ 542,261.00 | \$ 541,643.54 |  \$ 542,878.46 | \$ 617.46 | 0.0483 |
 | 990        | \$ 235,996.80 | \$ 235,689.77 |  \$ 236,303.82 | \$ 307.03 | 0.0485 |
 | 1089       | \$ 378,721.16 | \$ 378,205.41 |  \$ 379,236.90 | \$ 515.75 | 0.0488 |
 | 1097       | \$ 440,957.44 | \$ 440,370.68 |  \$ 441,544.20 | \$ 586.76 | 0.0541 |
 | 667        | \$ 314,811.66 | \$ 314,324.37 |  \$ 315,298.95 | \$ 487.29 | 0.0551 |


 Um ponto importante de ressaltar, é que houveram algumas lojas que não obtiveram bons resultados, e que em uma próxima iteração deve ser tratadas individualmente para verificar qual pode ser o problema para essas lojas. As 5 piores lojas tiveram a seguinte performance:


 | ID da Loja |     Previsões |  Pior Cenário | Melhor Cenário |         MAE |  MAPE |
 | :--------- | ------------: | ------------: | -------------: | ----------: | ----: |
 | 292        | \$ 103,891.24 | \$ 100,561.08 |  \$ 107,221.41 | \$ 3,330.17 | 0.562 |
 | 909        | \$ 237,516.14 | \$ 229,953.85 |  \$ 245,078.43 | \$ 7,562.29 | 0.511 |
 | 876        | \$ 206,953.81 | \$ 202,905.53 |  \$ 211,002.09 | \$ 4,048.28 | 0.316 |
 | 722        | \$ 353,882.56 | \$ 351,866.71 |  \$ 355,898.42 | \$ 2,015.85 | 0.268 |
 | 274        | \$ 195,549.67 | \$ 194,157.14 |  \$ 196,942.21 | \$ 1,392.54 | 0.244 |


 Como resultado final, temos os seguintes cenários:

 | Cenários       |           Valores |
 | :------------- | ----------------: |
 | Previsão Feita | \$ 285,901,376.00 |
 | Pior Cenário   | \$ 285,158,456.49 |
 | Melhor Cenário | \$ 286,644,307.44 |

 ## 7.1. Distribuição das Previsões por Loja
 ![](/img/business_performance.png)


 # 8. Conclusões
 Conforme pôde ser verificado, o projeto resolveu o problema inicial, que era a previsão de faturamento das lojas feitas de forma manual por seus gerentes.

 Outro ponto importante de destacar é que com a solução criada, o CFO pode agora consultar as lojas de forma automática utilizando o BOT criado, dando mais agilidade na tomada de decisão.

 # 9. Lições Aprendidas
 * Priorizar tarefas e soluções
 * Desenvolver soluções de forma cíclica, entregando assim resultado mais rapidamente
 * Construção de um BOT para o aplicativo de mensagens Telegram, afim de agilizar o acesso à informações.

 # 10. Próximos Passos
 * Investigar a razão de algumas lojas estarem com previsões ruins
 * Selecionar outros algoritmos para treinamento no próximo ciclo, a fim de buscar uma solução que melhore o desempenho da previsão
 * Criar uma apllicação Web utilizando o framework Streamlit para dar acesso Web às previsões para os gerentes das lojas.
 * Implementar testes unitários nas classes e funções produzidas.
 * Criar novas *Features* para tentar melhorar a perfomance do modelo atual e de modelos futuros.
 * Aplicar técnicas de programação para melhorar o desempenho da solução criada.
