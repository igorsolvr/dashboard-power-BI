## 📌 Visão Geral do Projeto
Este dashboard foi desenvolvido para resolver [problema de negócio que o painel resolve - ex: a necessidade de monitorar o faturamento mensal e identificar gargalos na cadeia de suprimentos]. 

O objetivo principal é transformar dados brutos em insights acionáveis para apoiar a tomada de decisão da liderança de [Área - ex: Vendas/Marketing/Operações].

## 🛠️ Estrutura do Projeto (Power BI Project - .pbip)
Este projeto foi salvo no formato de projeto do Power BI (`.pbip`), permitindo o controle de versão de código e uma separação clara entre a camada de dados e de relatório.

A estrutura do repositório está organizada da seguinte forma:
* **`projeto.pbip`:** Arquivo principal que vincula o relatório ao dataset.
* **`projeto.SemanticModel`:** Pasta que contém a modelagem de dados, relações e o código **TMDL** (Tabular Model Definition Language) com as medidas DAX.
* **`projeto.Report`:** Pasta que armazena a estrutura visual do dashboard, configurações de páginas e layouts em formato JSON.

---

## 📂 Como Explorar o Código no GitHub
Você pode analisar o desenvolvimento deste projeto diretamente pelo navegador:
* **Métricas e DAX:** Navegue até `projeto.SemanticModel/definition/tables/Medidas.tmdl` (ou na pasta de tabelas) para visualizar as regras de negócio escritas.
* **Consultas Power Query (M):** Os scripts de ETL (Extração, Transformação e Carga) podem ser encontrados dentro dos arquivos `.tmdl` de cada tabela na pasta `projeto.SemanticModel/definition/tables/`.

