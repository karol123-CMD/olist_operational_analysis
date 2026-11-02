> Cópia em Markdown para leitura no GitHub, convertida de [formulas_kpis.docx](originais/formulas_kpis.docx). O Word original foi preservado; o texto e os valores foram transcritos, sem revisão ou reconciliação nesta etapa.

# DOCUMENTAÇÃO TÉCNICA E ANALÍTICA

**Formulas e KPIs**

**Projeto: Análise Estratégica de E-Commerce Olist**

Fonte de Dados: Brazilian E-Commerce Public Dataset by Olist (Kaggle)

Período dos Dados: Setembro/2016 – Agosto/2018

Ferramentas: Microsoft Excel, Power Query, Tabelas Dinâmicas

Total de Tabelas Documentadas: 15 tabelas (7 brutas + 3 auxiliares + 5 analíticas)

Classificação: Uso Interno / Portfólio Analítico

## KPIs e Fórmulas

### 1.1 Visão Geral da Arquitetura Métrica do Projeto

Esta seção documenta a arquitetura métrica do projeto, detalhando a lógica de negócio e as fórmulas aplicadas para transformar dados brutos em indicadores de performance (KPIs). Toda a estrutura foi consolidada em uma Base Tratada única, onde as tabelas de itens e pagamentos foram agregadas por pedido para garantir a integridade dos cálculos e evitar a duplicidade de valores.

### 1.2 Dicionário de Métricas e KPIs

#### 1. GMV (Gross Merchandise Volume)

Definição de Negócio: Valor total bruto transacionado na plataforma. Representa a receita total antes de cancelamentos ou taxas.

Fórmula: Soma de (Preço do Produto + Valor do Frete)

Base Utilizada: Base tratada (com agregação prévia de itens por pedido para evitar duplicidade).

Interpretação: Indicador fundamental de escala e tração financeira do e-commerce.

#### 2. Ticket Médio

Definição de Negócio: Valor médio gasto pelo cliente em cada pedido realizado.

Fórmula: GMV Total / Quantidade Total de Pedidos

Base Utilizada: Base tratada.

Interpretação: Reflete o perfil de consumo e ajuda a medir a eficiência de estratégias de aumento de cesta (upselling).

#### 3. Frete Médio

Definição de Negócio: Custo logístico médio repassado ao consumidor por transação.

Fórmula: Valor Total de Fretes / Pedidos com frete registrado

Base Utilizada: Base tratada.

Interpretação: Identifica disparidades geográficas e o peso do custo logístico na decisão de compra do cliente.

#### 4. Taxa de Entrega (Delivery Rate)

Definição de Negócio: Eficiência da operação em concluir o ciclo logístico até o destino final.

Fórmula: (Pedidos com Status "Entregue" / Total de Pedidos) * 100

Base Utilizada: Base de Pedidos (Orders).

Interpretação: Mede a saúde da última milha (last-mile) e a integridade do fluxo de transporte.

#### 5. SLA no Prazo (SLA Compliance)

Definição de Negócio: Percentual de entregas realizadas dentro ou antes da data prometida ao cliente.

Fórmula: (Pedidos Entregues no Prazo / Total de Pedidos Entregues) * 100

Base Utilizada: Base tratada (comparação entre data de entrega real e data estimada).

Interpretação: O principal KPI de confiabilidade. Impacta diretamente a satisfação e a nota de avaliação (Review Score).

#### 6. Taxa de Atraso (Delay Rate)

Definição de Negócio: Proporção de clientes que receberam seus produtos após o prazo estipulado.

Fórmula: (Pedidos Entregues com Atraso / Total de Pedidos Entregues) * 100

Filtros Aplicados: Considera apenas pedidos com o ciclo de entrega concluído.

Interpretação: Indica falhas de processo que geram detratores e aumentam o volume de chamados no suporte.

#### 7. Tempo Médio de Entrega (Lead Time)

Definição de Negócio: Tempo médio (em dias) que a operação leva para entregar o produto após a compra.

Fórmula: Média de (Data de Entrega Real - Data de Compra)

Base Utilizada: Base tratada (diferença entre os timestamps de compra e recebimento).

Interpretação: Avalia a agilidade logística total, desde o processamento do pedido até a entrega final.
