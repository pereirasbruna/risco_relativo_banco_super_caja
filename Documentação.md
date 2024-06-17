# Documentação

**Risco Relativo para o Banco Caja**

O Banco Caja nos procurou para avaliar seu banco atual de clientes para compreender um pouco o perfil de pessoas com risco de inadimplência e assim, auxiliar na tomada de decisão da empresa.

**Contexto:**

- a diminuição das taxas de juros no mercado desencadeou um aumento significativo na demanda por solicitações de crédito;
- processo é manual e está ficando muito demorado;
- necessidade de automatizar processo;
- preocupação com a taxa de inadimplência mitigando o controle rigoroso de crédito;

**Objetivos:**

- Automatizar o processo de análise, utilizando técnicas avançadas de análise de dados, visando melhorar a eficiência, precisão e rapidez na avaliação das solicitações de crédito.

- desenvolver um score de crédito a partir de uma análise de dados;
- avaliar o risco relativo que possa classificar os solicitantes em diferentes categorias de risco com base em sua probabilidade de inadimplência.

**Informações do Dataset informado:**

| Informações dataset Banco Caja |  |  |
| --- | --- | --- |
| Arquivo | Variável | Descrição |
| user_info | user id | Número de identificação do cliente (único para cada cliente) |
|  | age | Idade do cliente |
|  | sex | Gênero do cliente |
|  | last month salary | Último salário mensal que o cliente informou ao banco |
|  | number dependents | Número de dependentes |
| loans_outstanding | loan id | Número de identificação do empréstimo (único para cada empréstimo) |
|  | user id | Número de identificação do cliente |
|  | loan type | Tipo de empréstimo (real state = imóveis, others= outros) |
| loans_detail | user id | Número de identificação do cliente |
|  | more 90 days overdue | Número de vezes que o cliente apresentou atraso superior a 90 dias |
|  | using lines not secured personal assets | Quanto o cliente está utilizando em relação ao seu limite de crédito, em linhas que não são garantidas por bens pessoais, como imóveis e automóveis |
|  | number times delayed payment loan 30 59 days | Número de vezes que o cliente atrasou o pagamento de um empréstimo (entre 30 e 59 dias) |
|  | debt ratio | Relação entre dívidas e ativos do cliente. Taxa de endividamento = Dívidas / Patrimonio |
|  | number times delayed payment loan 60 89 days | Número de vezes que o cliente atrasou o pagamento de um empréstimo (entre 60 e 89 dias) |
| default | user id | Número de identificação do cliente |
|  | default flag | Classificação dos clientes inadimplentes (1 para clientes já registrados alguma vez como inadimplentes, 0 para clientes sem histórico de inadimplência) |

**Marco 1:**

Perguntas norteadoras:

- Quais variáveis ​​mais influenciam o risco de não conformidade/inadimplência?
- Como essas variáveis ​​se correlacionam entre si e com o comportamento de pagamento do cliente?

**Conectar/importar dados para ferramentas:**Importado as tabelas user_info, loans_outstanding, loans_detail e default no Big Query.**Nome do projeto:** risco-relativo-laboratoria

**Nome do dataset:** banco_super_caja

**ID do conjunto de dados:** risco-relativo-laboratoria.banco_super_caja

**Identificar e tratar valores nulos:**

Processo importante para realizar a análise no futuro somente com informações relevantes.

Tabela:

default

Consulta:

```sql
SELECT *
FROM `risco-relativo-laboratoria.banco_super_caja.default`
WHERE
user_id IS NULL
OR default_flag IS NULL
```

---

Resultado:

Não houveram dados para exibir.

---

Tabela:

loans_detail

Consulta:

```sql
SELECT
*
FROM
`risco-relativo-laboratoria.banco_super_caja.loans_detail`
WHERE
user_id IS NULL
OR more_90_days_overdue IS NULL
OR using_lines_not_secured_personal_assets IS NULL
OR number_times_delayed_payment_loan_30_59_days IS NULL
OR debt_ratio IS NULL
OR number_times_delayed_payment_loan_60_89_days IS NULL
```

---

Resultado:

Não houveram dados para exibir.

---

Tabela:

loans_outstanding

Consulta:

```sql
SELECT
*
FROM
`risco-relativo-laboratoria.banco_super_caja.loans_outstanding`
WHERE
user_id IS NULL
OR loan_id IS NULL
OR loan_type IS NULL
```

---

Resultado:

Não houveram dados para exibir.

---

Tabela:

user_info

Consulta:

```sql
SELECT
*
FROM
`risco-relativo-laboratoria.banco_super_caja.user_info`
WHERE
user_id IS NULL
OR age IS NULL
OR sex IS NULL
OR last_month_salary IS NULL
OR number_dependents IS NULL
```

---

Resultado:

Houveram 7199 casos de nulos. Para saber o número de cada variável, foi aplicado o código abaixo:

```sql
SELECT
COUNT(CASE WHEN user_id IS NULL THEN 1 END) AS user_id_null_count,
COUNT(CASE WHEN age IS NULL THEN 1 END) AS age_null_count,
COUNT(CASE WHEN sex IS NULL THEN 1 END) AS sex_null_count,
COUNT(CASE WHEN last_month_salary IS NULL THEN 1 END) AS last_month_salary_null_count,
COUNT(CASE WHEN number_dependents IS NULL THEN 1 END) AS number_dependents_null_count
FROM
`risco-relativo-laboratoria.banco_super_caja.user_info`
```

