# utilizando_sqlite_localmente
Projeto completo de utilização do SQLite localmente com python no COLAB com operação SQL


# Documentação SQLAlchemy com Pandas no Google Colab

## 📌 Introdução
Este projeto demonstra como utilizar a biblioteca **SQLAlchemy** para manipulação de bancos de dados em memória com **SQLite**, integrado ao **Pandas** para realizar operações CRUD (Create, Read, Update, Delete). Tudo isso, de maneira prática e otimizada, diretamente no Google Colab.

---

## 🔧 1️⃣ Instalação das Dependências
Para utilizar o SQLAlchemy e o Pandas no Google Colab, execute o comando abaixo:

```bash
!pip install sqlalchemy pandas
```

---

## 📥 2️⃣ Importação das Bibliotecas
As bibliotecas necessárias para manipulação de dados e conexão com o banco de dados são:

```python
import sqlalchemy
from sqlalchemy import create_engine, MetaData, Table, inspect
import pandas as pd
```

---

## 🗄️ 3️⃣ Criação do Banco de Dados em Memória
A conexão será feita utilizando um banco de dados **SQLite** em memória, que é temporário e será perdido ao final da execução do script:

```python
engine = create_engine('sqlite:///:memory:')
```

> **Nota:** A conexão em memória é temporária e será perdida ao final da execução do script.

---

## 🌐 4️⃣ Leitura de Dados Externos
Utilizaremos um arquivo CSV hospedado em um repositório público para popular nosso banco de dados:

```python
url = 'https://raw.githubusercontent.com/alura-cursos/Pandas/main/clientes_banco.csv'
dados = pd.read_csv(url)
dados.head()
```

---

## 🗃️ 5️⃣ Criação da Tabela no Banco de Dados
O método `to_sql` permite criar a tabela e inserir os dados automaticamente:

```python
dados.to_sql('clientes', engine, index=False)
```

Para verificar as tabelas criadas, utilizamos o inspector:

```python
inspector = inspect(engine)
print(inspector.get_table_names())
```

---

## 🔍 6️⃣ Consultas SQL com Pandas
Podemos executar consultas SQL diretamente com Pandas para obter os dados:

```python
query = 'SELECT * FROM clientes'
pd.read_sql(query, engine)
```

---

## ✏️ 7️⃣ Atualização de Registros
Para atualizar um registro específico no banco, utilizamos o comando SQL `UPDATE`:

```python
from sqlalchemy import text

query = 'UPDATE clientes SET Rendimento_anual = 300000.0 WHERE ID_Cliente = 6840104'
with engine.connect() as conn:
    conn.execute(text(query))
    conn.commit()
```

> **Nota:** É necessário utilizar transações com `commit()` para salvar as alterações.

---

## 🗑️ 8️⃣ Exclusão de Registros
A exclusão de registros é feita com o comando SQL `DELETE`:

```python
query = 'DELETE FROM clientes WHERE ID_Cliente = 5008809'
with engine.connect() as conn:
    conn.execute(text(query))
    conn.commit()
```

---

## ➕ 9️⃣ Inserção de Novos Registros
Para inserir novos dados, utilizamos o comando SQL `INSERT INTO`:

```python
query = """
INSERT INTO clientes (
    ID_Cliente, Idade, Grau_escolaridade, Estado_civil,
    Tamanho_familia, Categoria_de_renda, Ocupacao, Anos_empregado,
    Rendimento_anual, Tem_carro, Moradia
) VALUES (
    6850985, 33, 'Doutorado', 'Solteiro', 1, 'Empregado', 
    'TI', 2, 290000, 0, 'Casa/apartamento próprio'
)
"""
with engine.connect() as conn:
    conn.execute(text(query))
    conn.commit()
```

---

## 👓 🔎 Visualização dos Dados Atualizados
Por fim, podemos visualizar todos os registros da tabela utilizando o Pandas:

```python
pd.read_sql_table('clientes', engine)
```

---

## ✅ Conclusão
Com **SQLAlchemy** e **Pandas**, conseguimos realizar manipulações completas em um banco de dados diretamente no Google Colab, facilitando a análise e persistência de dados em memória. Essa prática é essencial para **ETL**, **Data Science** e manipulação de grandes volumes de dados de forma simples e eficiente.

---


