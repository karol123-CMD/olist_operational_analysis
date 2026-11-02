> Cópia em Markdown para leitura no GitHub, convertida de [dicionario_de_dados_olist.docx](originais/dicionario_de_dados_olist.docx). O Word original foi preservado; o texto e os valores foram transcritos, sem revisão ou reconciliação nesta etapa.

# DOCUMENTAÇÃO TÉCNICA E ANALÍTICA

**Dicionário de Dados**

**Projeto: Análise Estratégica de E-Commerce Olist**

Fonte de Dados: Brazilian E-Commerce Public Dataset by Olist (Kaggle)

Período dos Dados: Setembro/2016 – Agosto/2018

Ferramentas: Microsoft Excel, Power Query, Tabelas Dinâmicas

Total de Tabelas Documentadas: 15 tabelas (7 brutas + 3 auxiliares + 5 analíticas)

Classificação: Uso Interno / Portfólio Analítico

## SEÇÃO 1 - DICIONÁRIO DE DADOS

### 1.1 Visão Geral do Modelo de Dados

O projeto é baseado no Brazilian E-Commerce Public Dataset by Olist, um conjunto de dados públicos que representa dois anos de operação de um marketplace brasileiro. A base original é composta por sete tabelas relacionais que cobrem toda a jornada de compra: do cadastro do cliente até a avaliação pós-entrega. Para fins analíticos, foram criadas três tabelas auxiliares de agregação e uma base tratada consolidada que integra todas as dimensões em uma única linha por pedido.

### 1.2 Arquitetura em Camadas

<table>
<tr><td>Camada</td><td>Tabelas</td><td>Descrição</td></tr>
<tr><td>Bruta (Fonte)</td><td>base_costumers, base_orders, base_items, base_products, bases_payments, base_categories, base_reviews</td><td>Dados originais do dataset público Olist, sem transformações. Representam a fonte de verdade do projeto.</td></tr>
<tr><td>Auxiliar (Staging)</td><td>aux_forma_pagamento, aux_payments_agregados, aux_itens_agregados</td><td>Tabelas intermediárias criadas via Power Query/Tabelas Dinâmicas para deduplicar e agregar dados por pedido antes da consolidação final.</td></tr>
<tr><td>Tratada (Base Analítica)</td><td>base_tratada</td><td>Base consolidada: uma linha por pedido, com todas as dimensões integradas. É a principal tabela de análise do projeto.</td></tr>
<tr><td>Analítica (Output)</td><td>indicadores, estatística_descritiva, tabelas_dinâmicas, dashboard</td><td>Camada de saída: KPIs calculados, análise estatística descritiva, tabelas dinâmicas de resumo e painel gerencial (dashboard).</td></tr>
</table>

### 1.3 Relacionamentos entre Tabelas

<table>
<tr><td>Tabela Pai</td><td>Chave</td><td>Tabela Filho</td><td>Cardinalidade</td><td>Observação</td></tr>
<tr><td>base_costumers</td><td>customer_id</td><td>base_orders</td><td>1 : N</td><td>Um cliente pode ter vários pedidos (mas customer_id é único por pedido na base original).</td></tr>
<tr><td>base_orders</td><td>order_id</td><td>base_items</td><td>1 : N</td><td>Um pedido pode ter múltiplos itens (order_item_id sequencial).</td></tr>
<tr><td>base_orders</td><td>order_id</td><td>bases_payments</td><td>1 : N</td><td>Um pedido pode ter múltiplas linhas de pagamento (tipos diferentes).</td></tr>
<tr><td>base_orders</td><td>order_id</td><td>base_reviews</td><td>1 : 1</td><td>Geralmente uma avaliação por pedido.</td></tr>
<tr><td>base_items</td><td>product_id</td><td>base_products</td><td>N : 1</td><td>Múltiplos itens podem referenciar o mesmo produto.</td></tr>
<tr><td>base_products</td><td>product_category_name</td><td>base_categories</td><td>N : 1</td><td>Tradução do nome da categoria para inglês.</td></tr>
<tr><td>base_orders</td><td>order_id</td><td>aux_itens_agregados</td><td>1 : 1</td><td>Agregação de itens por pedido (staging).</td></tr>
<tr><td>base_orders</td><td>order_id</td><td>aux_payments_agregados</td><td>1 : 1</td><td>Agregação de pagamentos por pedido (staging).</td></tr>
<tr><td>base_orders</td><td>order_id</td><td>base_tratada</td><td>1 : 1</td><td>Consolidação final — um registro por pedido.</td></tr>
</table>

**1.4 Documentação das Tabelas — Camada 1: Dados Brutos (Fonte)**

As sete tabelas abaixo representam os dados originais do Brazilian E-Commerce Public Dataset by Olist, obtidos sem transformações do Kaggle. Constituem a fonte de verdade do projeto.

**Tabela 1 de 15 — base_costumers**

<table>
<tr><td colspan="7">T01 — base_costumers</td></tr>
<tr><td colspan="2">Descrição</td><td colspan="6">Contém as informações cadastrais dos clientes. Cada linha representa um cliente único no contexto de um pedido. O campo customer_unique_id permite rastrear o mesmo consumidor ao longo de múltiplos pedidos, enquanto customer_id é exclusivo por pedido.</td></tr>
<tr><td colspan="2">Tipo</td><td colspan="6">Tabela de dimensão - Cadastro de Clientes</td></tr>
<tr><td colspan="2">Qtd. Campos</td><td colspan="6">5</td></tr>
<tr><td colspan="2">Chave Primária</td><td colspan="6">customer_id</td></tr>
<tr><td colspan="2">Chave(s) Estrangeira(s)</td><td colspan="6">-</td></tr>
<tr><td>Campo</td><td colspan="2">Tipo</td><td>Exemplo</td><td>Origem</td><td>Regra de Cálculo</td><td colspan="2">Observações</td></tr>
<tr><td>customer_id</td><td colspan="2">Texto (Hash MD5)</td><td>06b8999e2fba1a1fbc88172c00ba8bc7</td><td>Olist Dataset</td><td>Gerado pelo sistema. Único por pedido.</td><td colspan="2">Chave primária desta tabela. Chave estrangeira em base_orders e base_tratada. Não identifica o consumidor de forma absoluta — use customer_unique_id para análise de recompra.</td></tr>
<tr><td>customer_unique_id</td><td colspan="2">Texto (Hash MD5)</td><td>861eff4711a542e4b93843c6dd7febb0</td><td>Olist Dataset</td><td>Identificador permanente do consumidor. Um mesmo customer_unique_id pode ter vários customer_id.</td><td colspan="2">Permite análise de recompra e comportamento longitudinal do cliente. Útil para segmentação RFM.</td></tr>
<tr><td>customer_zip_code_prefix</td><td colspan="2">Texto Numérico (5 dígitos)</td><td>14409</td><td>Olist Dataset</td><td>Prefixo do CEP do endereço de entrega.</td><td colspan="2">Pode ser usado para análise geográfica com maior granularidade que UF. Atenção: pode haver CEPs desatualizados ou inválidos.</td></tr>
<tr><td>customer_city</td><td colspan="2">Texto</td><td>franca</td><td>Olist Dataset</td><td>Nome da cidade em letras minúsculas.</td><td colspan="2">Nomes não normalizados (ex: variações de acentuação). Usar com cautela em agrupamentos. Combinado com customer_state para evitar homonímias.</td></tr>
<tr><td>customer_state</td><td colspan="2">Texto (Sigla UF)</td><td>SP</td><td>Olist Dataset</td><td>Sigla do estado brasileiro (2 letras).</td><td colspan="2">Usado extensivamente nas análises regionais. Presente na base_tratada como uf_cliente. Sempre preenchido.</td></tr>
</table>

**Tabela 2 de 15 — base_orders**

<table>
<tr><td colspan="2">T02 — base_orders</td></tr>
<tr><td>Descrição</td><td>Tabela central do modelo. Registra o ciclo de vida de cada pedido, desde a compra até a entrega, com todas as datas relevantes. É a tabela de maior importância analítica: dela derivam os indicadores de SLA, tempo de entrega e status operacional.</td></tr>
<tr><td>Tipo</td><td>Tabela de fatos — Pedidos</td></tr>
<tr><td>Qtd. Campos</td><td>8</td></tr>
<tr><td>Chave Primária</td><td>order_id</td></tr>
<tr><td>Chave(s) Estrangeira(s)</td><td>customer_id → base_costumers</td></tr>
</table>

Atenção: as colunas de data estão armazenadas como números seriais do Excel (ex: 43010.45 = 09/10/2017 10:48). A conversão para data/hora legível foi realizada na base_tratada.