---

Resultado:

| last_month_salary_null_count | number_dependents_null_count |
| --- | --- |
| 7199 | 943 |

Decisão:

Por ser uma informação muito importante, iremos manter os valores nulos no banco de dados.

**Identificar e tratar valores duplicados:**Importante para que não ocorra vieses na análise.

---

Tabela:

default

Consulta:

```sql
SELECT
user_id,
default_flag,
COUNT(*) AS amount
FROM
`risco-relativo-laboratoria.banco_super_caja.default`
GROUP BY
user_id,
default_flag
HAVING
COUNT(*) > 1
```

---

Resultado:

Não houveram dados para exibir.

---

Tabela:

loans_detail

Consulta:

```sql
SELECT
user_id,
more_90_days_overdue,
using_lines_not_secured_personal_assets,
number_times_delayed_payment_loan_30_59_days,
debt_ratio,
number_times_delayed_payment_loan_60_89_days,
COUNT(*) AS amount
FROM
`risco-relativo-laboratoria.banco_super_caja.loans_detail`
GROUP BY
user_id,
more_90_days_overdue,
using_lines_not_secured_personal_assets,
number_times_delayed_payment_loan_30_59_days,
debt_ratio,
number_times_delayed_payment_loan_60_89_days
HAVING
COUNT(*) > 1
```

---

Resultado:

Não houveram dados para exibir.

---

Tabela:

loans_outstanding

Consulta:

```sql
SELECT
user_id,
loan_id,
loan_type,
COUNT(*) AS amount
FROM
`risco-relativo-laboratoria.banco_super_caja.loans_outstanding`
GROUP BY
user_id,
loan_id,
loan_type
HAVING
COUNT(*) > 1
```

---

Resultado:

Não houveram dados para exibir.

---

Tabela:

user_info

Consulta:

```sql
SELECT
user_id,
age,
sex,
last_month_salary,
number_dependents,
COUNT(*) AS amount
FROM
`risco-relativo-laboratoria.banco_super_caja.user_info`
GROUP BY
user_id,
age,
sex,
last_month_salary,
number_dependents
HAVING
COUNT(*) > 1
```

---

Resultado:

Não houveram dados para exibir.

---

**Unir tabelas:**

Processo importante por reunir todas as informações necessárias em somente um arquivo.

Tabela usadas:

user_info, default e loans_detail.

Consulta:

```sql
SELECT
user_info.*,
default_table.default_flag,
loans_detail.more_90_days_overdue,
loans_detail.using_lines_not_secured_personal_assets,
loans_detail.number_times_delayed_payment_loan_30_59_days,
loans_detail.debt_ratio,
loans_detail.number_times_delayed_payment_loan_60_89_days
FROM
`risco-relativo-laboratoria.banco_super_caja.user_info` AS user_info
LEFT JOIN
`risco-relativo-laboratoria.banco_super_caja.default` AS default_table
ON
user_info.user_id = default_table.user_id
LEFT JOIN
`risco-relativo-laboratoria.banco_super_caja.loans_detail` AS loans_detail
ON
user_info.user_id = loans_detail.user_id
```

---

Resultado:

Foram unidas as informações de 3 tabelas. A tabela loans_outstanding não foi usada pois existem informações de todas as transições por cliente, atrapalhando na leitura dos dados.

**Identificar e gerenciar dados fora do escopo da análise:**Tomar decisões sobre outliers, nulos ou qualquer informação que não seja relevante para a análise.

---

Tabela:

uniao_tabelas

Consulta:

```sql
SELECT
CORR(age, last_month_salary) AS correlacao
FROM
`risco-relativo-laboratoria.banco_super_caja.uniao_tabelas`
```

---

Resultado:

Correlação Pagamento de empréstimo atrasado entre 60 e 89 dias e Pagamento de empréstimo atrasado +90 dias: 0.99217552634076256 - 99,22%

Correlação Pagamento de empréstimo atrasado entre 30 e 59 dias e Pagamento de empréstimo atrasado entre 60 e 89 dias: 0.98655364549867408 - 98,66%

Correlação + 90 Dias em atraso e Flag inadimplência: 0.30748450335121 - 30,75%

Correlação empréstimo atrasado entre 30 e 59 dias e Flag inadimplência: 0.29920788112363567 - 29,92%

Correlação empréstimo atrasado entre 60 e 89 dias e Flag inadimplência: 0.27825366272046015 - 27,82%

Correlação número de dependentes e último salário: 0.0779958809466694 - 7,80%

Correlação idade e último salário: 0.035978007340201588 - 3%

Correlação nº dependentes e Flag inadimplência: 0.03192611935203380 - 3,19%

Correlação Inadimplência e último salário: 0.021943879599816363 - 2,19%

Correlação Inadimplência + 90 dias e último salário: 0.01362021891828010 - 1,36%

Correlação idade e número de dependentes: -0.21661156571947512 - -21,66%

Variável escolhida para futuras análises:

- more_90_days_overdue - 4.1213074268532566

---

**Identificar e tratar dados inconsistentes em variáveis ​​categóricas:**Ajustar valores inconsistentes.

Tabela:

loans_outstanding

Consulta:

```sql
CREATE OR REPLACE TABLE `risco-relativo-laboratoria.banco_super_caja.loans_outstanding` AS
SELECT
*,
UPPER(loan_type) AS loan_type_1
FROM
`risco-relativo-laboratoria.banco_super_caja.loans_outstanding`
```

---

Resultado:
Criado tabela com a coluna loan_type_1 em maiúsculo. Será necessário excluir a coluna repetida e atualizar o nome.

Consulta:

```sql
CREATE OR REPLACE TABLE `risco-relativo-laboratoria.banco_super_caja.loans_outstanding` AS
SELECT
user_id,
loan_id,
loan_type_1 AS loan_type
FROM
`risco-relativo-laboratoria.banco_super_caja.loans_outstanding`
```

---

Resultado:
Ajustado a tabela com as informações corretas.

**Identificar e tratar dados discrepantes em variáveis ​​numéricas**

Tabela:

uniao_tabelas

Informações encontradas:
Salário: 1560100

Entre 30 e 59 dias de atraso no pagamento: 96 e 98

Entre 60 e 89 dias de atraso no pagamento: 96 e 98

+ 90 dias de atraso no pagamento: 96 e 98

Decisão:
Não foram excluídos pois podem ser importantes para mais análises futuramente.

**Criar novas variáveis ​​**

Tabela:

loans_outstanding

Código:

```sql
SELECT DISTINCT loan_type
FROM risco-relativo-laboratoria.banco_super_caja.loans_outstanding
```

---

Decisão:
Foi encontrado as informações na coluna loan_type other, others e Real Estate. Neste sentido, vamos normalizar a informação de other para others.

Código:

```sql
create or replace table risco-relativo-laboratoria.banco_super_caja.loans_outstanding_v2 As
SELECT
*,
CASE
WHEN loan_type = 'OTHER' THEN 'OTHERS'
ELSE loan_type
END AS corrected_loan_type
FROM
`risco-relativo-laboratoria.banco_super_caja.loans_outstanding`;
```

---

Resultado:

Criado uma nova tabela com as informações corrigidas, entretanto com a coluna antiga e a nova de loan_type. Neste caso foi feito um replace na tabela, tirando a coluna antiga e renomeando a nova, conforme código abaixo:

```sql
create or replace table risco-relativo-laboratoria.banco_super_caja.loans_outstanding As
SELECT
user_id,
loan_id,
corrected_loan_type as loan_type
FROM
`risco-relativo-laboratoria.banco_super_caja.loans_outstanding_v2`;
```

---

Após isso, foi calculado quantos empréstimos cada pessoa fez e através deste foi gerado um join com a tabela *uniao_tabelas*, através do código abaixo:

```sql
CREATE OR REPLACE TABLE risco-relativo-laboratoria.banco_super_caja.uniao_tabelas AS
SELECT
u.*,
COALESCE(t.loan_count, 0) AS loan_count
FROM
risco-relativo-laboratoria.banco_super_caja.uniao_tabelas AS u
LEFT JOIN
(SELECT
user_id,
COUNT(*) AS loan_count
FROM
risco-relativo-laboratoria.banco_super_caja.loans_outstanding
GROUP BY
user_id) AS t
ON
t.user_id = u.user_id;
```

---

Resultado:

Criado a coluna de quantidade de empréstimos feitos por cliente cadastrado.

Criação da coluna real_estate_count e others_count para mostrar qual era a quantidade de cada tipo de empréstimo feito por cliente:

Código:

```sql
CREATE OR REPLACE TABLE risco-relativo-laboratoria.banco_super_caja.uniao_tabelas AS
SELECT
u.*,
t.real_estate_count,
t.others_count
FROM
risco-relativo-laboratoria.banco_super_caja.uniao_tabelas AS u
LEFT JOIN
(SELECT
user_id,
SUM(CASE WHEN loan_type = 'REAL ESTATE' THEN 1 ELSE 0 END) AS real_estate_count,
SUM(CASE WHEN loan_type = 'OTHERS' THEN 1 ELSE 0 END) AS others_count
FROM
risco-relativo-laboratoria.banco_super_caja.loans_outstanding
GROUP BY
user_id) AS t
ON
t.user_id = u.user_id;
```

---

Resultado:
Colunas criadas e adicionadas na tabela uniao_tabelas.

---

**Contagem de valores nulos:**

Código:

```sql
SELECT
COUNT(CASE WHEN age IS NULL THEN 1 END) AS age_null_count,
COUNT(CASE WHEN sex IS NULL THEN 1 END) AS sex_null_count,
COUNT(CASE WHEN last_month_salary IS NULL THEN 1 END) AS last_month_salary_null_count,
COUNT(CASE WHEN number_dependents IS NULL THEN 1 END) AS number_dependents_null_count,
COUNT(CASE WHEN default_flag IS NULL THEN 1 END) AS default_flag_null_count,
COUNT(CASE WHEN more_90_days_overdue IS NULL THEN 1 END) AS more_90_days_overdue_null_count,
COUNT(CASE WHEN using_lines_not_secured_personal_assets IS NULL THEN 1 END) AS using_lines_not_secured_personal_assets_null_count,
COUNT(CASE WHEN number_times_delayed_payment_loan_30_59_days IS NULL THEN 1 END) AS number_times_delayed_payment_loan_30_59_days_null_count,
COUNT(CASE WHEN debt_ratio IS NULL THEN 1 END) AS debt_ratio_null_count,
COUNT(CASE WHEN number_times_delayed_payment_loan_60_89_days IS NULL THEN 1 END) AS number_times_delayed_payment_loan_60_89_days_null_count,
COUNT(CASE WHEN loan_count IS NULL THEN 1 END) AS loan_count_null_count,COUNT(CASE WHEN real_estate_count IS NULL THEN 1 END) AS real_estate_null_count,COUNT(CASE WHEN others_count IS NULL THEN 1 END) AS others_count_null_count,COUNT(CASE WHEN age_quintile IS NULL THEN 1 END) AS age_quintile_null_count
FROM
`risco-relativo-laboratoria.banco_super_caja.uniao_tabelas`;
```

