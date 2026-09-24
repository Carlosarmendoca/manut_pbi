# 🛠️ CManut — Analytics de Manutenção Industrial

> Projeto de Data Analytics aplicado à manutenção industrial, utilizando dados sintéticos para explorar indicadores de confiabilidade, custos e gestão de estoque em diferentes ferramentas de Business Intelligence.

👉 **[Aceder ao Dashboard Interativo no Power BI](https://app.powerbi.com/view?r=eyJrIjoiMTY4M2Q2NjEtNDdjZC00NTViLTlmZGYtNTUzYzkxZDVmZGI4IiwidCI6ImI1MjVhODJiLTQzMjgtNDUzNC04NmRmLTgyYTk0NmQwODU0ZSJ9&navContentPaneEnabled=false)**

---

## 📌 Sobre o Projeto

O **CManut** foi desenvolvido para aplicar conceitos de **Engenharia de Manutenção, Confiabilidade, PCM e Data Analytics** em um cenário industrial simulado.

O projeto busca responder três questões principais:

* **Confiabilidade:** Os equipamentos estão disponíveis quando necessários?
* **Custos:** Onde estão concentrados os maiores gastos de manutenção?
* **Estoque:** Quais peças são mais utilizadas e quanto está sendo gasto com estoque?

O foco principal do projeto é o **dashboard desenvolvido em Power BI**, com indicadores, análises temporais, Pareto, comparação histórica e recursos de interatividade para apoiar a análise e a tomada de decisão.


---

# 📊 Dashboard Power BI

O relatório está estruturado em três páginas principais:

| Página                                 | Objetivo                                 | Principais análises                                                               |
| :------------------------------------- | :--------------------------------------- | :-------------------------------------------------------------------------------- |
| **Análise Temporal de Confiabilidade** | Avaliar a saúde operacional              | MTBF, MDT, Disponibilidade, Chamados Corretivos e análise por planta              |
| **Análise de Custos**                  | Avaliar impacto financeiro da manutenção | Custo de Peças, Custo Médio por OS, evolução mensal, YoY e Pareto de equipamentos |
| **Gestão de Peças e Estoque**          | Avaliar consumo e capital em estoque     | Consumo de Peças, Custo em Estoque, Top 10 e Classificação ABC                    |

### Principais recursos

* Navegação lateral retrátil utilizando **Bookmarks** e botões personalizados;
* Filtros integrados por **Localização, Categoria, Tipo de Serviço, Ano e Mês**;
* Tooltip analítico para detalhamento do **MDT por planta**;
* Comparações **YoY**;
* Pareto dinâmico de equipamentos;
* Classificação **ABC** de peças;
* Guia informativo com fórmulas dos principais indicadores e orientações de utilização.

---

# 🧮 Modelagem e DAX

O modelo foi estruturado seguindo uma abordagem de **fato e dimensão**, permitindo análises por planta, equipamento, peça, técnico, falha e período.

Entre as principais soluções desenvolvidas em DAX estão:

### Pareto Dinâmico

Utilização de `ALLSELECTED` para recalcular dinamicamente o custo acumulado dos equipamentos respeitando os filtros aplicados pelo usuário.

```dax
Custo Acumulado eqp = 
VAR Rankatual = [Rank Custo eqp]
RETURN
CALCULATE(
    '#MedidasCusto'[Custo Total OS Peças];
    FILTER(
        ALLSELECTED(DimEquipamento_normalized[NomeEquipamento]);
        [Rank Custo eqp] <= Rankatual
    )
)
```

### Comparativo YoY

Utilização de `SAMEPERIODLASTYEAR` para comparação histórica, com tratamento de ausência de base histórica.

```dax
Custo Estoque Variação vs LY Texto = 
VAR v = [% Custo total Estoque LY]
RETURN
IF(
    ISBLANK(v);
    "Sem base histórica";
    IF(
        v >= 0;
        "▲ " & FORMAT(v; "0.0%");
        "▼ " & FORMAT(v; "0.0%")
    )
)
```

### Valoração do Consumo de Estoque

Utilização de `SUMX` e `RELATED` para calcular o custo das saídas de estoque a partir da quantidade movimentada e do custo unitário da dimensão de peças.

---

# 🐍 Geração e Preparação dos Dados

Para viabilizar o projeto, foi desenvolvido um **dataset sintético de manutenção industrial em Python**, utilizando:

* Python
* Pandas
* NumPy
* Faker

O script gera dimensões e fatos relacionados a:

* Plantas;
* Equipamentos;
* Técnicos;
* Modos de falha;
* Peças;
* Chamados corretivos;
* Consumo de peças;
* Movimentações de estoque;
* Snapshot mensal de estoque.

O período simulado compreende **2019 a 2024**.

Foram utilizadas regras probabilísticas para representar uma redução dos chamados ao longo dos anos e uma redução de 25% nos tempos de atendimento e execução a partir de 2021, criando um cenário hipotético de evolução operacional.

Os dados também utilizam sementes fixas para permitir a reprodução do dataset.

---

# 🔄 Dados e Integração

A mesma base de dados foi utilizada em diferentes abordagens de BI.

### Power BI

Para o dashboard principal, as tabelas foram **exportadas para CSV** e posteriormente utilizadas como fonte de dados no Power BI.

### Google BigQuery

O script Python também possui integração com o **Google BigQuery**, permitindo armazenar e consultar as tabelas geradas.

### Looker Studio

A mesma base foi utilizada em uma segunda abordagem de visualização utilizando **Looker Studio**, permitindo explorar o mesmo contexto de manutenção sob outra ferramenta de BI.

Assim, o projeto não se limita à construção de um único dashboard, mas utiliza uma mesma estrutura de dados para explorar diferentes tecnologias de análise e visualização.

---

# 🤖 Uso de Inteligência Artificial

O desenvolvimento do código Python contou com **apoio de Inteligência Artificial**, utilizada principalmente para auxiliar na construção, exploração e evolução do script.

O código foi posteriormente analisado, testado e adaptado para atender às necessidades do projeto e às regras de negócio definidas.

Por se tratar de um **dataset sintético para fins de portfólio**, existem simplificações nas regras de geração e algumas características que não representariam necessariamente um ambiente produtivo real.

O objetivo do código não é reproduzir integralmente um sistema de manutenção industrial, mas fornecer uma base consistente para demonstrar:

**geração de dados → modelagem → análise → indicadores → visualização.**

---

# ⚠️ Limitações

Os dados utilizados são **100% sintéticos** e não representam uma empresa ou operação industrial real.

As regras de falhas, custos, estoque, consumo e evolução temporal foram criadas para fins de simulação.

Portanto, os indicadores apresentados devem ser interpretados como **resultados do cenário simulado**, e não como evidências de desempenho de uma operação real.

---

# 🎯 Objetivo do Projeto

O CManut representa a aplicação prática de conhecimentos de:

**Engenharia de Manutenção + Confiabilidade + PCM + Data Analytics + Business Intelligence**

Tecnologias utilizadas:

**Python • Pandas • NumPy • Faker • Google BigQuery • Power BI • DAX • Power Query • Looker Studio**

O projeto demonstra como dados de manutenção podem ser estruturados e transformados em informações para análise de **confiabilidade, custos, estoque e gestão de ativos**.