<table>
<tr><td colspan="2">Campo</td><td>Tipo</td><td>Exemplo</td><td>Origem</td><td>Regra de Cálculo</td><td>Observações</td></tr>
<tr><td>order_id</td><td colspan="2">Texto (Hash MD5)</td><td>e481f51cbdc54678b7cc49136f2d6af7</td><td>Olist Dataset</td><td>Gerado pelo sistema. Único por pedido.</td><td>Chave primária. Chave estrangeira em todas as demais tabelas operacionais. Sempre preenchido.</td></tr>
<tr><td>customer_id</td><td colspan="2">Texto (Hash MD5)</td><td>9ef432eb6251297304e76186b10a928d</td><td>Olist Dataset</td><td>Referência ao cliente do pedido.</td><td>Chave estrangeira para base_costumers. Sempre preenchido.</td></tr>
<tr><td>order_status</td><td colspan="2">Texto (Categórico)</td><td>delivered</td><td>Olist Dataset</td><td>Status atual do pedido no ciclo logístico.</td><td>Valores possíveis: delivered, shipped, canceled, invoiced, processing, created, approved, unavailable. Análise usa "delivered" como filtro principal. Pedidos não entregues = status diferente de "delivered".</td></tr>
<tr><td>order_purchase_timestamp</td><td colspan="2">Data/Hora (Serial Excel)</td><td>43010.455 (09/10/2017 10:55)</td><td>Olist Dataset</td><td>Momento exato em que o cliente finalizou a compra.</td><td>Ponto de partida para cálculo de prazo real e estimado. Presente na base_tratada como data_compra.</td></tr>
<tr><td>order_approved_at</td><td colspan="2">Data/Hora (Serial Excel)</td><td>43010.463 (09/10/2017 11:07)</td><td>Olist Dataset</td><td>Momento da aprovação do pagamento.</td><td>Pode ser nulo em pedidos cancelados ou não aprovados. Presente na base_tratada como data_aprovacao.</td></tr>
<tr><td>order_delivered_carrier_date</td><td colspan="2">Data/Hora (Serial Excel)</td><td>43012.829 (11/10/2017 19:54)</td><td>Olist Dataset</td><td>Data em que o pedido foi entregue à transportadora.</td><td>Presente na base_tratada como data_despacho. Pode ser nulo em pedidos não despachados.</td></tr>
<tr><td>order_delivered_customer_date</td><td colspan="2">Data/Hora (Serial Excel)</td><td>43018.892 (17/10/2017 21:25)</td><td>Olist Dataset</td><td>Data em que o cliente efetivamente recebeu o pedido.</td><td>Presente na base_tratada como data_entrega_cliente. Nulo em pedidos não entregues (2.963 casos). Usado para calcular prazo_entrega_dias e status_sla.</td></tr>
<tr><td>order_estimated_delivery_date</td><td colspan="2">Data (Serial Excel)</td><td>43026 (25/10/2017)</td><td>Olist Dataset</td><td>Data de entrega prometida ao cliente no momento da compra.</td><td>Presente na base_tratada como data_entrega_estimada. Usado para calcular atraso_dias = data_entrega_cliente - data_entrega_estimada.</td></tr>
</table>

**Tabela 3 de 15 — base_items**

<table>
<tr><td colspan="7">T03 — base_items</td></tr>
<tr><td colspan="2">Descrição</td><td colspan="5">Detalha os itens que compõem cada pedido. Um único pedido pode conter múltiplos itens (múltiplas linhas). Registra preço, frete e prazo de despacho por item. Esta granularidade exige agregação antes de uso na base_tratada para evitar duplicidade de pedidos.</td></tr>
<tr><td colspan="2">Tipo</td><td colspan="5">Tabela de fatos — Itens do Pedido</td></tr>
<tr><td colspan="2">Qtd. Campos</td><td colspan="5">7</td></tr>
<tr><td colspan="2">Chave Primária</td><td colspan="5">order_id + order_item_id (chave composta)</td></tr>
<tr><td colspan="2">Chave(s) Estrangeira(s)</td><td colspan="5">order_id → base_orders | product_id → base_products | seller_id (vendedor)</td></tr>
<tr><td>Campo</td><td colspan="2">Tipo</td><td>Exemplo</td><td>Origem</td><td>Regra de Cálculo</td><td>Observações</td></tr>
<tr><td>order_id</td><td colspan="2">Texto (Hash MD5)</td><td>00010242fe8c5a6d1ba2dd792cb16214</td><td>Olist Dataset</td><td>Referência ao pedido ao qual o item pertence.</td><td>Chave estrangeira para base_orders. Repetido para pedidos com mais de um item.</td></tr>
<tr><td>order_item_id</td><td colspan="2">Inteiro</td><td>1</td><td>Olist Dataset</td><td>Sequencial numérico do item dentro do pedido. Começa em 1.</td><td>Junto com order_id forma a chave composta desta tabela. Pedidos com order_item_id &gt; 1 são pedidos multi-item.</td></tr>
<tr><td>product_id</td><td colspan="2">Texto (Hash MD5)</td><td>4244733e06e7ecb4970a6e2683c13e61</td><td>Olist Dataset</td><td>Identificador do produto vendido.</td><td>Chave estrangeira para base_products. Permite enriquecer com categoria, peso e dimensões.</td></tr>
<tr><td>seller_id</td><td colspan="2">Texto (Hash MD5)</td><td>48436dade18ac8b2bce089ec2a041202</td><td>Olist Dataset</td><td>Identificador do vendedor responsável pelo item.</td><td>Não há tabela de vendedores neste projeto. Campo preservado para expansão futura da análise.</td></tr>
<tr><td>shipping_limit_date</td><td colspan="2">Data/Hora (Serial Excel)</td><td>42997.406 (27/09/2017)</td><td>Olist Dataset</td><td>Prazo limite para o vendedor despachar o item.</td><td>SLA interno entre Olist e vendedor. Não utilizado diretamente na base_tratada, mas relevante para análise de responsabilidade por atrasos.</td></tr>
<tr><td>price</td><td colspan="2">Decimal (R$)</td><td>58.90</td><td>Olist Dataset</td><td>Preço unitário do item.</td><td>Deve ser somado por order_id para obter valor_produtos na base_tratada. Feito via aux_itens_agregados. Valores em Reais (BRL).</td></tr>
<tr><td>freight_value</td><td colspan="2">Decimal (R$)</td><td>13.29</td><td>Olist Dataset</td><td>Valor do frete cobrado para aquele item.</td><td>Deve ser somado por order_id para obter valor_frete na base_tratada. Feito via aux_itens_agregados. O frete total do pedido = soma de todos os itens.</td></tr>
</table>

**Tabela 4 de 15 — base_products**

<table>
<tr><td colspan="7">T04 — base_products</td></tr>
<tr><td colspan="2">Descrição</td><td colspan="6">Contém o catálogo de produtos com atributos descritivos e logísticos. Permite categorização dos itens vendidos e cálculo de peso/dimensão para análise de frete. O campo category_name é em português, traduzido via base_categories.</td></tr>
<tr><td colspan="2">Tipo</td><td colspan="6">Tabela de dimensão — Produtos</td></tr>
<tr><td colspan="2">Qtd. Campos</td><td colspan="6">9</td></tr>
<tr><td colspan="2">Chave Primária</td><td colspan="6">product_id</td></tr>
<tr><td colspan="2">Chave(s) Estrangeira(s)</td><td colspan="6">product_category_name → base_categories</td></tr>
<tr><td>Campo</td><td colspan="2">Tipo</td><td>Exemplo</td><td>Origem</td><td>Regra de Cálculo</td><td colspan="2">Observações</td></tr>
<tr><td>product_id</td><td colspan="2">Texto (Hash MD5)</td><td>1e9e8ef04dbcff4541ed26657ea517e5</td><td>Olist Dataset</td><td>Identificador único do produto.</td><td colspan="2">Chave primária. Referenciado por base_items.</td></tr>
<tr><td>product_category_name</td><td colspan="2">Texto</td><td>perfumaria</td><td>Olist Dataset</td><td>Nome da categoria em português (formato snake_case).</td><td colspan="2">Chave estrangeira para base_categories. Pode ser nulo para produtos sem categoria definida.</td></tr>
<tr><td>product_name_lenght</td><td colspan="2">Inteiro</td><td>40</td><td>Olist Dataset</td><td>Número de caracteres do nome do produto.</td><td colspan="2">Dado anonimizado: o nome real não é exposto, apenas o tamanho.</td></tr>
<tr><td>product_description_lenght</td><td colspan="2">Inteiro</td><td>287</td><td>Olist Dataset</td><td>Número de caracteres da descrição do produto.</td><td colspan="2">Proxy de riqueza da listagem. Útil em análises de qualidade de catálogo.</td></tr>
<tr><td>product_photos_qty</td><td colspan="2">Inteiro</td><td>1</td><td>Olist Dataset</td><td>Quantidade de fotos disponíveis na listagem.</td><td colspan="2">Influencia a taxa de conversão. Não utilizado diretamente nos KPIs deste projeto.</td></tr>
<tr><td>product_weight_g</td><td colspan="2">Inteiro (gramas)</td><td>225</td><td>Olist Dataset</td><td>Peso do produto em gramas.</td><td colspan="2">Correlacionado com o custo de frete. Pode ter nulos em produtos sem ficha técnica completa.</td></tr>
<tr><td>product_length_cm</td><td colspan="2">Inteiro (cm)</td><td>16</td><td>Olist Dataset</td><td>Comprimento do produto em centímetros.</td><td colspan="2">Junto com altura e largura define o volume para cálculo de frete cubado.</td></tr>
<tr><td>product_height_cm</td><td colspan="2">Inteiro (cm)</td><td>10</td><td>Olist Dataset</td><td>Altura do produto em centímetros.</td><td colspan="2">Pode ser nulo. Vide observação de product_length_cm.</td></tr>
<tr><td>product_width_cm</td><td colspan="2">Inteiro (cm)</td><td>14</td><td>Olist Dataset</td><td>Largura do produto em centímetros.</td><td colspan="2">Pode ser nulo. Vide observação de product_length_cm.</td></tr>
</table>