Neste código, foi calculado o valor de nulos em cada coluna. Nas colunas last_month_salary, number_dependents, real_estate_count e others_count foram encontrados valores nulos. Neste caso, foi decidido preencher os valores vazios com número 0 neste momento e os valores organizados, de acordo com o código abaixo:

---

```sql
CREATE OR REPLACE TABLE risco-relativo-laboratoria.banco_super_caja.uniao_tabelas AS
SELECT
user_id,
age,
age_quintile,
sex,
COALESCE(number_dependents, 0) AS number_dependents,
COALESCE(last_month_salary, 0) AS last_month_salary,last_month_salary_quintile,
default_flag,
using_lines_not_secured_personal_assets,
number_times_delayed_payment_loan_30_59_days,
number_times_delayed_payment_loan_60_89_days,
more_90_days_overdue,
debt_ratio,
loan_count,
COALESCE(real_estate_count, 0) AS real_estate_count,
COALESCE(others_count, 0) AS others_count,
FROM
`risco-relativo-laboratoria.banco_super_caja.uniao_tabelas`;
```

Ao conversar com as colegas, decidimos fazer uma nova tabela contendo os valores nulos, para futuramente realizar as análises e tomar as melhores decisões. Depois de refazer a união e manter os nulos, foram encontradas as seguintes informações:

| number_dependents_nulls | last_month_salary_nulls | loan_count_nulls | real_estate_count_nulls | others_count_nulls |
| --- | --- | --- | --- | --- |
| 943 | 7199 | 425 | 425 | 425 |

---

**Análise exploratória inicial:**

Foi criado no Looker Studio algumas visualizações para compreender melhor como os dados estão se comportando.

![Documentac%CC%A7a%CC%83o%20b8c66a71e5c24e7db5f5c4aac6fcb606/image1.png](Documentac%CC%A7a%CC%83o%20b8c66a71e5c24e7db5f5c4aac6fcb606/image1.png)

![Documentac%CC%A7a%CC%83o%20b8c66a71e5c24e7db5f5c4aac6fcb606/image8.png](Documentac%CC%A7a%CC%83o%20b8c66a71e5c24e7db5f5c4aac6fcb606/image8.png)

![Documentac%CC%A7a%CC%83o%20b8c66a71e5c24e7db5f5c4aac6fcb606/image2.png](Documentac%CC%A7a%CC%83o%20b8c66a71e5c24e7db5f5c4aac6fcb606/image2.png)

![Documentac%CC%A7a%CC%83o%20b8c66a71e5c24e7db5f5c4aac6fcb606/image6.png](Documentac%CC%A7a%CC%83o%20b8c66a71e5c24e7db5f5c4aac6fcb606/image6.png)

Importante ressaltar que os valores apresentados possuem nulos e outliers não tratados. Podendo assim o quadro futuramente possuir informações diferentes em caso de manipulações extras nos dados.

![Documentac%CC%A7a%CC%83o%20b8c66a71e5c24e7db5f5c4aac6fcb606/image4.png](Documentac%CC%A7a%CC%83o%20b8c66a71e5c24e7db5f5c4aac6fcb606/image4.png)

Neste sentido, foram criadas mais variáveis para auxiliar na tomada de decisões.

```sql
CREATE OR REPLACE TABLE `risco-relativo-laboratoria.banco_super_caja.uniao_tabelas_com_nulos` AS
SELECT
*,
NTILE(4) OVER (ORDER BY age) AS age_quartile,
NTILE(4) OVER (ORDER BY number_dependents) AS number_dependents_quartile,
NTILE(4) OVER (ORDER BY last_month_salary) AS last_month_salary_quartile,
NTILE(4) OVER (ORDER BY using_lines_not_secured_personal_assets) AS using_lines_not_secured_personal_assets_quartile,
NTILE(4) OVER (ORDER BY debt_ratio) AS debt_ratio_quartile,
NTILE(4) OVER (ORDER BY loan_count) AS loan_count_quartile,
FROM `risco-relativo-laboratoria.banco_super_caja.uniao_tabelas_com_nulos`
```

---

Depois de pesquisar, foram feitas duas análises, uma contendo os valores nulos e outra substituindo os nulos por mediana. A mediana foi escolhida por não influenciar na distribuição dos dados.

Código:

```sql
CREATE OR REPLACE TABLE
`risco-relativo-laboratoria.banco_super_caja.uniao_tabelas_sem_nulos_v2` AS
WITH
MedianCalculation AS (
SELECT
PERCENTILE_CONT(number_dependents, 0.5) OVER() AS mediana_number_dependents,
PERCENTILE_CONT(last_month_salary, 0.5) OVER() AS mediana_last_month_salary,
PERCENTILE_CONT(loan_count, 0.5) OVER() AS mediana_loan_count,
PERCENTILE_CONT(real_estate_count, 0.5) OVER() AS mediana_real_estate_count,
PERCENTILE_CONT(others_count, 0.5) OVER() AS mediana_others_count
FROM
`risco-relativo-laboratoria.banco_super_caja.uniao_tabelas_sem_nulos`
WHERE
number_dependents IS NOT NULL
AND last_month_salary IS NOT NULL
AND loan_count IS NOT NULL
AND real_estate_count IS NOT NULL
AND others_count IS NOT NULL ),
MedianValues AS (
SELECT
DISTINCT mediana_number_dependents,
mediana_last_month_salary,
mediana_loan_count,
mediana_real_estate_count,
mediana_others_count
FROM
MedianCalculation ),
DataWithFilledNulls AS (
SELECT
*,
COALESCE(number_dependents, (
SELECT
mediana_number_dependents
FROM
MedianValues)) AS number_dependents_filled,
COALESCE(last_month_salary, (
SELECT
mediana_last_month_salary
FROM
MedianValues)) AS last_month_salary_filled,
COALESCE(loan_count, (
SELECT
mediana_loan_count
FROM
MedianValues)) AS loan_count_filled,
COALESCE(real_estate_count, (
SELECT
mediana_real_estate_count
FROM
MedianValues)) AS real_estate_count_filled,
COALESCE(others_count, (
SELECT
mediana_others_count
FROM
MedianValues)) AS others_count_filled
FROM
`risco-relativo-laboratoria.banco_super_caja.uniao_tabelas_sem_nulos` )
SELECT
user_id,
age,
sex,
has_dependents,
default_flag,
using_lines_not_secured_personal_assets,
number_times_delayed_payment_loan_30_59_days,
number_times_delayed_payment_loan_60_89_days,
more_90_days_overdue,
debt_ratio,
number_dependents_filled AS number_dependents,
last_month_salary_filled AS last_month_salary,
loan_count_filled AS loan_count,
real_estate_count_filled AS real_estate_count,
others_count_filled AS others_count
FROM
DataWithFilledNulls;
```

---

Agora foram calculados os quintiles novamente e a partir deste, continuar a análise.

```sql
CREATE OR REPLACE TABLE `risco-relativo-laboratoria.banco_super_caja.uniao_tabelas_sem_nulos` AS
SELECT
*,
NTILE(5) OVER (ORDER BY age) AS age_quintile,
NTILE(5) OVER (ORDER BY number_dependents) AS number_dependents_quintile,
NTILE(5) OVER (ORDER BY last_month_salary) AS last_month_salary_quintile,
NTILE(5) OVER (ORDER BY using_lines_not_secured_personal_assets) AS using_lines_not_secured_personal_assets_quintile,
NTILE(5) OVER (ORDER BY debt_ratio) AS debt_ratio_quintile,
NTILE(5) OVER (ORDER BY loan_count) AS loan_count_quintile,
NTILE(5) OVER (ORDER BY number_times_delayed_payment_loan_30_59_days) AS number_times_delayed_payment_loan_30_59_days_quintile,
NTILE(5) OVER (ORDER BY number_times_delayed_payment_loan_60_89_days) AS number_times_delayed_payment_loan_60_89_days_quintile,
NTILE(5) OVER (ORDER BY more_90_days_overdue) AS more_90_days_overdue_quintile,
NTILE(5) OVER (ORDER BY real_estate_count) AS real_estate_count_quintile,
NTILE(5) OVER (ORDER BY others_count) AS others_count_quintile,
FROM `risco-relativo-laboratoria.banco_super_caja.uniao_tabelas_sem_nulos`
```

---

Após conversar com as colegas, tomamos a decisão de retirar os usuários que não possuíam empréstimos. Neste caso, foi realizada a seguinte consulta, transformando o banco de dados de 36000 para 35575 usuários.

