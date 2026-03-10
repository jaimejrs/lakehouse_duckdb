# 🏠 Modern Lakehouse — DuckDB + Apache Iceberg + MinIO + dbt

> Projeto baseado no **hands-on da comunidade [Data Engineer Help](https://www.youtube.com/@dataengineerhelp)**, adaptado com necessidades específicas e conhecimentos adquiridos ao longo da construção do lakehouse.

---

## 📖 Sobre o Projeto

Este repositório implementa um **Lakehouse moderno, 100% open source e totalmente containerizado**, demonstrando na prática conceitos avançados de engenharia de dados como:

- **Time Travel** — acesso a versões anteriores dos dados
- **Schema Evolution** — evolução de schema sem quebrar compatibilidade
- **ACID Transactions** — garantia de consistência dos dados
- **Modelagem dimensional** com **dbt** (staging → marts)
- **Análise exploratória** em **Jupyter Lab**

Todo o ambiente sobe com um único `docker compose up` e roda **100% local**, sem dependência de cloud ou licenças pagas.

---

## 🛠️ Tech Stack

| Camada | Tecnologia | Versão | Papel |
|--------|-----------|--------|-------|
| **Query Engine** | [DuckDB](https://duckdb.org/) | 0.10.0 | Motor analítico OLAP in-memory |
| **Table Format** | [Apache Iceberg](https://iceberg.apache.org/) (PyIceberg) | 0.6.0 | Formato de tabela com versionamento e time travel |
| **Object Storage** | [MinIO](https://min.io/) | latest | Armazenamento S3-compatible local |
| **Transformação** | [dbt-core](https://www.getdbt.com/) + dbt-duckdb | 1.7.0 | Transformações ELT e modelagem dimensional |
| **Notebooks** | [Jupyter Lab](https://jupyter.org/) | 4.0.9 | Ambiente interativo de análise e exploração |
| **Orquestração** | [Docker Compose](https://docs.docker.com/compose/) | 2.0+ | Orquestração de todos os serviços |
| **Linguagem** | [Python](https://www.python.org/) | 3.11 | Scripts de ingestão, geração de dados e queries |

### Bibliotecas Python Utilizadas

| Biblioteca | Finalidade |
|-----------|-----------|
| `pandas` 2.1.3 | Manipulação e análise de dados |
| `pyarrow` 14.0.1 | Serialização Parquet e integração Arrow |
| `faker` 20.1.0 | Geração de dados sintéticos realistas |
| `boto3` 1.34.0 | SDK AWS para interação com MinIO (S3) |
| `s3fs` / `fsspec` 2023.12.2 | Filesystem abstraction para S3 |
| `matplotlib` 3.8.2 | Visualizações estáticas |
| `seaborn` 0.13.0 | Visualizações estatísticas |
| `plotly` 5.18.0 | Visualizações interativas |

---

## 🏗️ Arquitetura

```
┌─────────────────────────────────────────────────────────────┐
│                      Docker Compose                         │
│                                                             │
│  ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌─────────┐   │
│  │  MinIO   │   │  DuckDB  │   │   dbt    │   │ Jupyter │   │
│  │ (S3)     │◄──│ (Engine) │◄──│ (ELT)   │    │  Lab    │   │
│  │ :9000    │   │          │   │          │   │ :8888   │   │
│  │ :9001    │   │          │   │          │   │         │   │
│  └──────────┘   └──────────┘   └──────────┘   └─────────┘   │
│       ▲              ▲              ▲              │        │
│       │              │              │              │        │
│       └──────────────┴──────────────┴──────────────┘        │
│                    Volume Compartilhado                     │
│                  (lakehouse.duckdb + data)                  │
└─────────────────────────────────────────────────────────────┘
```

### Fluxo de Dados

1. **Init Service** → Cria bucket no MinIO, gera 5.000 registros fake com Faker e cria tabela Iceberg
2. **MinIO** → Armazena dados em formato Iceberg (metadados + Parquet)
3. **DuckDB** → Consulta tabelas Iceberg diretamente via extensões `httpfs` e `iceberg`
4. **dbt** → Transforma dados brutos em modelos analíticos (staging → marts)
5. **Jupyter Lab** → Análise interativa e visualizações

---

## 🧊 Funcionalidades Iceberg

- **Time Travel**: Consulte versões anteriores dos dados via snapshots
- **Schema Evolution**: Adicione/remova colunas sem quebrar queries existentes
- **ACID Transactions**: Consistência garantida nas operações
- **Metadados Versionados**: Estrutura completa em `s3://lakehouse/iceberg/vendas_real/`

---

## 📌 Origem e Créditos

Este projeto é baseado no **hands-on promovido pela comunidade [Data Engineer Help](https://www.youtube.com/@dataengineerhelp)**, com adaptações para necessidades específicas de aprendizado e experimentação com o ecossistema moderno de dados.

- **Repositório original**: [aureliowozhiak/modern-lakehouse-duckdb-iceberg](https://github.com/aureliowozhiak/modern-lakehouse-duckdb-iceberg)
- **Comunidade**: [Data Engineer Help](https://www.youtube.com/@dataengineerhelp)

---

## 📄 Licença

Projeto open source para fins educacionais e de aprendizado.