**Tabela 5 de 15 — bases_payments**

<table>
<tr><td colspan="7">T05 — bases_payments</td></tr>
<tr><td colspan="2">Descrição</td><td colspan="6">Registra os pagamentos realizados por pedido. Um único pedido pode ter múltiplas linhas de pagamento (ex: cartão + voucher). Por isso, antes da integração na base_tratada, os pagamentos são agregados por pedido em aux_payments_agregados.</td></tr>
<tr><td colspan="2">Tipo</td><td colspan="6">Tabela de fatos — Pagamentos</td></tr>
<tr><td colspan="2">Qtd. Campos</td><td colspan="6">5</td></tr>
<tr><td colspan="2">Chave Primária</td><td colspan="6">order_id + payment_sequential (chave composta)</td></tr>
<tr><td colspan="2">Chave(s) Estrangeira(s)</td><td colspan="6">order_id → base_orders</td></tr>
<tr><td colspan="2"></td><td colspan="6"></td></tr>
<tr><td>Campo</td><td colspan="2">Tipo</td><td>-Exemplo</td><td>Origem</td><td>Regra de Cálculo</td><td colspan="2">Observações</td></tr>
<tr><td>order_id</td><td colspan="2">Texto (Hash MD5)</td><td>b81ef226f3fe1789b1e8b2acac839d17</td><td>Olist Dataset</td><td>Referência ao pedido.</td><td colspan="2">Chave estrangeira para base_orders. Pode aparecer múltiplas vezes quando o pagamento usa mais de um método.</td></tr>
<tr><td>payment_sequential</td><td colspan="2">Inteiro</td><td>1</td><td>Olist Dataset</td><td>Número sequencial do tipo de pagamento dentro do pedido. Começa em 1.</td><td colspan="2">Junto com order_id forma a chave composta. Pedidos com payment_sequential &gt; 1 usaram múltiplos meios de pagamento.</td></tr>
<tr><td>payment_type</td><td colspan="2">Texto (Categórico)</td><td>credit_card</td><td>Olist Dataset</td><td>Tipo de meio de pagamento utilizado.</td><td colspan="2">Valores: credit_card, boleto, voucher, debit_card, not_defined. Usado na análise de distribuição de formas de pagamento. "not_defined" representa 3 pedidos na base.</td></tr>
<tr><td>payment_installments</td><td colspan="2">Inteiro</td><td>8</td><td>Olist Dataset</td><td>Número de parcelas escolhidas pelo cliente.</td><td colspan="2">Valor 1 = pagamento à vista (inclusive boleto e débito). Usado para calcular campo parcelas na base_tratada via MAX por pedido.</td></tr>
<tr><td>payment_value</td><td colspan="2">Decimal (R$)</td><td>99.33</td><td>Olist Dataset</td><td>Valor pago nesta linha de pagamento.</td><td colspan="2">Deve ser somado por order_id para obter valor_pagamento total do pedido. Inclui preço do produto + frete + eventuais taxas/descontos via voucher. Pode diferir de valor_total_pedido por uso de crédito.</td></tr>
</table>

**Tabela 6 de 15 — base_categories**

<table>
<tr><td colspan="7">T06 — base_categories</td></tr>
<tr><td colspan="2">Descrição</td><td colspan="6">Tabela de tradução entre os nomes de categoria em português (padrão da base Olist) e seus equivalentes em inglês. Usada como de-para para internacionalização e padronização de rótulos em análises.</td></tr>
<tr><td colspan="2">Tipo</td><td colspan="6">Tabela de dimensão — Categorias de Produto</td></tr>
<tr><td colspan="2">Qtd. Campos</td><td colspan="6">2</td></tr>
<tr><td colspan="2">Chave Primária</td><td colspan="6">product_category_name</td></tr>
<tr><td colspan="2">Chave(s) Estrangeira(s)</td><td colspan="6">product_category_name referenciada por base_products</td></tr>
<tr><td>Campo</td><td colspan="2">Tipo</td><td>Exemplo</td><td>Origem</td><td>Regra de Cálculo</td><td colspan="2">Observações</td></tr>
<tr><td>product_category_name</td><td colspan="2">Texto</td><td>beleza_saude</td><td>Olist Dataset</td><td>Nome original da categoria em português (snake_case).</td><td colspan="2">Chave primária. Deve coincidir com o campo homônimo em base_products para JOIN.</td></tr>
<tr><td>Product_category_name_english</td><td colspan="2">Texto</td><td>health_beauty</td><td>Olist Dataset</td><td>Tradução do nome da categoria para inglês.</td><td colspan="2">Usado para relatórios internacionalizados. Nem todas as categorias têm tradução disponível.</td></tr>
</table>

**Tabela 7 de 15 — base_reviews**

<table>
<tr><td colspan="8">T07 — base_reviews</td></tr>
<tr><td colspan="2">Descrição</td><td colspan="6">Armazena as avaliações dos clientes após a entrega. Cada registro representa o feedback de um pedido com nota de 1 a 5 estrelas, título opcional e comentário. É a base para o indicador de satisfação do cliente (CSAT).</td></tr>
<tr><td colspan="2">Tipo</td><td colspan="6">Tabela de fatos — Avaliações dos Clientes</td></tr>
<tr><td colspan="2">Qtd. Campos</td><td colspan="6">7</td></tr>
<tr><td colspan="2">Chave Primária</td><td colspan="6">review_id</td></tr>
<tr><td colspan="2">Chave(s) Estrangeira(s)</td><td colspan="6">order_id → base_orders</td></tr>
<tr><td>Campo</td><td colspan="2">Tipo</td><td>Exemplo</td><td>Origem</td><td>Regra de Cálculo</td><td colspan="2">Observações</td></tr>
<tr><td>review_id</td><td colspan="2">Texto (Hash MD5)</td><td>7bc2406110b926393aa56f80a40eba40</td><td>Olist Dataset</td><td>Identificador único da avaliação.</td><td colspan="2">Chave primária. Podem existir múltiplas avaliações para um mesmo pedido em casos de contestação.</td></tr>
<tr><td>order_id</td><td colspan="2">Texto (Hash MD5)</td><td>73fc7af87114b39712e6da79b0a377eb</td><td>Olist Dataset</td><td>Referência ao pedido avaliado.</td><td colspan="2">Chave estrangeira para base_orders.</td></tr>
<tr><td>review_score</td><td colspan="2">Inteiro (1–5)</td><td>4</td><td>Olist Dataset</td><td>Nota atribuída pelo cliente: 1 (péssimo) a 5 (excelente).</td><td colspan="2">Indicador central de satisfação. KPI "Avaliação média = 4,09". Análise revela 57,8% nota 5 e 14,7% notas 1–2. Nunca nulo neste dataset.</td></tr>
<tr><td>review_comment_title</td><td colspan="2">Texto</td><td>recomendo</td><td>Olist Dataset</td><td>Título curto do comentário, fornecido opcionalmente.</td><td colspan="2">Alta taxa de nulos: a maioria dos clientes não preenche o título. Não utilizado diretamente nos KPIs.</td></tr>
<tr><td>review_comment_message</td><td colspan="2">Texto Livre</td><td>Recebi bem antes do prazo estipulado.</td><td>Olist Dataset</td><td>Texto completo do comentário deixado pelo cliente.</td><td colspan="2">Alta taxa de nulos. Potencial para análise de texto (NLP) em fases futuras. Mensagens em português.</td></tr>
<tr><td>review_creation_date</td><td colspan="2">Data (Serial Excel)</td><td>43118 (28/01/2018)</td><td>Olist Dataset</td><td>Data em que o sistema enviou a pesquisa de satisfação ao cliente.</td><td colspan="2">Geralmente alguns dias após a entrega confirmada.</td></tr>
<tr><td>review_answer_timestamp</td><td colspan="2">Data/Hora (Serial Excel)</td><td>43118.907 (28/01/2018 21:46)</td><td>Olist Dataset</td><td>Momento exato em que o cliente respondeu à pesquisa.</td><td colspan="2">Combinado com review_creation_date permite calcular o tempo de resposta do cliente.</td></tr>
</table>

