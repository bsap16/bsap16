# Ficha Técnica

## Segmentação de dados de "O Mercado”

- **Objetivo:**
    - Realizar uma análise exploratória e descritiva, segmentando clientes com a RFM (Recência, Frequência e Valor Monetário) e oferecer insights de como utilizar esses dados para manter e aumentar a receita da empresa.
- **Equipe:**
    - Bruna Paiva
- **Ferramentas e Tecnologias:**
    - Spreadsheets, Google Slides, Looker Studio e Loom.
- **Processamento e análises:**
    - Marco 1
        - Importar dados para ferramentas /Limpar dados (Identificar duplicatas e nulos)
            - Inicialmente eu importei as tabelas para um única planilha denominada de "Segmentação", separando em abas com os nomes das planilhas de origem, sendo respectivamente "resumo_compras", "transacoes" e "clientes". Depois limpei os dados em outra aba dessa planilha denominado "dados_processados".
            - Utilizei a fórmula =QUERY(UNIQUE(IMPORTRANGE("link";"aba+colunas")); "select Col1, Col2, where Col1 is not null and Col2 is not null") para limpar os dados (eliminar repetições e eliminar dados nulos) e trazer as tabelas para as abas correspondentes dos dados de cada planilha.
            - Utilizei a fórmula =IMPORTRANGE para unir todas planilhas em uma só separando por abas que informam quantas duplicatas e nulos as tabelas originais tem antes de limpar os dados.
            - Utilizei essa fórmula =CONT.VALORES(A:A) - CONT.VALORES(UNIQUE(A:A)) para saber quantas duplicatas tinham na tabela.
            - Utilizei essa fórmula =CONTAR.VAZIO(B2:B22128) para saber quantos espaços nulos tinham na tabela.
            - Utilizei esses passos "Selecionar tabela, Formatar, Formatar Condicional, em 'formatar células se' cliquei em formula personalizada, aí coloquei essa fórmula: =CONT.SE($A:$A;$A2) >1" para identificar as duplicatas.
        - Unir tabelas
            
            Utilizei a fórmula =QUERY(clientes_limpos!A1:O2241; "select A,K,C,D,O,J,M,N,L,I where K <= 99"; -1) e  =PROCV(A2;resumo_compras_limpos!A2:I2241;2;0) para unir as tabelas na aba “analise_dados” e remover os nulos. Nela coloquei os dados id_cliente, idade, faixa_salarial, nivel_educacao, estado_civil, media_salarial,  total_filhos, total_gasto, data_entrada, ultima_compra, variacao_dias, transacoes, compras_online, compras_loja, resposta_campanha, ano_mes_entrada, variacoes_meses, variacao_compra total_vinhos, total_frutas, total_carnes, total_peixes, total_doces, total_outros.
            
        - Criar variáveis
            - Criei a variavel Transações com a fórmula =[CONT.SE](http://cont.se/)(transacoes!B:B;A2) para trazer os dados de quantas transações cada cliente fez.
                - Utilizei a fórmula =CONT.SE(transacoes!B:B;A2) para somar quantas transações cada cliente fez conforme os dados da aba "transacoes" e coloquei na aba "resumos_compras". Para aplicar em todas as linhas usei os atalhos shift+ctrl+seta pra baixo > coloquei até a última linha com valor e usei o atalho pra aplicar fórmula ctrl+d.
            - Usei subtração simples para trazer a variável de idade. Se possível verei outra fórmula para isso.
            - Usei =SOMA(F2;G2) para ter um total de filhos por cliente.
            - Utilizei a fórmula =SOMA(B2:G2) para saber o total gasto por cada cliente no resumo de compras e utilizei a fórmula =PROCV(A2;resumo_compras_limpos!A2:I2241;9;0) pra transferir esses dados para a aba de clientes.
            - Fiz a variável Ano/Mês com a fórmula =MÊS(H2) & "/" & ANO(H2) para saber o mês e ano de entrada de cada cliente inicialmente, mas acabou sendo mais interessante utilizar a fórmula =TEXTO(H2;"YYYY/MM") para determinar o ano e o mês ordenados na hora de passar para a tabela dinâmica.
            - Para preencher os dados nulos da coluna salário utilizei a fórmula =ARRAYFORMULA(SE(E2:E2241="";MÉDIA(FILTER(E2:E2241;E2:E2241<>""));E2:E2241)) para preencher esses dados com a média salarial dos clientes e não perder os dados de 24 clientes.
            - Para fazer obter os dados de última compra eu utilizei a fórmula =MAX(FILTER()).
            - Criei as variáveis Compra na loja e Compra online para saber o o número de vezes que cada tipo de compra foi realizado por cada cliente, utilizei a fórmula =CONT.SES(transacoes_limpos!D:D; "Online"; transacoes_limpos!B:B;A2).
            - Criei a variável Variação de compras para entender qual era a média de compras efetuadas pelos clientes do período que efetuou a primeira compra e a última, utilizei a fórmula =SE((O2-H2) = 0;J2; MÉDIA( J2 / (O2 - H2))).
            - Criei a variável Faixa etária para diminuir a variação de idades e facilitar o entendimento da faixa etária que compra nesta empresa, utilizei a fórmula =IFS(K2<=35; "Jovem adulto"; K2<=60; "Adulto"; K2>60; "Idoso").
        - Agrupar dados e visualizar variáveis categóricas
            
            Em outra aba agrupei os dados indicados em tabelas dinâmicas para inserir gráficos.
            
            Fiz tabelas dinâmicas na aba Dashboard e coloquei os seguinte dados para vizualisar: media_salarial (scorecard), transacoes (gráfico de barra), idade (gráfico de barras), escolaridade (gráfico de pizza), estado civil (gráfico de barra), filhos por cliente (gráfico de barra), feedback da campanha (gráfico de pizza), média dos produtos vendidos (gráfico de barra), segmentação de clientes (gráfico de barra), média de compras efetuadas pelos clientes (gráfico de barra), faixa etária (gráfico de pizza).
            
        - Calcular quartis, decis e percentis
            
            Entendi como fazer os cálculos acabei optando pelo percentis dividindo as informações em quintis, utilizei a fórmula =PERCENTIL(K:K;0,2) utilizando os valores de variações de dias da primeira pra última compra para obter a recência, para frequência utilizei os valores de total de transações realizadas pelos clientes e para o valor utilizei o valor total gasto por cada cliente.
            
        - Aplicar segmentação
            
            Pra realizar a segmentação RFM eu precisei utilizar essa fórmula =SE(K2<=$Y$2;5;SE(K2<=$Y$3;4;SE(K2<=$Y$4;3;SE(K2<=$Y$5;2;SE(K2<=$Y$6;1))))) em cada item, sendo o score de recência de 5 a 1 e o de frequência e valor de 1 a 5, depois de atribuir esses valores eu fiz o cálculo de média com a fórmula =ARRED(MÉDIA(T2:U2)) para obter a média de frequência e valor. Depois de atribuir todos esses valores pude utilizar a fórmula =IFS(E(S2<=2; V2<=2); "Hibernando";
            E(S2=1; V2>=4); "Não posso perdê-lo";
            E(S2<=2; V2>=2); "Risco de Perda";
            E(S2>=2; S2<=3; V2<2); "Sonolento";
            E(S2<=3; S2>=2; V2>=2; V2<=3); "Precisa de Atenção";
            E(S2>=3; S2<=4; V2=1); "Promissor";
            E(S2>=4; V2=1); "Cliente novo";
            E(S2>=3; V2>1; V2<=3); "Potencialmente Leal";
            E(S2>=2; S2<=4; V2>=3); "Cliente Leal";
            E(S2>=3; V2<=4); "Cliente Leal";
            E(S2<=4; S2<=5; V2>=4; V2<=5); "Campeão";
            E(S2<=5; S2>=5; V2>=5; V2<=5); "Campeão") para realizar a segmentação de clientes em 10 grupos com base nos scores de recência e média de frequência/valor. Os melhores clientes são campeões e os piores são hibernados. Finalizei colocando mapa de calor nos scores.
            
    - Marco 2
        - Contrui a tabela auxilar para realiza a análise de coorte.
        - Criei novas variáveis com os dados de transações na aba de análise de coorte depois de transferir os dados com a fórmula QUERY. Utilizei a fórmula =PROCV para trazer os dados de data de entrada para essa tabela, usei essa fórmula =DATADIF(E2; C2; "M") para obter a variação de meses entre a data de entrada e as transações, usei essa fórmula =TEXTO(E4;"YYYY/MM") para gerar a data Ano/Mês de entrada e de transação.
        - Para fazer a análise de coorte eu usei a tabela dinâmica e coloquei os dados Ano/Mês de entrada nas linhas, a variação de meses nas colunas e em valores eu coloquei o id de cliente. Finalizei colocando mapa de calor nos dados. Fiz a análise apenas com dados obtidos, não fiz por trimestre ou churn até então.
        - Após concluir a análise de coorte fiz um histograma com os dados de variações de meses e apliquei as medidas de tendência central com as fórmulas =MODO, =MÉDIA e =MED.
    - Marco 3
        - Importei os dados do Google Sheets para o Looker Studio e fiz um Dashboard com scorecards de valores totais e médios, gráficos de barra, gráficos de pizza, gráfico de linha do tempo e filtro. Usei os dados de segmentação de cliente para fazer o filtro, scorecards de média salarial, de valor total gasto, de faturamento total, valores totais gastos em produtos separados; os gráficos de barra estão os dados de escolaridade, faixa etária, filhos por cliente e estado civil; gráfico de pizza com os dados de lugares de compra e feedback de campanha de marketing; gráfico de séria temporal com os dados de id de cliente da análise de coorte dividido por trimestres.
        - O dashboard ficou interativo e permitiu uma análise fácil dos segmentos de clientes, além de uma análise da linha do tempo que permite observar quantas transações foram realizada neste período e onde começa o problema da empresa.
- **Resultados e Conclusões:**
    
    Com base nos dados, especialmente do Dashboard do Looker Studio, pude notar que a partir dos dois últimos trimestre desses dados o número de compras cai e potencializa uma possível crise na empresa. 
    
    A segmentação de clientes permite avaliar que os clientes Campeões são de grande valia para a empresa, especialmente para aumentar o número de clientes com o mesmo tipo de comportamento. Os maiores números de clientes ficaram nos grupos de Clientes Leais e Risco de Perda, que represantam 62% do faturamento total da empresa, mas que precisam aprender a gastar como os Campeões que tem a média de gasto em comparação a este grupo de 42% a mais.
    
    A grande maioria desses grupos são casados, têm ensino superior e estão na faixa etária entre 35 e 60 anos. Os clientes Campeões e Leais não têm filhos em sua maioria, mas os clientes de Risco de Perda têm na sua maioria 1 filho. O feedback da campanha de marketing é muito fraco em todos os segmentos e a busca compras na loja física é maior do que online. Os produtos favoritos dos clientes são vinhos e carnes, os menos favoritos são doces e frutas. 
    
- **Limitações/Próximos Passos:**
    
    Minhas dificuldades nesse projeto começaram na aplicação da análise de coorte que tive muita dificuldade de fazer a tabela dinâmica funcionar.
    
    Seria interessante utilizar mais dados para entender melhor os clientes e poder indicar mais detalhadamente os produtos. Outro ponto interessante seria saber mais sobre os produtos que são ofertados, pois ainda ficou essa lacuna em outros do que poderia combinar com os produtos mais vendidos.
    
- **Links de interesse:**
    
    [Segmentação - Google Sheets](https://docs.google.com/spreadsheets/d/1YFbIQwelFJRC1cwcz5htuXmeXHZ29SDGAlIC46p-1rs/edit#gid=1472137358)
    
    [Segmentação de “O Mercado” - Looker Studio](https://lookerstudio.google.com/reporting/bef6c3b9-8e32-47a1-ba6c-db140db38fd4/page/uvvuD)
    
    [**Análise de dados de “O Mercado” - Loom**](https://www.loom.com/share/e7bd6c80c3094846947a52db683d93d2?sid=d4ca9412-576c-4852-ab61-7e235341ca52)