```sql
CREATE OR REPLACE TABLE risco-relativo-laboratoria.banco_super_caja.uniao_tabelas_sem_nulosAS
SELECT
*
FROM
risco-relativo-laboratoria.banco_super_caja.uniao_tabelas_sem_nulos
WHERE user_id NOT IN (
9884, 18876, 865, 14999, 24483, 3108, 28122, 12771, 30541, 3683, 32473, 14767, 20824, 35568, 31904, 7457,
17431, 12798, 12647, 23057, 26354, 7328, 31655, 252, 15497, 18765, 10498, 19010, 20887, 5558, 5323, 26777,
29247, 20605, 23390, 28253, 25661, 12542, 29590, 12589, 11279, 25244, 9518, 29278, 12253, 6359, 23007,
21362, 35069, 13935, 22882, 8394, 4955, 22169, 3833, 24667, 33591, 20491, 33497, 8445, 30272, 13031, 31819,
29333, 24420, 15513, 10906, 6943, 25816, 6748, 20647, 5651, 18086, 793, 17166, 7192, 17857, 5096, 16129,
5262, 12628, 21211, 3354, 36000, 32941, 35980, 16493, 9443, 7874, 35897, 26641, 27586, 15628, 31743, 12021,
20144, 26500, 35411, 25920, 32745, 3146, 31073, 6115, 9882, 17119, 25261, 14178, 9803, 5446, 5403, 20449,
21835, 18593, 23020, 27069, 28069, 21961, 5927, 17717, 15377, 33168, 430, 28429, 19976, 10883, 35172, 26003,
13719, 25845, 28990, 5293, 21016, 30829, 15201, 27542, 2322, 21442, 23092, 29077, 4195, 24161, 32277, 20131,
34642, 12192, 32880, 16913, 8058, 14674, 13537, 34367, 19007, 19634, 27702, 22312, 15957, 29931, 24046, 6319,
28233, 11120, 4014, 29111, 10039, 22762, 33470, 30818, 3368, 21644, 29396, 32628, 31037, 12039, 8840, 23614,
28083, 33750, 29732, 1712, 21186, 15015, 7063, 29046, 21877, 12927, 8618, 6787, 26727, 32229, 25341, 18496,
16385, 4207, 1161, 297, 8696, 25650, 32706, 6221, 19110, 7679, 23732, 22641, 35642, 12769, 120, 4256, 15325,
21610, 27163, 5378, 19144, 7657, 4516, 14422, 12415, 20233, 15254, 3392, 32600, 27245, 28694, 15142, 30389,
17104, 4628, 2320, 22527, 25933, 32351, 348, 14046, 56, 30513, 5800, 26470, 2341, 3876, 2741, 16369, 29301,
6489, 14891, 3465, 6444, 11680, 655, 9806, 27188, 2246, 10308, 13306, 14543, 18161, 27941, 16611, 13520,
19316, 9221, 7866, 8841, 25426, 7897, 11718, 26273, 4281, 33551, 5653, 1282, 5385, 208, 25254, 29793, 24308,
18533, 11573, 24637, 29725, 8053, 18828, 21589, 21056, 31517, 27611, 7943, 13515, 12792, 2878, 5597, 1339,
30708, 24579, 13649, 27140, 19716, 2012, 32240, 5900, 20057, 8859, 3927, 22269, 26074, 7198, 29234, 35915,
19107, 32999, 6494, 31346, 3358, 34270, 32427, 31126, 23825, 28247, 18465, 33138, 30123, 34396, 3202, 5668,
9665, 14311, 6486, 25915, 20202, 27755, 31905, 20430, 1163, 14993, 34011, 26452, 7550, 25653, 35013, 13258,
59, 33619, 13407, 26409, 12561, 7452, 9539, 5576, 11010, 7555, 20488, 19503, 22807, 4392, 10631, 22723, 35216,
21559, 21005, 15626, 23576, 10728, 1193, 15194, 30737, 16212, 24297, 1333, 16778, 11910, 32822, 27783, 28705,
23491, 23888, 5965, 9962, 12178, 17253, 29538, 18588, 32408, 2597, 1670, 2837, 32752, 12629, 6737, 4645, 5644,
21099, 13674, 12152, 774, 10202, 2291, 29188, 719, 10987, 21520, 4010, 17525, 31022, 24893, 610, 20082, 1958,
27311, 15113, 21241, 13471, 16306, 768, 27846, 2525, 169, 10424, 22315, 29257, 20501, 11430, 17267, 21476,
16003, 10245, 26924, 19012);
```

---

**Cálculo Risco Relativo:**

Risco relativo é a probabilidade que um indivíduo do grupo exposto desenvolver a causa do estudo relativa à probabilidade de um indivíduo do grupo não-exposto desenvolver a mesma causa. Isso é comumente utilizado no estudo de doenças e uso de medicações.

```sql
WITH Totals AS (
SELECT
COUNTIF(default_flag = 1) AS default_1,
COUNTIF(default_flag = 0) AS default_0
FROM risco-relativo-laboratoria.banco_super_caja.uniao_tabelas_sem_nulos
)

SELECT
quintile,
variaveis,
COUNTIF(default_flag = 1) / t.default_1 AS alto_risco,
COUNTIF(default_flag = 0) / t.default_0 AS baixo_risco,
(COUNTIF(default_flag = 1) / t.default_1) / (COUNTIF(default_flag = 0) / t.default_0) AS risco_entre_baixo_risco,
CASE
WHEN (COUNTIF(default_flag = 1) / t.default_1) / (COUNTIF(default_flag = 0) / t.default_0) > 1 THEN 'Alto risco de inadimplencia'
WHEN (COUNTIF(default_flag = 1) / t.default_1) / (COUNTIF(default_flag = 0) / t.default_0) < 1 THEN 'Baixo risco de inadimplencia'
ELSE 'Risco indefinido'
END AS categoria
FROM (
SELECT
age_quintile AS quintile, 'age' AS variaveis, default_flag FROM risco-relativo-laboratoria.banco_super_caja.uniao_tabelas_sem_nulos
UNION ALL
SELECT
number_dependents_quintile AS quintile, 'number_dependents' AS variaveis, default_flag FROM risco-relativo-laboratoria.banco_super_caja.uniao_tabelas_sem_nulos
UNION ALL
SELECT
last_month_salary_quintile AS quintile, 'last_month_salary' AS variaveis, default_flag FROM risco-relativo-laboratoria.banco_super_caja.uniao_tabelas_sem_nulos
UNION ALL
SELECT
using_lines_not_secured_personal_assets_quintile AS quintile, 'using_lines_not_secured_personal_assets' AS variaveis, default_flag FROM risco-relativo-laboratoria.banco_super_caja.uniao_tabelas_sem_nulos
UNION ALL
SELECT
debt_ratio_quintile AS quintile, 'debt_ratio' AS variaveis, default_flag FROM risco-relativo-laboratoria.banco_super_caja.uniao_tabelas_sem_nulos
UNION ALL
SELECT
loan_count_quintile AS quintile, 'loan_count' AS variaveis, default_flag FROM risco-relativo-laboratoria.banco_super_caja.uniao_tabelas_sem_nulos
UNION ALL
SELECT
number_times_delayed_payment_loan_30_59_days_quintile AS quintile, 'number_times_delayed_payment_loan_30_59_days' AS variaveis, default_flag FROM risco-relativo-laboratoria.banco_super_caja.uniao_tabelas_sem_nulos
UNION ALL
SELECT
number_times_delayed_payment_loan_60_89_days_quintile AS quintile, 'number_times_delayed_payment_loan_60_89_days' AS variaveis, default_flag FROM risco-relativo-laboratoria.banco_super_caja.uniao_tabelas_sem_nulos
UNION ALL
SELECT
more_90_days_overdue_quintile AS quintile, 'more_90_days_overdue' AS variaveis, default_flag FROM risco-relativo-laboratoria.banco_super_caja.uniao_tabelas_sem_nulos
UNION ALL
SELECT
real_estate_count_quintile AS quintile, 'real_estate_count' AS variaveis, default_flag FROM risco-relativo-laboratoria.banco_super_caja.uniao_tabelas_sem_nulos
UNION ALL
SELECT
others_count_quintile AS quintile, 'others_count' AS variaveis, default_flag FROM risco-relativo-laboratoria.banco_super_caja.uniao_tabelas_sem_nulos
) AS combined_data, Totals t
GROUP BY quintile, variaveis, t.default_1, t.default_0
ORDER BY variaveis, quintile;
```