### 1.5 Documentação das Tabelas — Camada 2: Tabelas Auxiliares (Staging)

As tabelas auxiliares foram criadas no Excel/Power Query como etapa intermediária de transformação. Seu objetivo é resolver problemas de granularidade das fontes brutas antes da consolidação na base_tratada: a base_items e bases_payments têm múltiplas linhas por pedido, o que geraria duplicidade se usadas diretamente.

**Tabela 8 de 15 — aux_forma_pagamento**

<table>
<tr><td colspan="8">T08 — aux_forma_pagamento</td></tr>
<tr><td colspan="2">Descrição</td><td colspan="6">Identifica a forma de pagamento principal de cada pedido (a de maior valor ou a de sequencial 1). Resolve a multivalorização da bases_payments, gerando um único rótulo por pedido para uso na base_tratada.</td></tr>
<tr><td colspan="2">Tipo</td><td colspan="6">Tabela auxiliar de staging — Forma de Pagamento Principal</td></tr>
<tr><td colspan="2">Qtd. Campos</td><td colspan="6">2</td></tr>
<tr><td colspan="2">Chave Primária</td><td colspan="6">order_id</td></tr>
<tr><td colspan="2">Chave(s) Estrangeira(s)</td><td colspan="6">order_id → base_orders</td></tr>
<tr><td>Campo</td><td colspan="2">Tipo</td><td>Exemplo</td><td>Origem</td><td>Regra de Cálculo</td><td colspan="2">Observações</td></tr>
<tr><td>order_id</td><td colspan="2">Texto (Hash MD5)</td><td>00010242fe8c5a6d1ba2dd792cb16214</td><td>Power Query / Transformação</td><td>Referência ao pedido. Um registro por pedido.</td><td colspan="2">Chave primária desta tabela auxiliar. Garante granularidade 1:1 com base_orders.</td></tr>
<tr><td>forma_pagamento</td><td colspan="2">Texto (Categórico)</td><td>credit_card</td><td>Derivado de bases_payments</td><td>Forma de pagamento dominante do pedido. Determinada com base no payment_type de maior valor ou por MAX/FIRST aplicado por order_id.</td><td colspan="2">Valores possíveis: credit_card, boleto, voucher, debit_card, not_defined. Presente na base_tratada como forma_pagamento_principal.</td></tr>
</table>

**Tabela 9 de 15 — aux_payments_agregados**

<table>
<tr><td colspan="8">T09 — aux_payments_agregados</td></tr>
<tr><td colspan="2">Descrição</td><td colspan="6">Agrega os dados de pagamento por pedido, pivotando as formas de pagamento em colunas separadas. Resolve a multivalorização da bases_payments garantindo uma linha por pedido com os valores desagregados por canal.</td></tr>
<tr><td colspan="2">Tipo</td><td colspan="6">Tabela auxiliar de staging — Pagamentos Agregados por Pedido</td></tr>
<tr><td colspan="2">Qtd. Campos</td><td colspan="6">8</td></tr>
<tr><td colspan="2">Chave Primária</td><td colspan="6">order_id</td></tr>
<tr><td colspan="2">Chave(s) Estrangeira(s)</td><td colspan="6">order_id → base_orders</td></tr>
<tr><td>Campo</td><td colspan="2">Tipo</td><td>Exemplo</td><td>Origem</td><td>Regra de Cálculo</td><td colspan="2">Observações</td></tr>
<tr><td>order_id</td><td colspan="2">Texto (Hash MD5)</td><td>00010242fe8c5a6d1ba2dd792cb16214</td><td>Derivado de bases_payments</td><td>Identificador do pedido. Um registro por pedido após agregação.</td><td colspan="2">Chave primária desta tabela. Chave estrangeira para base_orders.</td></tr>
<tr><td>credit_card</td><td colspan="2">Decimal (R$)</td><td>72.19</td><td>SUMIF de bases_payments</td><td>Soma dos valores pagos com cartão de crédito neste pedido. Zero se não utilizado.</td><td colspan="2">Pivot do payment_type=credit_card. Presente na base_tratada como campo homônimo.</td></tr>
<tr><td>boleto</td><td colspan="2">Decimal (R$)</td><td>0</td><td>SUMIF de bases_payments</td><td>Soma dos valores pagos com boleto bancário. Zero se não utilizado.</td><td colspan="2">Pivot do payment_type=boleto. Presente na base_tratada como campo homônimo.</td></tr>
<tr><td>voucher</td><td colspan="2">Decimal (R$)</td><td>20.59</td><td>SUMIF de bases_payments</td><td>Soma dos valores cobertos por voucher/crédito. Zero se não utilizado.</td><td colspan="2">Pivot do payment_type=voucher. Presente na base_tratada. Justifica diferença entre GMV e receita de pagamentos.</td></tr>
<tr><td>debit_card</td><td colspan="2">Decimal (R$)</td><td>0</td><td>SUMIF de bases_payments</td><td>Soma dos valores pagos com cartão de débito. Zero se não utilizado.</td><td colspan="2">Pivot do payment_type=debit_card. Presente na base_tratada como campo homônimo.</td></tr>
<tr><td>not_defined</td><td colspan="2">Decimal (R$)</td><td>0</td><td>SUMIF de bases_payments</td><td>Valor residual de pagamentos sem tipo identificado.</td><td colspan="2">Corresponde a apenas 3 pedidos na base. Tratado como caso excepcional.</td></tr>
<tr><td>parcelas</td><td colspan="2">Inteiro</td><td>2</td><td>MAX de bases_payments por order_id</td><td>Número máximo de parcelas entre todos os pagamentos do pedido.</td><td colspan="2">Representa o parcelamento efetivo do pedido. Para pedidos à vista = 1. Presente na base_tratada.</td></tr>
<tr><td>valor_pagamento</td><td colspan="2">Decimal (R$)</td><td>72.19</td><td>SUM de payment_value por order_id</td><td>Valor total transacionado no pedido (soma de todos os tipos de pagamento).</td><td colspan="2">Pode diferir de valor_total_pedido por uso de voucher ou crédito. Presente na base_tratada.</td></tr>
</table>

**Tabela 10 de 15 — aux_itens_agregados**

<table>
<tr><td colspan="7">T10 — aux_itens_agregados</td></tr>
<tr><td colspan="2">Descrição</td><td colspan="6">Agrega frete e valor de produtos por pedido a partir da base_items. Resolve a multivalorização de pedidos com múltiplos itens, gerando um único registro por pedido com os totais consolidados.</td></tr>
<tr><td colspan="2">Tipo</td><td colspan="6">Tabela auxiliar de staging — Itens Agregados por Pedido</td></tr>
<tr><td colspan="2">Qtd. Campos</td><td colspan="6">3</td></tr>
<tr><td colspan="2">Chave Primária</td><td colspan="6">order_id (Rótulos de Linha)</td></tr>
<tr><td colspan="2">Chave(s) Estrangeira(s)</td><td colspan="6">order_id → base_orders → base_items</td></tr>
<tr><td>Campo</td><td colspan="2">Tipo</td><td>Exemplo</td><td>Origem</td><td>Regra de Cálculo</td><td colspan="2">Observações</td></tr>
<tr><td>order_id (Rótulos de Linha)</td><td colspan="2">Texto (Hash MD5)</td><td>00010242fe8c5a6d1ba2dd792cb16214</td><td>Tabela Dinâmica sobre base_items</td><td>order_id usado como linha na tabela dinâmica. Um registro por pedido.</td><td colspan="2">Chave primária desta tabela. Garante granularidade 1:1 necessária para a base_tratada.</td></tr>
<tr><td>Soma de freight_value</td><td colspan="2">Decimal (R$)</td><td>13.29</td><td>SUM de freight_value por order_id em base_items</td><td>Soma de todos os valores de frete dos itens do pedido.</td><td colspan="2">Presente na base_tratada como valor_frete. Usado no KPI de frete médio (R$22,90) e frete total (R$2.251.909).</td></tr>
<tr><td>Soma de price</td><td colspan="2">Decimal (R$)</td><td>58.90</td><td>SUM de price por order_id em base_items</td><td>Soma de todos os preços dos itens do pedido.</td><td colspan="2">Presente na base_tratada como valor_produtos. Usado no KPI de GMV e valor total dos produtos (R$13,6M).</td></tr>
</table>

