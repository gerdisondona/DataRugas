# Projeto DataRugas - Guia de Utilização e Documentação

## 1. Visão Geral

Bem-vindo ao projeto **DataRugas**. Este projeto implementa uma solução completa de ponta a ponta no Microsoft Fabric, abrangendo engenharia de dados, treinamento de modelos de Machine Learning e visualização de dados interativa.

O objetivo principal é transformar dados brutos de vendas e despesas em insights acionáveis, disponibilizados através de um dashboard em Power BI. A solução é automatizada para garantir que os dados estejam sempre atualizados com mínima intervenção manual.

## 2. Arquitetura do Projeto

O projeto utiliza a **Arquitetura Medalhão (Medallion Architecture)**, uma abordagem padrão para organizar e refinar dados em camadas lógicas. O fluxo de dados pode ser visualizado da seguinte forma:

![Fluxo de Dados do Projeto](https://encurtador.com.br/7jOzj)

A arquitetura é dividida nas seguintes camadas:

* **Ingestão de Dados**: O processo inicial captura os dados brutos de fontes externas.
* **Camada Bronze**: Armazena os dados em seu estado original, brutos e sem tratamento. Serve como um registro histórico e ponto de partida para o processamento.
* **Camada Silver**: Os dados da camada Bronze são limpos, validados, padronizados e enriquecidos. Esta camada representa uma fonte de dados confiável e curada.
* **Camada Gold**: Os dados da camada Silver são agregados e transformados em modelos de dados otimizados para análise e consumo, geralmente em formato de fatos e dimensões. É a camada que alimenta diretamente os dashboards e os modelos de Machine Learning.
* **Serviço de ML e Visualização**: A camada final, onde os dados da camada Gold são utilizados para treinar modelos preditivos e para criar relatórios e dashboards interativos para o usuário final.

---

## 3. Como Utilizar o Dashboard

Para os stakeholders e usuários de negócio, a interação com o projeto é focada exclusivamente no consumo do dashboard final.

### 3.1. Acessando o Relatório

1.  Acesse o ambiente do Microsoft Fabric.
2.  No menu à esquerda, navegue até o Workspace **`DataRugas`**.
3.  Dentro do workspace, localize o **Report** (Relatório) com o nome **`BI_datarugas_marcelo_v2`**.
4.  Clique para abrir o relatório interativo. Você poderá navegar pelas páginas, usar os filtros e explorar os dados.

### 3.2. Atualização dos Dados

**O processo de atualização de dados foi projetado para ser automático.** Existe um pipeline de dados que executa todos os notebooks (`BronzeNotebook` → `SilverNotebook` → `GoldNotebook`) em sequência, processando os dados desde a fonte até a camada Gold e, por fim, atualizando o modelo semântico que alimenta o dashboard.

**O que isso significa para você?**
Você não precisa fazer nada para que os dados sejam atualizados. Basta acessar o relatório e os dados apresentados serão os mais recentes, conforme a frequência de atualização agendada.

#### E se uma atualização manual for necessária?

Em situações excepcionais, pode ser necessário forçar uma atualização completa do fluxo de dados. Para isso, siga os passos abaixo (geralmente executado por um membro da equipe de dados):

1.  No workspace `DataRugas`, localize o **Pipeline principal** do projeto (pode ter um nome como "Pipeline DataRugas" ou similar).
2.  Clique no ícone de **Executar (Run)** para iniciar o pipeline.
3.  Aguarde a conclusão de todas as etapas (Ingestão → Bronze → Silver → Gold).
4.  Após a conclusão do pipeline, navegue até o **Semantic model** (Modelo Semântico) com o nome **`BI_datarugas_marcelo_v2`** ou **`gold_bi`**.
5.  Clique no ícone **Atualizar agora (Refresh now)** para garantir que o Power BI carregue os dados mais recentes da camada Gold.

---

## 4. Componentes do Projeto

Abaixo está uma lista detalhada dos artefatos criados neste workspace e suas respectivas funções.

| Nome do Artefato | Tipo | Camada/Finalidade | Dono | Descrição |
| :--- | :--- | :--- | :--- | :--- |
| **Bronze** | Lakehouse | Dados bronze | Equipe3 | Armazena os dados brutos e não tratados. |
| `BronzeNotebook` | Notebook | Ingestão de dados | Equipe3 | Lê o arquivo Excel de origem e salva os dados na Lakehouse Bronze. |
| **Silver** | Lakehouse | Dados silver | Equipe3 | Armazena os dados limpos, validados e transformados. |
| `SilverNotebook` | Notebook | Processo inicial | Equipe3 | Lê os dados da camada Bronze, aplica regras de limpeza e validação e salva na Lakehouse Silver. |
| **gold** | Lakehouse | Dados ouro | Equipe3 | Armazena os dados agregados e prontos para análise. |
| `gold` | Notebook | Transformação | Equipe3 | Cria os modelos de dados finais (fatos/dimensões) a partir da camada Silver e salva na Lakehouse Gold. |
| `gold_bi` | Semantic model | Dados ouro | DataRugas | Modelo de dados que serve como ponte entre a Lakehouse Gold e os relatórios. |
| `ML Model for ...` | Notebook | Serviço de ML | Equipe3 | Notebooks para treinamento e experimentação de modelos de Machine Learning. |
| `ML-Model-for...` | Experiment | Serviço de ML | Equipe3 | Experimentos de ML para rastrear execuções e métricas dos modelos. |
| `BI_datarugas_...` | Report / Dashboard | Visualização | DataRugas | O relatório e dashboard final para consumo pelo stakeholder. |

---

## 5. Para Manutenção e Desenvolvimento Futuro

Para membros da equipe técnica que precisem dar manutenção ou evoluir o projeto:

* **Fonte de Dados**: O dado de origem principal é o arquivo `Base 2 - Vendas Clientes ConsigCar.xlsx`, localizado dentro da Lakehouse Bronze na seção `Files/Arquivos`. Para atualizar com uma nova versão, substitua este arquivo.
* **Lógica de Transformação**:
    * A lógica de **ingestão** está no `BronzeNotebook`.
    * As regras de **limpeza e validação** estão no `SilverNotebook`.
    * As **agregações e regras de negócio** para a camada analítica estão no `GoldNotebook`.
* **Modelos de Machine Learning**: Os notebooks prefixados com `ML Model` contêm o código para treinamento e inferência. Os resultados e execuções são rastreados nos `Experiments`.

### Pré-requisitos para Desenvolvimento

* Acesso de **Membro (Member)** ou **Contribuidor (Contributor)** ao workspace `DataRugas` no Microsoft Fabric.
* Conhecimento em PySpark (para os notebooks) e SQL.
* Licença de Power BI Pro ou Premium para editar e publicar os relatórios.


### Link do painel DataRugas
https://app.fabric.microsoft.com/links/novbJt7Btw?ctid=90f4b4cf-01dc-4528-abeb-ca174e50ab94&pbi_source=linkShare
