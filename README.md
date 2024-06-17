<p align="center">
 Análise de dados do dataset de Mortes por Overdose de Opioides
</p>

## Declaração de Uso dos Dados

Este repositório contém análises e visualizações de dados do conjunto de dados "Opioid Overdose Deaths" obtido do [Kaggle](https://www.kaggle.com/datasets/jazzang/opioid-overdose-deaths). 

### Conformidade com a Lei do Serviço de Saúde Pública (42 USC 242m(d))

Os dados são usados exclusivamente para fins de relatórios e análises estatísticas de saúde. Todas as análises e visualizações foram realizadas em conformidade com as diretrizes legais, garantindo que:
- Não há esforços para identificar quaisquer casos relatados.
- Não são apresentadas nem publicadas contagens de mortes de 9 ou menos, nem taxas de mortalidade baseadas em contagens de 9 ou menos.

## Sobre

<p align=justify>A dependência de opioides e as taxas de mortalidade relacionadas a esses medicamentos atingiram níveis epidêmicos nos Estados Unidos e em outros países. Com o aumento significativo dos casos de overdose envolvendo opioides, é essencial realizar novas pesquisas para entender melhor essa crise de saúde pública. Este relatório apresenta uma análise do conjunto de dados "Opioid Overdose Deaths", que inclui informações detalhadas sobre mortes por overdose, populações, taxas de mortalidade e prescrições de opioides entre os anos de 1999 e 2014.</p>

## Descrição do Dataset

<p align=justify>O dataset contém as seguintes variáveis:</p>

- **State (Estado)**
  - Valores únicos: 51
  - Válidos: 816 (100%)
  - Incompatíveis: 0 (0%)
  - Faltantes: 0 (0%)
  - Mais Comum: Alabama (2%)

- **Year (Ano)**
  - Válidos: 816 (100%)
  - Incompatíveis: 0 (0%)
  - Faltantes: 0 (0%)
  - Média: 2001
  - Desvio Padrão: 4.61
  - Quartis: Min: 1999, 25%: 2003, 50%: 2007, 75%: 2011, Max: 2014

- **Deaths (Mortes)**
  - Valores únicos: 497
  - Válidos: 816 (100%)
  - Incompatíveis: 0 (0%)
  - Faltantes: 0 (0%)
  - Mais Comum: Suprimido (2%)

- **Population (População)**
  - Válidos: 816 (100%)
  - Incompatíveis: 0 (0%)
  - Faltantes: 0 (0%)
  - Média: 5.87 milhões
  - Desvio Padrão: 6.56 milhões
  - Quartis: Min: 492 mil, 25%: 1.58 milhões, 50%: 4.12 milhões, 75%: 6.60 milhões, Max: 38.8 milhões

- **Crude Rate (Taxa Bruta)**
  - Valores únicos: 164
  - Válidos: 816 (100%)
  - Incompatíveis: 0 (0%)
  - Faltantes: 0 (0%)
  - Mais Comum: Não Confiável (4%)

- **Crude Rate Lower 95% Confidence Interval (Intervalo de Confiança Inferior de 95% para a Taxa Bruta)**
  - Valores únicos: 151
  - Válidos: 816 (100%)
  - Incompatíveis: 0 (0%)
  - Faltantes: 0 (0%)
  - Mais Comum: 4.5 (2%)

- **Crude Rate Upper 95% Confidence Interval (Intervalo de Confiança Superior de 95% para a Taxa Bruta)**
  - Valores únicos: 172
  - Válidos: 816 (100%)
  - Incompatíveis: 0 (0%)
  - Faltantes: 0 (0%)
  - Mais Comum: 4.2 (2%)

- **Prescriptions Dispensed by US Retailers in that year (millions) (Prescrições Dispensadas por Varejistas nos EUA naquele ano, em milhões)**
  - Válidos: 816 (100%)
  - Incompatíveis: 0 (0%)
  - Faltantes: 0 (0%)

## Objetivos

- Identificar padrões e tendências nas mortes por overdose de opioides.
- Explorar a relação entre mortes por overdose, população, taxas de mortalidade e prescrições de opioides.
- Desenvolver visualizações interativas para facilitar a compreensão dos dados.

## Metodologia

Os dados foram processados e analisados utilizando a biblioteca Pandas para manipulação de dados e o Dash para visualizações interativas. A análise inclui a criação de novos campos, filtragem de dados e a geração de gráficos para identificar padrões e insights.

### Estrutura do Dataset

### Remoção das linhas com valores ausentes ou suprimidos

```python
import pandas as pd

# Lendo a base de dados utilizada.
data = pd.read_csv('Multiple Cause of Death 1999-2014 v1.1.csv', sep=',')

# Visualizar dataframe
data.head()

# Informações das colunas
data.info()

# Total de registros
data.count()

# Total de nulos
data.isnull().sum()

# Total de NaN
data.isna().sum()

# Total de dados duplicados
data.duplicated().sum()

# Remover linhas com valores ausentes ou suprimidos
drop_rows = data[(data['Deaths'] == 'Suppressed') | (data['Deaths'] == 'Unreliable') | 
                 (data['Crude Rate'] == 'Suppressed') | (data['Crude Rate'] == 'Unreliable')].index
data.drop(drop_rows, inplace=True)
```

### Análise Temporal das Mortes por Overdose de Opioides

```python
import matplotlib.pyplot as plt

pivot_data = data.pivot_table(index='Year', columns='State', values='Deaths', aggfunc='sum')
plt.figure(figsize=(14, 8))
pivot_data.plot(kind='bar', stacked=True, figsize=(14, 8))
plt.title('Total de Mortes por Estado e Ano')
plt.xlabel('Ano')
plt.ylabel('Total de Mortes')
plt.xticks(rotation=45)
plt.legend(loc='upper left', bbox_to_anchor=(1, 1))
plt.tight_layout()
plt.show()
```

### Análise de Correlação

```python
import seaborn as sns

# Selecionar as colunas relevantes para a correlação
correlation_data = data[['Deaths', 'Population', 'Crude Rate', 'Prescriptions Dispensed by US Retailers in that year (millions)']]

# Calcular a matriz de correlação
correlation_matrix = correlation_data.corr()

# Plotar o mapa de calor
plt.figure(figsize=(10, 8))
sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm', vmin=-1, vmax=1, linewidths=0.5, linecolor='black')
plt.title('Mapa de Calor das Correlações entre Variáveis')
plt.show()

```

## Conclusão
<p align=justify>A análise exploratória do conjunto de dados de mortes por overdose de opioides revelou importantes relações entre variáveis como mortes, população, taxa bruta e prescrições dispensadas por varejistas nos Estados Unidos. A visualização interativa fornecida pelo Dash oferece uma maneira eficiente de explorar essas correlações, permitindo uma melhor compreensão dos fatores que influenciam as mortes por overdose de opioides.</p>