### 1.6 Documentação da Tabela — Camada 3: Base Tratada (Analítica Central)

A base_tratada é a tabela mais importante do projeto. É o resultado da consolidação de todas as tabelas brutas e auxiliares em uma única visão por pedido (33 campos). Ela é a fonte direta para os indicadores, a estatística descritiva, as tabelas dinâmicas e o dashboard.

*Regra de construção: uma linha por pedido. Frete e produtos somados via aux_itens_agregados. Pagamentos pivotados via aux_payments_agregados. Forma de pagamento principal via aux_forma_pagamento. Dados de cliente via VLOOKUP em base_costumers. Colunas derivadas (SLA, atraso, faixas) calculadas via fórmulas Excel.*

**Tabela 11 de 15 — base_tratada**

<table>
<tr><td colspan="2">T11 — base_tratada</td></tr>
<tr><td>Descrição</td><td>Base analítica consolidada. Uma linha por pedido com 33 campos que integram dados de cliente, pedido, logística, financeiro, pagamento e indicadores derivados. É a principal tabela usada em todas as análises do projeto.</td></tr>
<tr><td>Tipo</td><td>Tabela analítica — Base Tratada (One Big Table)</td></tr>
<tr><td>Qtd. Campos</td><td>33</td></tr>
<tr><td>Chave Primária</td><td>id_pedido</td></tr>
<tr><td>Chave(s) Estrangeira(s)</td><td>id_cliente → base_costumers | (todos os demais campos via JOIN/VLOOKUP nas bases brutas e auxiliares)</td></tr>
</table>

**Grupo 1 — Identificação e Status do Pedido**

<table>
<tr><td>Campo</td><td>Tipo</td><td>Exemplo</td><td>Origem</td><td>Regra de Cálculo</td><td>Observações</td></tr>
<tr><td>id_pedido</td><td>Texto (Hash MD5)</td><td>e481f51cbdc54678b7cc49136f2d6af7</td><td>base_orders.order_id</td><td>Cópia direta do campo order_id.</td><td>Chave primária da base_tratada. Único por linha.</td></tr>
<tr><td>id_cliente</td><td>Texto (Hash MD5)</td><td>9ef432eb6251297304e76186b10a928d</td><td>base_orders.customer_id</td><td>Cópia direta do campo customer_id.</td><td>Chave estrangeira. Permite consulta a dados do cliente em base_costumers.</td></tr>
<tr><td>status_pedido</td><td>Texto (Categórico)</td><td>delivered</td><td>base_orders.order_status</td><td>Cópia direta. Indica o estágio atual do pedido.</td><td>Filtro principal para indicadores: apenas "delivered" entra em cálculos de entrega e SLA.</td></tr>
</table>

**Grupo 2 — Datas do Ciclo do Pedido**

<table>
<tr><td>Campo</td><td>Tipo</td><td>Exemplo</td><td>Origem</td><td>Regra de Cálculo</td><td>Observações</td></tr>
<tr><td>data_compra</td><td>Data/Hora (Serial Excel)</td><td>43010.455 (09/10/2017)</td><td>base_orders.order_purchase_timestamp</td><td>Cópia direta do timestamp de compra.</td><td>Base de todos os cálculos temporais do pedido.</td></tr>
<tr><td>data_aprovacao</td><td>Data/Hora (Serial Excel)</td><td>43010.463 (09/10/2017)</td><td>base_orders.order_approved_at</td><td>Cópia direta. Momento da aprovação do pagamento.</td><td>Pode ser nulo em pedidos não aprovados.</td></tr>
<tr><td>data_entrega_estimada</td><td>Data (Serial Excel)</td><td>43026 (25/10/2017)</td><td>base_orders.order_estimated_delivery_date</td><td>Cópia direta. Data prometida ao cliente.</td><td>Denominador do cálculo de SLA: se data_entrega_cliente ≤ data_entrega_estimada → No Prazo.</td></tr>
<tr><td>data_despacho</td><td>Data/Hora (Serial Excel)</td><td>43012.829 (11/10/2017)</td><td>base_orders.order_delivered_carrier_date</td><td>Cópia direta. Data de entrega à transportadora.</td><td>Pode ser nulo se o pedido não foi despachado.</td></tr>
<tr><td>data_entrega_cliente</td><td>Data/Hora (Serial Excel)</td><td>43018.892 (17/10/2017)</td><td>base_orders.order_delivered_customer_date</td><td>Cópia direta. Data de recebimento pelo cliente.</td><td>Nulo em 2.963 pedidos não entregues. Usado em prazo_entrega_dias e atraso_dias.</td></tr>
</table>

**Grupo 3 — SLA e Classificação Temporal**

<table>
<tr><td>Campo</td><td>Tipo</td><td>Exemplo</td><td>Origem</td><td>Regra de Cálculo</td><td>Observações</td></tr>
<tr><td>status_sla</td><td>Texto (Categórico)</td><td>No prazo</td><td>Calculado na base_tratada</td><td>SE(data_entrega_cliente &lt;= data_entrega_estimada → "No prazo"; data_entrega_cliente &gt; data_entrega_estimada → "Atrasado"; SENULO → "Não entregue"; incongruência de datas → "Inconsistente").</td><td>Campo central para análise de SLA. 4 valores possíveis: No prazo (89.936), Atrasado (6.534), Não entregue (2.963), Inconsistente (8).</td></tr>
<tr><td>prazo_entrega_dias</td><td>Inteiro (dias)</td><td>8</td><td>Calculado na base_tratada</td><td>INT(data_entrega_cliente) - INT(data_compra). Número inteiro de dias entre compra e entrega real.</td><td>Nulo quando data_entrega_cliente é nulo. Média = 12,5 dias; Mediana = 10 dias; Máximo = 210 dias.</td></tr>
<tr><td>prazo_estimado_dias</td><td>Inteiro (dias)</td><td>16</td><td>Calculado na base_tratada</td><td>INT(data_entrega_estimada) - INT(data_compra). Prazo prometido em dias corridos.</td><td>Sempre presente (campo de origem nunca nulo). Média = 24,4 dias; Mediana = 24 dias.</td></tr>
<tr><td>atraso_dias</td><td>Inteiro (dias, pode ser negativo)</td><td>-8</td><td>Calculado na base_tratada</td><td>INT(data_entrega_cliente) - INT(data_entrega_estimada). Valores negativos = entrega antecipada. Valores positivos = atraso real.</td><td>Calculado apenas quando ambas as datas estão presentes. Média = -11,9 dias (a maioria chega antes do prazo). Máximo positivo = +188 dias.</td></tr>
<tr><td>faixa_atraso</td><td>Texto (Categórico)</td><td>No prazo</td><td>Calculado na base_tratada</td><td>Classificação do atraso em faixas: atraso_dias ≤ 0 → "No prazo"; 1–3 → "Atraso de 1 a 3 dias"; 4–7 → "Atraso de 4 a 7 dias"; 8–15 → "Atraso de 8 a 15 dias"; &gt;15 → "Atraso acima de 15 dias"; sem entrega → "Não entregue".</td><td>Diretamente usada no TD 7 (Tabelas Dinâmicas) e no dashboard. Permite segmentação por severidade.</td></tr>
<tr><td>ano_compra</td><td>Inteiro</td><td>2017</td><td>Calculado: ANO(data_compra)</td><td>Extração do ano da data_compra.</td><td>Valores presentes: 2016, 2017, 2018.</td></tr>
<tr><td>mes_compra</td><td>Inteiro (1–12)</td><td>10</td><td>Calculado: MÊS(data_compra)</td><td>Extração do mês numérico da data_compra.</td><td>Usado em conjunto com nome_mes e ano_mes para análises temporais.</td></tr>
<tr><td>nome_mes</td><td>Texto</td><td>outubro</td><td>Calculado: ESCOLHA(mes_compra,...) ou TEXTO()</td><td>Nome do mês por extenso em português.</td><td>Usado como rótulo nos gráficos e tabelas dinâmicas.</td></tr>
<tr><td>ano_mes</td><td>Texto (AAAA-MM)</td><td>2017-10</td><td>Calculado: TEXTO(ano_compra,""0000"")&amp;""-""&amp;TEXTO(mes_compra,""00"")</td><td>Concatenação de ano e mês com separador "-" para ordenação cronológica.</td><td>Usado como eixo temporal na TD 1 (Pedidos por mês) e TD 8 (Valor por mês).</td></tr>
</table>