---

**Fazendo variáveis dummies:**

As variáveis dummies foram feitas através do resultado do risco relativo. Por exemplo, relativo à variável idade foi criado a coluna dummy_age onde se o percentil deste cliente está dentro do risco relativo maior que 1, na coluna dummy_age terá presença do número 1. Caso negativo, a presença do número 0. Assim, a máquina pode entender a presença ou ausência de certas características, necessárias para ensinamento de máquina. Além disso, essa metodologia foi necessária para a criação do score caja, sendo este o score de risco da Banco Caja. Como nota de corte, foi designado como cliente alto risco de inadimplência quem ficou com a nota <=4.

Código:

```sql
SELECT
*,
CASE WHEN age_quintile IN (1, 2, 3) THEN 1 ELSE 0 END AS dummy_age,
CASE WHEN debt_ratio_quintile = 4 THEN 1 ELSE 0 END AS dummy_debt_ratio,
CASE WHEN loan_count IN (1, 2) THEN 1 ELSE 0 END AS dummy_loan_count,
CASE WHEN last_month_salary_quintile IN (1, 2) THEN 1 ELSE 0 END AS dummy_last_month_salary,
CASE WHEN more_90_days_overdue_quintile = 5 THEN 1 ELSE 0 END AS dummy_more_90_days_overdue,
CASE WHEN number_dependents_quintile IN (4, 5) THEN 1 ELSE 0 END AS dummy_number_dependents,
CASE WHEN using_lines_not_secured_personal_assets_quintile = 5 THEN 1 ELSE 0 END AS dummy_using_lines_not_secured_personal_assets,
(CASE WHEN age_quintile IN (1, 2, 3) THEN 1 ELSE 0 END +
CASE WHEN debt_ratio_quintile = 4 THEN 1 ELSE 0 END +
CASE WHEN last_month_salary_quintile IN (1, 2) THEN 1 ELSE 0 END +
CASE WHEN more_90_days_overdue_quintile = 5 THEN 1 ELSE 0 END +
CASE WHEN number_dependents_quintile IN (4, 5) THEN 1 ELSE 0 END +
CASE WHEN using_lines_not_secured_personal_assets_quintile = 5 THEN 1 ELSE 0 END +
CASE WHEN loan_count IN (1, 2) THEN 1 ELSE 0 END) AS score,
CASE WHEN
(CASE WHEN age_quintile IN (1, 2, 3) THEN 1 ELSE 0 END +
CASE WHEN debt_ratio_quintile = 4 THEN 1 ELSE 0 END +
CASE WHEN last_month_salary_quintile IN (1, 2) THEN 1 ELSE 0 END +
CASE WHEN more_90_days_overdue_quintile = 5 THEN 1 ELSE 0 END +
CASE WHEN number_dependents_quintile IN (4, 5) THEN 1 ELSE 0 END +
CASE WHEN using_lines_not_secured_personal_assets_quintile = 5 THEN 1 ELSE 0 END +
CASE WHEN loan_count IN (1, 2) THEN 1 ELSE 0 END)
IN (0, 1, 2, 3) THEN 0 ELSE 1 END AS score_range
FROM
`risco-relativo-laboratoria.banco_super_caja.uniao_tabelas_sem_nulos`;
```

---

**Tradução do dataset para portugues**
Foi realizada a tradução do dataset para Português.

