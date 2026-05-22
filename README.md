## Visão Geral do Projeto
A empresa fictícia Caramelo Pet enfrentava dificuldades para consolidar e analisar seus dados comerciais, pois as informações estavam distribuídas em diversos arquivos Excel separados por:
País | Cidade | Produtos | Categorias e subcategorias | Vendedores | Vendas

O gerente comercial, Augusto, precisava de uma solução centralizada para acompanhar os resultados da empresa de forma estratégica e visual.

## Objetivo do Projeto
Desenvolver um dashboard analítico no Power BI capaz de:

* Integrar múltiplas fontes de dados;
* Criar um modelo dimensional limpo e organizado;
* Gerar indicadores de vendas e lucratividade;
* Permitir análises temporais e geográficas;
* Identificar padrões, tendências e oportunidades de negócio;
* Apoiar tomadas de decisão estratégicas.

## Ferramentas Utilizadas
* Microsoft Power BI
* Power Query
* DAX
* Microsoft Excel

## Estrutura do Projeto (Power BI Project - .pbip)
Este projeto foi salvo no formato de projeto do Power BI (`.pbip`), permitindo o controle de versão de código e uma separação clara entre a camada de dados e de relatório.

A estrutura do repositório está organizada da seguinte forma:
* **`projeto.pbip`:** Arquivo principal que vincula o relatório ao dataset.
* **`projeto.SemanticModel`:** Pasta que contém a modelagem de dados, relações e o código **TMDL** (Tabular Model Definition Language) com as medidas DAX.
* **`projeto.Report`:** Pasta que armazena a estrutura visual do dashboard, configurações de páginas e layouts em formato JSON.

O projeto foi desenvolvido utilizando o modelo estrela (Star Schema), proporcionando melhor desempenho nas consultas, maior organização dos relacionamentos entre as tabelas, facilidade na construção das análises e maior escalabilidade para futuras expansões do modelo de dados.

## Processo de ETL e Tratamento dos Dados

O processo de preparação dos dados foi realizado utilizando o Power Query.

Etapas executadas:
### Importação dos arquivos

Foram importados múltiplos arquivos Excel contendo informações de vendas e tabelas dimensionais.

### Padronização dos dados

Foram realizados tratamentos como:

* Correção de tipos de dados;
* Padronização de nomes de colunas;
* Remoção de inconsistências;
* Ajustes em dados nulos;
* Criação de colunas auxiliares.
### Modelagem dimensional

O modelo foi estruturado utilizando tabelas fato e dimensão.

#### Tabela Fato
* f_vendas
#### Tabelas Dimensão
* d_produto
* d_categoria
* d_subcategoria
* d_pais
* d_cidade
* d_representanteVendas
* d_calendario
---

## Inteligência de Negócios & Métricas DAX

O projeto conta com uma tabela centralizada de medidas (`Medidas`) organizadas por pastas funcionais (*Vendas* e *Vendedores*), focadas em responder a perguntas estratégicas de negócio.

---

### Fórmulas Destacadas e Engenharia de Contexto

#### 1. Cálculo Iterativo de Receita e Custo (Contexto de Linha)
Como os valores unitários e custos padrão residem na dimensão `d_produto` e o volume transacional está na fato `f_vendas`, utilizei funções iterativas (`SUMX`) combinadas com a `RELATED` para calcular os totais dinamicamente sem inflar o tamanho do arquivo com colunas calculadas desnecessárias.

*   **Receita Bruta:**
    
```dax
    ReceitaBruta = 
    SUMX(
        f_vendas,
        f_vendas[Unidades] * RELATED(d_produto[PrecoVarejo])
    )
```

*   **Custo Total:**
    
```dax
    CustoTotal = 
    SUMX(
        f_vendas,
        f_vendas[Unidades] * RELATED(d_produto[CustoPadrao])
    )
```

#### 2. Análise de Market Share (% Partição)
Para calcular a representatividade de cada produto ou categoria no faturamento global, foi criada uma métrica utilizando a função `ALL()`, que remove as restrições dos filtros visuais para estabelecer uma base fixa de comparação.

*   **Percentual de Receita (% Share):**
    
```dax
    %Receita = 
    DIVIDE(
        [ReceitaBruta],
        CALCULATE([ReceitaBruta], ALL()),
        0
    )
```

#### 3. Média Móvel e Análise Temporal Mensal
Para suavizar distorções diárias e focar na tendência de faturamento de médio prazo, foi implementada a média calculada através dos meses ativos usando `AVERAGEX` e `VALUES`.

*   **Média Receita Mensal:**
    
```dax
    MediaReceitaMensal = 
    CALCULATE(
        AVERAGEX(
            VALUES(f_vendas[DataAnoMes]),
            [ReceitaBruta]
        )
    )
```

#### 4. Análise de Performance Comercial (Ranking Dinâmico)
Para o time de vendas, foi desenvolvida uma análise de performance baseada na receita gerada. Utilizou-se a função `RANKX` com o parâmetro `DENSE` para evitar saltos numéricos em caso de empate, integrada a uma regra de UI/UX baseada em `SWITCH` para exibição de indicadores visuais (KPIs com emojis) diretamente nas tabelas.

*   **Posição no Ranking:**
    
```dax
    RankingVendedor = 
    RANKX(
        ALLSELECTED(d_representanteVendas[NomeRepresentante]),
        [ReceitaBruta],
        ,
        DESC,
        DENSE
    )
```
*   **Indicador Visual (UI/UX):**
    
```dax
    IconeRanking = 
    SWITCH(
        [RankingVendedor],
        1, "🥇",
        2, "🥈",
        3, "🥉",   
        "•"
    )
```

---
## 📂 Como Explorar o Código no GitHub
Você pode analisar o desenvolvimento deste projeto diretamente pelo navegador:
* **Métricas e DAX:** Navegue até `projeto.SemanticModel/definition/tables/Medidas.tmdl` (ou na pasta de tabelas) para visualizar as regras de negócio escritas.
* **Consultas Power Query (M):** Os scripts de ETL (Extração, Transformação e Carga) podem ser encontrados dentro dos arquivos `.tmdl` de cada tabela na pasta `projeto.SemanticModel/definition/tables/`.

## Dashboard