**Grupo 4 — Localização do Cliente**

<table>
<tr><td>Campo</td><td>Tipo</td><td>Exemplo</td><td>Origem</td><td>Regra de Cálculo</td><td>Observações</td></tr>
<tr><td>cidade_cliente</td><td>Texto</td><td>sao paulo</td><td>VLOOKUP: base_costumers.customer_city via id_cliente</td><td>Nome da cidade do endereço de entrega.</td><td>Em minúsculas. Pode conter variações de grafia. Não utilizado diretamente nos KPIs, mas útil para análises de granularidade maior que UF.</td></tr>
<tr><td>uf_cliente</td><td>Texto (Sigla UF)</td><td>SP</td><td>VLOOKUP: base_costumers.customer_state via id_cliente</td><td>Sigla do estado do cliente.</td><td>Chave para análises regionais: TD 2 (pedidos por estado), TD 4 (tempo médio por estado), TD 5 (frete por estado), TD 9 (frete médio por estado). Sempre preenchido.</td></tr>
</table>

**Grupo 5 — Valores Financeiros**

<table>
<tr><td>Campo</td><td>Tipo</td><td>Exemplo</td><td>Origem</td><td>Regra de Cálculo</td><td>Observações</td></tr>
<tr><td>valor_frete</td><td>Decimal (R$)</td><td>8.72</td><td>aux_itens_agregados.Soma de freight_value</td><td>Soma de todos os fretes dos itens do pedido.</td><td>KPI frete médio = MÉDIA desta coluna (R$22,90). KPI frete total = SOMA desta coluna (R$2.251.909).</td></tr>
<tr><td>valor_produtos</td><td>Decimal (R$)</td><td>29.99</td><td>aux_itens_agregados.Soma de price</td><td>Soma de todos os preços dos produtos do pedido.</td><td>KPI valor total dos produtos = R$13.591.643. Componente principal do GMV.</td></tr>
<tr><td>valor_total_pedido</td><td>Decimal (R$)</td><td>38.71</td><td>Calculado: valor_produtos + valor_frete</td><td>Soma de produtos e frete. Representa o valor de face do pedido.</td><td>KPI ticket médio = MÉDIA desta coluna (R$159,83). Componente do GMV total (R$15,8M). Pode diferir de valor_pagamento.</td></tr>
<tr><td>credit_card</td><td>Decimal (R$)</td><td>18.12</td><td>aux_payments_agregados.credit_card</td><td>Valor pago com cartão de crédito neste pedido.</td><td>Zero quando não utilizado. Usado na análise de forma de pagamento (TD 10).</td></tr>
<tr><td>boleto</td><td>Decimal (R$)</td><td>0</td><td>aux_payments_agregados.boleto</td><td>Valor pago com boleto bancário.</td><td>Zero quando não utilizado.</td></tr>
<tr><td>voucher</td><td>Decimal (R$)</td><td>20.59</td><td>aux_payments_agregados.voucher</td><td>Valor coberto por voucher/crédito.</td><td>Justifica diferença entre valor_total_pedido e valor_pagamento. Indica programa de fidelização.</td></tr>
<tr><td>debit_card</td><td>Decimal (R$)</td><td>0</td><td>aux_payments_agregados.debit_card</td><td>Valor pago com cartão de débito.</td><td>Zero quando não utilizado.</td></tr>
<tr><td>valor_pagamento</td><td>Decimal (R$)</td><td>38.71</td><td>aux_payments_agregados.valor_pagamento</td><td>Total efetivamente transacionado (soma de todos os meios de pagamento).</td><td>KPI Receita de Pagamentos = SOMA desta coluna (R$16.008.872). Pode ser maior que valor_total_pedido (ex: crédito extra).</td></tr>
<tr><td>parcelas</td><td>Inteiro</td><td>1</td><td>aux_payments_agregados.parcelas</td><td>Número máximo de parcelas do pedido.</td><td>Média = 2,93 parcelas. Mediana = 2. Máximo = 24. Usado em estatística descritiva.</td></tr>
<tr><td>forma_pagamento_principal</td><td>Texto (Categórico)</td><td>credit_card</td><td>aux_forma_pagamento.forma_pagamento</td><td>Forma de pagamento predominante do pedido.</td><td>Usada no TD 10. Valores: credit_card (75,8%), boleto (19,9%), voucher (2,8%), debit_card (1,5%), not_defined (&lt;0,01%).</td></tr>
</table>

**Grupo 6 — Indicadores Binários (Flags)**

<table>
<tr><td>Campo</td><td>Tipo</td><td>Exemplo</td><td>Origem</td><td>Regra de Cálculo</td><td>Observações</td></tr>
<tr><td>pedido_entregue</td><td>Inteiro (0 ou 1)</td><td>1</td><td>Calculado: SE(data_entrega_cliente&lt;&gt;"";1;0)</td><td>1 se o pedido foi entregue (data_entrega_cliente preenchida), 0 caso contrário.</td><td>SOMA desta coluna = 96.470 pedidos entregues. Taxa entrega = SOMA/TOTAL = 97,0%.</td></tr>
<tr><td>pedido_no_prazo</td><td>Inteiro (0 ou 1)</td><td>1</td><td>Calculado: SE(status_sla="No prazo";1;0)</td><td>1 se status_sla = "No prazo", 0 caso contrário.</td><td>SOMA = 89.936. Taxa SLA = SOMA/pedidos_entregues = 93,2%.</td></tr>
<tr><td>pedido_atrasado</td><td>Inteiro (0 ou 1)</td><td>0</td><td>Calculado: SE(status_sla="Atrasado";1;0)</td><td>1 se status_sla = "Atrasado", 0 caso contrário.</td><td>SOMA = 6.534. Taxa de atraso = SOMA/pedidos_entregues = 6,77%.</td></tr>
<tr><td>pedido_inconsistente</td><td>Inteiro (0 ou 1)</td><td>0</td><td>Calculado: SE(status_sla="Inconsistente";1;0)</td><td>1 se o pedido possui inconsistência de datas (ex: entrega antes da compra).</td><td>SOMA = 8. Muito baixo, não impacta KPIs principais. Manter monitorado.</td></tr>
</table>

### 1.7 Documentação das Tabelas — Camada 4: Tabelas Analíticas (Output)

As tabelas analíticas representam a camada de saída do projeto. São construídas a partir da base_tratada e sintetizam os resultados para consumo executivo e operacional. Incluem os KPIs calculados, a estatística descritiva das principais variáveis, as tabelas dinâmicas de resumo e o dashboard.

**Tabela 12 de 15 — indicadores**

<table>
<tr><td colspan="8">T12 — indicadores</td></tr>
<tr><td colspan="2">Descrição</td><td colspan="6">Centraliza todos os KPIs operacionais e financeiros calculados a partir da base_tratada. Cada linha é um indicador com nome, valor calculado e interpretação de negócio. Serve como fonte direta para o dashboard.</td></tr>
<tr><td colspan="2">Tipo</td><td colspan="6">Tabela analítica — Indicadores (KPIs)</td></tr>
<tr><td colspan="2">Qtd. Campos</td><td colspan="6">3</td></tr>
<tr><td colspan="2">Chave Primária</td><td colspan="6">Indicador (nome do KPI)</td></tr>
<tr><td colspan="2">Chave(s) Estrangeira(s)</td><td colspan="6">—</td></tr>
<tr><td>Campo</td><td colspan="2">Tipo</td><td>Exemplo</td><td>Origem</td><td>Regra de Cálculo</td><td colspan="2">Observações</td></tr>
<tr><td>Indicador</td><td colspan="2">Texto</td><td>Total de pedidos</td><td>Definição do analista</td><td>Nome descritivo do KPI.</td><td colspan="2">Rótulo do indicador.</td></tr>
<tr><td>Resultado</td><td colspan="2">Numérico ou Percentual</td><td>99.441</td><td>Fórmula sobre base_tratada</td><td>Valor calculado do KPI. Veja regras detalhadas na tabela abaixo.</td><td colspan="2">Atualizado automaticamente se a base_tratada for alterada.</td></tr>
<tr><td>Interpretação</td><td colspan="2">Texto Livre</td><td>Quantidade total de pedidos existentes na base analisada.</td><td>Definição do analista</td><td>Texto explicativo do que o indicador mede e como interpretar.</td><td colspan="2">Campo narrativo. Não calculado.</td></tr>
</table>