```sql
CREATE TABLE `risco-relativo-laboratoria.banco_super_caja.tabela_pt` AS
SELECT
user_id AS id_usuario,
age AS idade,
sex AS sexo,
has_dependents AS possui_dependentes,
default_flag AS classificacao_inadimplente,
using_lines_not_secured_personal_assets AS usando_linhas_nao_seguradas_com_bens_pessoais,
number_times_delayed_payment_loan_30_59_days AS numero_vezes_atraso_pagamento_emprestimo_30_59_dias,
number_times_delayed_payment_loan_60_89_days AS numero_vezes_atraso_pagamento_emprestimo_60_89_dias,
more_90_days_overdue AS mais_de_90_dias_vencido,
debt_ratio AS taxa_de_endividamento,
number_dependents AS numero_dependentes,
last_month_salary AS salario_mes_passado,
loan_count AS quantidade_emprestimos,
real_estate_count AS quantidade_emprestimos_imoveis,
others_count AS quantidade_emprestimos_outros,
age_quintile AS quintil_idade,
number_dependents_quintile AS quintil_numero_dependentes,
last_month_salary_quintile AS quintil_salario_mes_passado,
using_lines_not_secured_personal_assets_quintile AS quintil_usando_linhas_nao_seguradas_com_bens_pessoais,
debt_ratio_quintile AS quintil_taxa_de_endividamento,
loan_count_quintile AS quintil_quantidade_emprestimos,
number_times_delayed_payment_loan_30_59_days_quintile AS quintil_numero_vezes_atraso_pagamento_emprestimo_30_59_dias,
number_times_delayed_payment_loan_60_89_days_quintile AS quintil_numero_vezes_atraso_pagamento_emprestimo_60_89_dias,
more_90_days_overdue_quintile AS quintil_mais_de_90_dias_vencido,
real_estate_count_quintile AS quintil_quantidade_emprestimos_imoveis,
others_count_quintile AS quintil_quantidade_emprestimos_outros,
dummy_age AS dummy_idade,
dummy_debt_ratio AS dummy_taxa_de_endividamento,
dummy_loan_count AS dummy_quantidade_emprestimos,
dummy_last_month_salary AS dummy_salario_mes_passado,
dummy_more_90_days_overdue AS dummy_mais_de_90_dias_vencido,
dummy_number_dependents AS dummy_numero_dependentes,
using_lines_not_secured_personal_assets AS dummy_usando_linhas_nao_seguradas_com_bens_pessoais,
score AS pontuacao,
score_range AS dummy_pontuacao
FROM `risco-relativo-laboratoria.banco_super_caja.uniao_tabelas_sem_nulos`;
```

---

**Dashboard:**
Foram escolhidos para o dashboard tons mais terrosos e alaranjados para apresentar um ar de sofisticação em conjunto com as cores do fruto Caja. O relatório possui 3 abas. Na primeira apresenta informações gerais relacionadas ao perfil dos clientes do banco.

![Documentac%CC%A7a%CC%83o%20b8c66a71e5c24e7db5f5c4aac6fcb606/image7.png](Documentac%CC%A7a%CC%83o%20b8c66a71e5c24e7db5f5c4aac6fcb606/image7.png)

Na segunda, apresenta valores relacionados com o comportamento financeiro do cliente e seu possível risco:

![Documentac%CC%A7a%CC%83o%20b8c66a71e5c24e7db5f5c4aac6fcb606/image5.png](Documentac%CC%A7a%CC%83o%20b8c66a71e5c24e7db5f5c4aac6fcb606/image5.png)

E por último, é apresentado ao banco o resultado das hipóteses trazidas pelo time de negócio.

![Documentac%CC%A7a%CC%83o%20b8c66a71e5c24e7db5f5c4aac6fcb606/image3.png](Documentac%CC%A7a%CC%83o%20b8c66a71e5c24e7db5f5c4aac6fcb606/image3.png)

**Conclusões:**
Através da metodologia de risco relativo foi possível conhecer características de comportamento de possíveis clientes inadimplentes. De todo o banco de clientes informados, somente 0,57% são considerados inadimplentes, por isso, tentar buscar um padrão de comportamento entre tão poucos pode ser considerado desafiante, visto que são casos considerados raros.

Para avaliações mais precisas, será necessário o uso de modelos estatísticos mais elaborados, como regressão logística por exemplo. Esta metodologia foi aplicada, porém os resultados gerados não foram satisfatórios, não sendo incluídos no relatório final.

**Limitações**:
Foi considerado uma limitação neste projeto informações sobre o negócio e seus objetivos. No momento de criar o score ou de realizar a limpeza dos dados, não ficou claro o foco da análise, se seriam somente para reconhecer o padrão dos inadimplentes em geral ou dentro de uma faixa de segmentação de clientes. Esta falta de objetividade acabou dispersando um pouco a análise, gerando informações mais genéricas.

A falta de informações, como a moeda utilizada pelo banco (real, dólar, euro), valores dos empréstimos concedidos anteriormente, quantidade e valor das parcelas, comprometimento salarial e demais informações dos clientes, como escolaridade, patrimônio, entre outros, foi sentida durante a manipulação dos dados.

A falta de conhecimento em metodologias estatísticas também esteve presente durante a modelagem das informações. Durante a criação do modelo de machine learning algumas informações e manipulações no banco de dados ficaram em aberto, acabando atrapalhando no sucesso do desenvolvimento do modelo.

Importante ressaltar que o desenho reflete o cenário econômico e social atual, sendo assim passível de mudanças e também de ajustes futuros.

O projeto será retomado futuramente para a construção de um modelo de regressão logística onde o analista do banco poderá avaliar melhor o risco de inadimplência do cliente que procura o produto de empréstimo.