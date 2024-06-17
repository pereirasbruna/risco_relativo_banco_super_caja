# Ficha Técnica

## 1. Identificação do Projeto

**Nome do Projeto:** Risco Relativo - Banco Caja

**Data de Início:** 15 - 05 - 2024

**Data de Término:** 14 - 06 - 2024

**Equipe do Projeto:** Bruna Santos

## 2. Objetivos

- avaliar o risco relativo que possa classificar os solicitantes em diferentes categorias de risco com base em sua probabilidade de inadimplência.
- desenvolver um score de crédito a partir de uma análise de dados;

## 3. Metodologia

**Ferramentas e Tecnologias:**

- Ferramenta de Análise (Python e Excel)
- Banco de Dados (SQL)
- Ferramentas de Visualização (Looker)

## 4. Dados

**Descrição dos Dados:**

- **Fontes de Dados:** Disponibilizado pelo cliente.
- **Formato dos Dados:** CSV
- **Volume de Dados:** 36 mil registros.

## 5. **Processamento e análises:**

- Processo de Limpeza: Nesta etapa, os dados foram analisados e limpos, de acordo com as necessidades encontradas ao explorar o dataset.
- Após a limpeza, as informações foram analisadas e novas variáveis foram estabelecidas para auxiliar na obtenção dos objetivos, como quantidade de emprestimos por pessoa e suas origens.
- Após a limpeza dos dados e a criação das variáveis, foi realizada uma análise de Risco Relativo, utilizando a coluna de histórico de inadimplência como base. Foram cruzadas as informações de perfil com o histórico de inadimplência, e o resultado do risco relativo foi utilizado para criar uma pontuação.
- A lógica da pontuação é baseada na presença ou ausência das características destacadas no risco relativo. Se o cliente possui um detalhe no perfil que apresenta risco, ele recebe uma pontuação. Ao todo, foram analisados sete pontos, permitindo categorizar o usuário de 0 a 7 pontos, sendo que uma pontuação a partir de 4 indica alto risco.

## 5. Resultados e conclusões:

Através da metodologia de risco relativo, foi possível conhecer características de comportamento de possíveis clientes inadimplentes. De todo o banco de clientes informado, somente 0,57% são considerados inadimplentes, por isso, tentar buscar  um padrão de comportamento entre tão poucos pode ser considerado desafiante, visto que são casos raros. No Banco Super Caja foram considerados clientes de alto risco 14,8% dos usuários, onde possuem comportamentos que devem ser analisados mais profundamente. Lembrando que não o score desenvolvido não caracteriza o cliente como inadimplentes, mas sim que este possui um perfil de risco. Para avaliações mais precisas dos clientes e do seu comportamento, será necessário o uso de modelos estatísticos mais elaborados, como regressão logística por exemplo. Esta metodologia até foi aplicada, porém os resultados gerados não foram satisfatórios e estes não incluídos no relatório final.

## 6. Limitações

Este projeto enfrentou diversas limitações provenientes de diferentes origens. Para começar, uma das principais limitações foi a falta de clareza nas informações sobre o negócio e seus objetivos. Por exemplo, ao criar o score ou realizar a limpeza dos dados, não ficou claro se o foco da análise era apenas identificar padrões gerais de inadimplência ou se deveria considerar uma segmentação específica de clientes. Essa falta de objetividade dispersou a análise, resultando em informações mais genéricas.

A falta de informações essenciais, como a moeda utilizada pelo banco (real, dólar, euro), valores dos empréstimos concedidos anteriormente, quantidade e valor das parcelas, comprometimento salarial e demais dados dos clientes, como escolaridade e patrimônio, foi um desafio significativo durante a manipulação dos dados.

Além disso, a falta de conhecimento em metodologias estatísticas prejudicou a modelagem das informações. Durante a criação do modelo de machine learning, algumas informações e manipulações no banco de dados ficaram em aberto, dificultando o sucesso do desenvolvimento do modelo.

É importante ressaltar que o desenho atual reflete o cenário econômico e social presente, sendo assim sujeito a mudanças e ajustes futuros.

O projeto será retomado futuramente com o objetivo de construir um modelo de regressão logística, permitindo que o analista do banco avalie melhor o risco de inadimplência dos clientes que buscam produtos de empréstimo.