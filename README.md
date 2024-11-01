# Desafio Zandir

Este projeto contém uma série de análises e manipulações de dados usando Python e SQL. Também explora conceitos teóricos de Data Lake e visualizações gráficas para interpretação dos resultados. Certifique-se de possuir os arquivos `atendimento.csv` e `vendas.csv` no ambiente onde executará o código.

## Índice

- [Manipulação de Dados](#manipulação-de-dados)
  - [Valor total de vendas](#valor-total-de-vendas)
  - [Cliente com mais compras](#cliente-com-mais-compras)
  - [Atendimentos nos últimos 30 dias](#atendimentos-nos-últimos-30-dias)
  - [Motivo mais comum dos atendimentos](#motivo-mais-comum-dos-atendimentos)
- [Consultas SQL](#consultas-sql)
  - [Total de clientes](#total-de-clientes)
  - [Número de clientes por cidade](#número-de-clientes-por-cidade)
  - [Top 5 estados com mais clientes](#top-5-estados-com-mais-clientes)
- [Relatório Visual](#relatório-visual)
  - [Gráfico de atendimentos por motivo](#gráfico-de-atendimentos-por-motivo)
  - [Tendência de vendas](#tendência-de-vendas)
- [Data Lake](#data-lake)
  - [Conceito e Benefícios](#conceito-e-benefícios)

---

### Manipulação de Dados

Este notebook realiza análises nos dados de vendas e atendimentos ao cliente.

#### Valor total de vendas

Calcula o valor total de vendas com o código abaixo:

```python
import pandas as pd

vendas = pd.read_csv('vendas.csv')
total_vendas = vendas['valor_venda'].sum()
print(f'O valor total de todas as vendas foi de R${total_vendas}')
```

#### Cliente com mais compras

Identifica o cliente com o maior número de compras:

```python
# Exemplo de solução para calcular o cliente com mais compras
cliente_com_mais_compras = vendas['id_cliente'].value_counts().idxmax()
print(f'O cliente com mais compras é: {cliente_com_mais_compras}')
```

#### Atendimentos nos últimos 30 dias

Conta a quantidade de atendimentos realizados nos últimos 30 dias:

```python
atendimento = pd.read_csv('atendimento.csv')
total_atendimentos = atendimento['id_atendimento'].count()
print(f'Foram realizados {total_atendimentos} atendimentos nos últimos 30 dias.')
```

#### Motivo mais comum dos atendimentos

Determina o motivo mais frequente nos atendimentos:

```python
motivo_mais_comum = atendimento['motivo'].value_counts().idxmax()
print(f'O motivo mais comum dos atendimentos é: {motivo_mais_comum}')
```

---

### Consultas SQL

Executa consultas SQL baseadas na estrutura de um banco de dados de clientes.

#### Total de clientes

Retorna o total de clientes cadastrados:

```sql
SELECT COUNT(*) AS Total_clientes FROM clientes;
```

#### Número de clientes por cidade

Agrupa o número de clientes por cidade:

```sql
SELECT cidade, COUNT(*) AS numero_clientes FROM clientes GROUP BY cidade;
```

#### Top 5 estados com mais clientes

Lista os cinco estados com mais clientes cadastrados:

```sql
SELECT estado, COUNT(*) AS numero_clientes FROM clientes GROUP BY estado ORDER BY numero_clientes DESC LIMIT 5;
```

---

### Relatório Visual

#### Gráfico de atendimentos por motivo

Gera um gráfico de barras para representar o número de atendimentos por motivo:

```python
import matplotlib.pyplot as plt
import numpy as np

fonte = {
    'family': 'DejaVu Sans Mono', # fonte
    'color':  'black', # cor
    'weight': 'bold', # estilo - negrito
    'size': 15, # tamanho
}

x = atendimento['motivo'].groupby(atendimento['motivo']).count().index
y = atendimento['motivo'].groupby(atendimento['motivo']).count()

# alterando resolução para melhor exibição do eixo 'x' (motivos)
plt.figure(figsize=(10, 10))

# limitando o eixo 'y' (melhor visualização)
plt.ylim(0, 5)

# criando um gráfico de barras (eixo x, eixo y)
plt.bar(x, y)
# título
plt.title('Atendimentos por motivo', fontdict=fonte)
# nome do eixo 'x'
plt.xlabel('Motivo', fontdict=fonte)
# nome do eixo 'y'
plt.ylabel('Número de atendimentos', fontdict=fonte)
# gera o gráfico
plt.show()
```

#### Tendência de vendas

Gera um gráfico de linha mostrando a tendência de vendas dos últimos seis meses:

```python
import matplotlib.pyplot as plt
import numpy as np

fonte = {
    'family': 'DejaVu Sans Mono', # fonte
    'color':  'black', # cor
    'weight': 'bold', # estilo - negrito
    'size': 15, # tamanho
}

# Converter a coluna 'data_venda' para o tipo datetime
vendas['data_venda'] = pd.to_datetime(vendas['data_venda'])

# Extrair o nome do mês
vendas['mes'] = vendas['data_venda'].dt.strftime('%B')

# Agrupar por mês e contar o número de vendas
vendas_por_mes = vendas.groupby('mes').size().reindex([
    'January', 'February', 'March', 'April', 'May', 'June',
    'July', 'August', 'September', 'October', 'November', 'December'
], fill_value=0) # 'fill_value' preenche as entradas onde os dados são ausentes, no caso como apenas o mês de setembro possui entrada
# os demais vão aparecer mesmo sem receber valor algum, permitindo a exibição da 'tendência de vendas' de maneria mais clara

# Definir os eixos x (mês) e y (número de vendas)
x = vendas_por_mes.index
y = vendas_por_mes.values

# alterando resolução para melhor exibição do eixo 'x' (mês)
plt.figure(figsize=(12, 12))

plt.plot(x, y, marker='o')  # 'maker' adiciona marcadores nos pontos
# 'loc' posição no gráfico, 'x' e 'y' posição mais específica
plt.title('Vendas por mês', loc='left', y=0.96, x=0.01, fontdict=fonte)
plt.xlabel('Mês', fontdict=fonte)
plt.ylabel('Número de vendas', fontdict=fonte)

plt.show()
```

---

### Data Lake

#### Conceito e Benefícios

**Data Lake** é um repositório que permite armazenar grandes volumes de dados em seu formato original, estruturado ou não. As vantagens incluem:

- **Flexibilidade**: Possibilita armazenamento em qualquer formato, adaptando-se a várias finalidades.
- **Escalabilidade**: Suporta grandes volumes de dados, sendo ideal para empresas com necessidades de análise em larga escala.
- **Custo-benefício**: Reduz os custos de armazenamento e facilita o processamento de dados brutos.