**Detalhamento dos KPIs e suas Regras de Cálculo:**

<table>
<tr><td>KPI</td><td>Resultado</td><td>Regra de Cálculo</td></tr>
<tr><td>Total de pedidos</td><td>99.441</td><td>CONTAGEM de id_pedido em base_tratada (todas as linhas)</td></tr>
<tr><td>Pedidos entregues</td><td>96.470</td><td>SOMASE(pedido_entregue = 1)</td></tr>
<tr><td>Pedidos no prazo</td><td>89.936</td><td>SOMASE(pedido_no_prazo = 1)</td></tr>
<tr><td>Pedidos atrasados</td><td>6.534</td><td>SOMASE(pedido_atrasado = 1)</td></tr>
<tr><td>Pedidos não entregues</td><td>2.963</td><td>Total de pedidos - Pedidos entregues (96.470 - 2.963 = 99.433 ≈ total - pedidos_entregues)</td></tr>
<tr><td>Pedidos inconsistentes</td><td>8</td><td>SOMASE(pedido_inconsistente = 1)</td></tr>
<tr><td>Taxa de entrega no prazo</td><td>93,23%</td><td>pedidos_no_prazo / pedidos_entregues = 89.936 / 96.470</td></tr>
<tr><td>Percentual não entregue</td><td>2,98%</td><td>pedidos_nao_entregues / total_pedidos = 2.963 / 99.441</td></tr>
<tr><td>Taxa de atraso</td><td>6,77%</td><td>pedidos_atrasados / pedidos_entregues = 6.534 / 96.470</td></tr>
<tr><td>Tempo médio de entrega</td><td>12,50 dias</td><td>MÉDIA(prazo_entrega_dias) para pedidos entregues</td></tr>
<tr><td>Maior prazo de entrega</td><td>210 dias</td><td>MÁXIMO(prazo_entrega_dias)</td></tr>
<tr><td>Menor prazo de entrega</td><td>1 dia</td><td>MÍNIMO(prazo_entrega_dias &gt; 0)</td></tr>
<tr><td>Ticket médio</td><td>R$ 159,83</td><td>MÉDIA(valor_total_pedido) para pedidos entregues</td></tr>
<tr><td>Frete médio por pedido</td><td>R$ 22,90</td><td>MÉDIA(valor_frete) para pedidos com frete &gt; 0</td></tr>
<tr><td>Frete total</td><td>R$ 2.251.909,54</td><td>SOMA(valor_frete)</td></tr>
<tr><td>Valor total dos produtos</td><td>R$ 13.591.643,70</td><td>SOMA(valor_produtos)</td></tr>
<tr><td>GMV (Preço + Frete)</td><td>R$ 15.843.553,24</td><td>SOMA(valor_total_pedido) = Valor produtos + Frete</td></tr>
<tr><td>Receita de Pagamentos</td><td>R$ 16.008.872,12</td><td>SOMA(valor_pagamento) — inclui vouchers e créditos</td></tr>
<tr><td>Avaliação média</td><td>4,09 / 5</td><td>MÉDIA(review_score) a partir de base_reviews (JOIN com base_tratada via order_id)</td></tr>
</table>

**Tabela 13 de 15 — estatística_descritiva**

<table>
<tr><td colspan="10">T13 — estatística_descritiva</td></tr>
<tr><td colspan="2">Descrição</td><td colspan="8">Consolida a análise estatística descritiva das principais variáveis numéricas do projeto. Cada coluna representa uma variável e cada linha uma medida estatística.</td></tr>
<tr><td colspan="2">Tipo</td><td colspan="8">Tabela analítica — Estatística Descritiva</td></tr>
<tr><td colspan="2">Qtd. Campos</td><td colspan="8">9</td></tr>
<tr><td colspan="2">Chave Primária</td><td colspan="8">Métrica (linha)</td></tr>
<tr><td colspan="2">Chave(s) Estrangeira(s)</td><td colspan="8">—</td></tr>
<tr><td>Métrica</td><td colspan="2">Prazo Entrega</td><td>Prazo Estimado</td><td>Atraso</td><td>Frete</td><td>Valor Produtos</td><td>Valor Total Pedido</td><td>Valor Pagamento</td><td colspan="2">Parcelas</td></tr>
<tr><td>Quantidade</td><td colspan="2">96.475</td><td>99.441</td><td>6.535</td><td>98.328</td><td>98.666</td><td>98.666</td><td>99.437</td><td colspan="2">99.438</td></tr>
<tr><td>Média</td><td colspan="2">12,5 d</td><td>24,4 d</td><td>-11,9 d</td><td>R$22,90</td><td>R$137,75</td><td>R$159,83</td><td>R$159,86</td><td colspan="2">2,93</td></tr>
<tr><td>Mediana</td><td colspan="2">10 d</td><td>24 d</td><td>-12 d</td><td>R$17,09</td><td>R$85,00</td><td>R$104,56</td><td>R$105,29</td><td colspan="2">2</td></tr>
<tr><td>Mínimo</td><td colspan="2">1 d</td><td>2 d</td><td>-147 d</td><td>R$5,70</td><td>R$0,85</td><td>R$9,59</td><td>R$9,59</td><td colspan="2">1</td></tr>
<tr><td>Máximo</td><td colspan="2">210 d</td><td>156 d</td><td>+188 d</td><td>R$1.795</td><td>R$13.440</td><td>R$13.664</td><td>R$13.664</td><td colspan="2">24</td></tr>
<tr><td>Amplitude</td><td colspan="2">209 d</td><td>154 d</td><td>335 d</td><td>R$1.789</td><td>R$13.439</td><td>R$13.654</td><td>R$13.654</td><td colspan="2">23</td></tr>
<tr><td>Desvio Padrão</td><td colspan="2">9,56 d</td><td>8,83 d</td><td>10,18 d</td><td>R$21,66</td><td>R$210,17</td><td>R$220,06</td><td>R$221,95</td><td colspan="2">2,72</td></tr>
</table>

*Interpretação: A mediana do prazo de entrega (10 dias) é menor que a média (12,5 dias), indicando que pedidos com prazos muito longos puxam a média para cima. Para o atraso, a mediana negativa (-12 dias) confirma que a maioria dos pedidos chega antes do prazo, com os atrasos positivos sendo eventos mais raros mas de alta severidade.*

**Tabela 14 de 15 — tabelas_dinâmicas**

<table>
<tr><td colspan="6">T14 — tabelas_dinâmicas</td></tr>
<tr><td colspan="2">Descrição</td><td colspan="4">Aba que consolida 10 tabelas dinâmicas (pivot tables) construídas a partir da base_tratada. Cada tabela dinâmica responde a uma pergunta analítica específica.</td></tr>
<tr><td colspan="2">Tipo</td><td colspan="4">Tabela analítica — Tabelas Dinâmicas (Pivot Tables)</td></tr>
<tr><td colspan="2">Qtd. Campos</td><td colspan="4">Variável por TD</td></tr>
<tr><td colspan="2">Chave Primária</td><td colspan="4">—</td></tr>
<tr><td colspan="2">Chave(s) Estrangeira(s)</td><td colspan="4">—</td></tr>
<tr><td>Nº</td><td colspan="2">Nome da TD</td><td>Dimensões</td><td colspan="2">Métricas / Resultado</td></tr>
<tr><td>TD 1</td><td colspan="2">Pedidos por Mês</td><td>Linhas: ano_mes</td><td colspan="2">Contagem de pedidos por período. Evidencia sazonalidade e crescimento (2016–2018).</td></tr>
<tr><td>TD 2</td><td colspan="2">Pedidos por Estado (UF)</td><td>Linhas: uf_cliente</td><td colspan="2">Contagem de pedidos por estado. SP=41.746 (42%), RJ=12.852 (12,9%), MG=11.635 (11,7%).</td></tr>
<tr><td>TD 3</td><td colspan="2">Status de SLA</td><td>Linhas: status_sla</td><td colspan="2">Distribuição por status: No prazo (89.936), Atrasado (6.534), Não entregue (2.963), Inconsistente (8).</td></tr>
<tr><td>TD 4</td><td colspan="2">Tempo Médio de Entrega/UF</td><td>Linhas: uf_cliente</td><td colspan="2">MÉDIA(prazo_entrega_dias) por estado. SP=8,7 dias (melhor). RR=29,3 dias (pior).</td></tr>
<tr><td>TD 5</td><td colspan="2">Frete Total por Estado</td><td>Linhas: uf_cliente</td><td colspan="2">SOMA(valor_frete) por estado. SP=R$718.723. Valores proporcionais ao volume de pedidos.</td></tr>
<tr><td>TD 6</td><td colspan="2">Status Original do Pedido</td><td>Linhas: status_pedido</td><td colspan="2">Distribuição por order_status: delivered(96.478), shipped(1.107), canceled(625), unavailable(609), others.</td></tr>
<tr><td>TD 7</td><td colspan="2">Faixa de Atraso</td><td>Linhas: faixa_atraso</td><td colspan="2">Distribuição de atrasos: No prazo (89.936), Atraso 1-3d (1.870), 4-7d (1.802), 8-15d (1.637), &gt;15d (1.225), Não entregue (2.963).</td></tr>
<tr><td>TD 8</td><td colspan="2">Valor Total por Mês</td><td>Linhas: nome_mes</td><td colspan="2">SOMA(valor_total_pedido) por mês. Janeiro=R$1,24M. Novembro=R$1,18M (influência de Black Friday).</td></tr>
<tr><td>TD 9</td><td colspan="2">Frete Médio por Estado</td><td>Linhas: uf_cliente</td><td colspan="2">MÉDIA(valor_frete) por estado. SP=R$17,22 (mais baixo). RR=R$48,59, PB=R$48,62 (mais altos — 182% acima de SP).</td></tr>
<tr><td>TD 10</td><td colspan="2">Forma de Pagamento</td><td>Linhas: forma_pagamento_principal</td><td colspan="2">Contagem e Valor total por forma de pagamento. credit_card=75.387 pedidos (R$12,5M = 78,6% do volume).</td></tr>
</table>

