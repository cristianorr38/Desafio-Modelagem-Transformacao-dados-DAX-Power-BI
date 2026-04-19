## 📊 Desafio DIO - Modelagem e Transformação de dados com DAX no Power BI

![PowerBI](https://img.shields.io/badge/PowerBI-integrado-yellow)
![Status](https://img.shields.io/badge/Status-Finalizado-blue)
![Build](https://img.shields.io/badge/build-success-success)
[![GitHub Repo Size](https://img.shields.io/github/repo-size/cristianorr38/Desafio-Modelagem-Transformacao-dados-DAX-Power-BI)](https://github.com/cristianorr38/Desafio-DIO-Relatorio-Gerencial-Vendas)
[![GitHub Stars](https://img.shields.io/github/stars/cristianorr38/Desafio-Modelagem-Transformacao-dados-DAX-Power-BI?style=social)](https://github.com/cristianorr38/Desafio-Modelagem-Transformacao-dados-DAX-Power-BI)
![GitHub last commit](https://img.shields.io/github/last-commit/cristianorr38/Desafio-Modelagem-Transformacao-dados-DAX-Power-BI)
![License](https://img.shields.io/badge/license-MIT-blue)

Este repositório contém a resolução do desafio de projeto focado na transformação de uma base de dados *flat* (tabela única) em um modelo **Star Schema**.  
O objetivo principal foi aplicar técnicas de **ETL** e **modelagem dimensional** para otimizar a performance e a usabilidade dos dados financeiros.

---

### 📌 Contexto do Desafio
A partir do dataset **Financial Sample**, foi necessário decompor a tabela única em tabelas de dimensão e uma tabela fato, garantindo que cada atributo descritivo fosse corretamente normalizado.

---

### 🏗️ Estrutura do Modelo Dimensional (Star Schema)

#### 1. Tabela Fato
**F_Vendas**: Contém as métricas de desempenho e as chaves estrangeiras.  
**Campos:** SK_ID, ID_Produto, Produto, Units Sold, Sales Price, Discount Band, Segment, Country, Sales, Profit, Date.

#### 2. Tabelas de Dimensão
- **D_Produtos**: Agrupamento de informações de vendas por produto.  
  *Métricas Calculadas:* Média de Unidades Vendidas, Média do Valor de Vendas, Mediana, Valor Máximo e Mínimo.

- **D_Produtos_Detalhes**: Detalhamento técnico dos produtos (Discount Band, Sale Price, Units Sold, Manufacturing Price).

- **D_Descontos**: Informações analíticas sobre descontos aplicados.

- **D_Detalhes**: Tabela complementar contendo atributos de vendas não contemplados nas outras dimensões.

- **D_Calendário**: Tabela dimensional de tempo criada via DAX.

#### 3. Tabela de Backup
**Financials_origem**: Mantida no modelo em modo oculto, servindo como base de segurança e *staging* para as demais transformações.

---

### 🛠️ Transformações e Funções DAX

#### Criação da Tabela de Calendário
Utilizei a função `CALENDAR()` para garantir uma dimensão temporal dinâmica e contínua:

```DAX
D_Calendar = 
VAR DataMinima = MIN(F_Vendas[Date])
VAR DataMaxima = MAX(F_Vendas[Date])
RETURN
ADDCOLUMNS (
    CALENDAR (DataMinima, DataMaxima),
    "Ano", YEAR([Date]),
    "Mês Num", MONTH([Date]),
    "Mês Nome", FORMAT([Date], "MMMM"),
    "Trimestre", "T" & FORMAT([Date], "Q"),
    "Dia da Semana", FORMAT([Date], "DDDD")
)
```

---

### 🛠️ Processos de ETL (Power Query)

- **Criação de Índices:** Implementação de índices condicionais para garantir a unicidade dos produtos.  
- **Agrupamento (Group By):** Utilizado na tabela D_Produtos para consolidar métricas estatísticas de vendas diretamente na carga dos dados.  
- **Condicionais:** Criação de colunas personalizadas para segmentação de índices de produtos.  
- **Reorganização:** Ordenação lógica das colunas para facilitar a leitura por ferramentas de visualização.  

---

📐  **Diagrama Dimensional (Star Schema)**

<img width="1917" height="861" alt="Diagrama_Dimensional_StarSchema_FinancialSample" src="https://github.com/user-attachments/assets/827d4dbc-2554-4d2e-b396-d6f927e71777" /><br>

![icon](https://github.com/user-attachments/assets/d37d097e-e2cc-47c4-9021-ac7c4dabb9ff) &nbsp; **Diagrama Mermaid**

```mermaid
erDiagram
    F_Vendas {
        int SK_ID
        int ID_Produto
        int ID_Categoria
        float Sales
        float Gross_Sales
        float Profit
        float Sale_Price
        int Units_Sold
        float Discounts
        date Date
    }

    D_Produtos {
        int ID_Produto
        string Produto
        float Media_Manufatura
        float Media_Unidades_Vendidas
        float Media_Valor_Vendas
        float Mediana_Valor_Vendas
        float Valor_Max_Venda
        float Valor_Min_Venda
    }

    D_Produto_Detalhes {
        int ID_Produto
        string Discount_Band
        float Manufacturing_Price
        float Sale_Price
        int Units_Sold
    }

    D_Descontos {
        string Discount_Band
        float Percentual
        int ID_Produto
    }

    D_Categorias {
        int ID_Categoria
        string Segment
        string Country
    }

    D_Detalhes {
        int ID_Detalhe
        int ID_Produto
        string Product
        float Sales
        float Profit
        float COGS
        string Month_Name
        int Month_Number
        int Year
    }

    D_Calendario {
        date Date
        int Ano
        int Mes_Num
        string Mes_Nome
        string Dia_Semana
        int Trimestre
        int Semestre
    }

    F_Vendas ||--o{ D_Produtos : "referencia"
    F_Vendas ||--o{ D_Produto_Detalhes : "referencia"
    F_Vendas ||--o{ D_Descontos : "referencia"
    F_Vendas ||--o{ D_Categorias : "referencia"
    F_Vendas ||--o{ D_Detalhes : "referencia"
    F_Vendas ||--o{ D_Calendario : "referencia"
```

---

### ✅ Progresso do projeto

Progresso atual:  
`███████████████████████` **100% Concluído**

---

### 🚀 Como Visualizar o Projeto

1. Clone este repositório.  
2. Abra o arquivo `.pbix` no **Power BI Desktop**.  
3. Observe a aba **Modelo** para visualizar o diagrama em estrela e as relações (1:N) entre as dimensões e a fato.  

---

### 📦 Release Notes - v1.0.0

#### 🎯 Título
Versão Final - Desafio-Modelagem-Transformacao-dados-DAX-Power-BI

#### 👨‍💻 Autor
**Cristiano**  
Analista de Sistemas focado em **Data Science** e **Engenharia de Dados**.

#### 📝 Descrição
Esta é a primeira versão oficial do projeto **Desafio DIO - Modelagem e Transformação de dados com DAX no Power BI**.  
O projeto foi desenvolvido utilizando **Power BI Desktop**, com documentação completa no GitHub.

---

#### 📜 Licença
Este projeto está licenciado sob a **MIT License**.  
Sinta-se livre para usar, modificar e compartilhar, mantendo os créditos ao autor.