**Tabela 15 de 15 — dashboard**

<table>
<tr><td colspan="4">T15 — dashboard</td></tr>
<tr><td>Descrição</td><td colspan="3">Painel gerencial interativo construído sobre as tabelas dinâmicas e a base_tratada. Apresenta os principais KPIs, gráficos de distribuição e permite filtragem por período, estado, SLA e forma de pagamento.</td></tr>
<tr><td>Tipo</td><td colspan="3">Camada de visualização — Dashboard Gerencial</td></tr>
<tr><td>Qtd. Campos</td><td colspan="3">N/A (visual)</td></tr>
<tr><td>Chave Primária</td><td colspan="3">—</td></tr>
<tr><td>Chave(s) Estrangeira(s)</td><td colspan="3">—</td></tr>
<tr><td colspan="2">Componente</td><td colspan="2">Descrição</td></tr>
<tr><td colspan="2">KPIs Principais</td><td colspan="2">Cards com total de pedidos, taxa de entrega, SLA, taxa de atraso, ticket médio, frete médio, GMV e avaliação média.</td></tr>
<tr><td colspan="2">Gráfico de Volume</td><td colspan="2">Evolução mensal do número de pedidos (TD 1). Permite identificar sazonalidade e crescimento.</td></tr>
<tr><td colspan="2">Gráfico de SLA</td><td colspan="2">Distribuição de status SLA (No prazo, Atrasado, Não entregue). Fonte: TD 3.</td></tr>
<tr><td colspan="2">Mapa Regional</td><td colspan="2">Distribuição de pedidos por estado (TD 2) e tempo médio de entrega (TD 4).</td></tr>
<tr><td colspan="2">Distribuição de Frete</td><td colspan="2">Comparação de frete médio por UF (TD 9). Evidencia disparidade regional.</td></tr>
<tr><td colspan="2">Pagamentos</td><td colspan="2">Distribuição por forma de pagamento (TD 10): volume e valor por canal.</td></tr>
<tr><td colspan="2">Atrasos por Faixa</td><td colspan="2">Gráfico de faixas de atraso (TD 7). Segmentação por severidade.</td></tr>
<tr><td colspan="2">Filtros Interativos</td><td colspan="2">Segmentadores: período (ano_mes), estado (uf_cliente), status SLA, forma de pagamento. Permitem análise dinâmica.</td></tr>
</table>

### 1.8 Consolidado de Chaves Primárias e Estrangeiras

<table>
<tr><td>Tabela</td><td>Chave Primária (PK)</td><td>Chaves Estrangeiras (FK)</td><td>Notas</td></tr>
<tr><td>base_costumers</td><td>customer_id</td><td>—</td><td>customer_unique_id para rastreio de recompra.</td></tr>
<tr><td>base_orders</td><td>order_id</td><td>customer_id → base_costumers</td><td>Tabela central do modelo.</td></tr>
<tr><td>base_items</td><td>order_id + order_item_id</td><td>order_id → base_orders product_id → base_products</td><td>Chave composta.</td></tr>
<tr><td>base_products</td><td>product_id</td><td>product_category_name → base_categories</td><td>—</td></tr>
<tr><td>bases_payments</td><td>order_id + payment_sequential</td><td>order_id → base_orders</td><td>Chave composta.</td></tr>
<tr><td>base_categories</td><td>product_category_name</td><td>—</td><td>Tabela de de-para.</td></tr>
<tr><td>base_reviews</td><td>review_id</td><td>order_id → base_orders</td><td>—</td></tr>
<tr><td>aux_forma_pagamento</td><td>order_id</td><td>order_id → base_orders</td><td>Derivada.</td></tr>
<tr><td>aux_payments_agregados</td><td>order_id</td><td>order_id → base_orders</td><td>Derivada.</td></tr>
<tr><td>aux_itens_agregados</td><td>order_id</td><td>order_id → base_orders</td><td>Derivada.</td></tr>
<tr><td>base_tratada</td><td>id_pedido (= order_id)</td><td>id_cliente → base_costumers e todas as auxiliares via id_pedido</td><td>One Big Table.</td></tr>
</table>

### 1.9 Glossário de Termos e Siglas

<table>
<tr><td>Termo / Sigla</td><td>Definição</td></tr>
<tr><td>SLA</td><td>Service Level Agreement — Acordo de Nível de Serviço. No contexto deste projeto, refere-se ao cumprimento do prazo de entrega prometido ao cliente.</td></tr>
<tr><td>GMV</td><td>Gross Merchandise Value — Valor total de mercadorias transacionadas. Calculado como soma de valor_produtos + valor_frete.</td></tr>
<tr><td>Ticket Médio</td><td>Valor médio por pedido. Calculado como MÉDIA(valor_total_pedido) sobre pedidos entregues.</td></tr>
<tr><td>CSAT</td><td>Customer Satisfaction Score — Pontuação de satisfação do cliente. Aproximado aqui pela avaliação média (review_score).</td></tr>
<tr><td>CAC</td><td>Customer Acquisition Cost — Custo de aquisição de cliente. Mencionado no contexto estratégico das recomendações.</td></tr>
<tr><td>Hash MD5</td><td>Identificador único gerado por função de hash criptográfica (32 caracteres hexadecimais). Todos os IDs neste dataset são hashes MD5.</td></tr>
<tr><td>Serial Excel</td><td>Formato interno do Excel para datas: número inteiro representando dias desde 01/01/1900 (ex: 43010 = 09/10/2017). A parte decimal representa a hora.</td></tr>
<tr><td>Power Query</td><td>Ferramenta de ETL integrada ao Microsoft Excel usada para importar, transformar e combinar dados das bases brutas.</td></tr>
<tr><td>Tabela Dinâmica</td><td>Pivot Table: recurso do Excel para sumarização e agrupamento de dados por categorias, com cálculo automático de métricas.</td></tr>
<tr><td>Atraso_dias</td><td>Campo calculado = data_entrega_cliente - data_entrega_estimada. Negativo = adiantamento. Positivo = atraso efetivo.</td></tr>
<tr><td>Não entregue</td><td>Pedidos com status diferente de "delivered" ou sem data_entrega_cliente registrada (2.963 casos = 2,98% do total).</td></tr>
<tr><td>Inconsistente</td><td>Pedidos com anomalia de datas (ex: entrega anterior à compra). Apenas 8 casos na base.</td></tr>
<tr><td>One Big Table</td><td>Padrão analítico em que todos os dados são consolidados em uma única tabela desnormalizada para facilitar análise.</td></tr>
<tr><td>RFM</td><td>Recência, Frequência e Monetário — framework de segmentação de clientes mencionado como melhoria futura.</td></tr>
<tr><td>Faixa de Atraso</td><td>Classificação categórica do atraso: No prazo, 1-3 dias, 4-7 dias, 8-15 dias, acima de 15 dias, Não entregue.</td></tr>
</table>
