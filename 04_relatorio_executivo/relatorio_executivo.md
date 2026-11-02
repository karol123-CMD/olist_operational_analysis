> Cópia em Markdown para leitura no GitHub, convertida de [Relatório Executivo_ Olist E-Commerce.docx](<Relatório Executivo_ Olist E-Commerce.docx>). O Word original foi preservado; o texto e os valores foram transcritos, sem revisão ou reconciliação nesta etapa.

# RELATÓRIO EXECUTIVO: Análise Operacional de Pedidos e Entregas SLA

Empresa: Olist E-Commerce | Fonte: Brazilian Public Dataset | Período: 2016–2018

**Ficha Técnica**

Escopo da Análise: 99.441 pedidos (Set/2016 a Ago/2018)

Ferramentas: Microsoft Excel, Power Query, Tabelas Dinâmicas

Fonte dos Dados: Kaggle - Brazilian E-Commerce Public Dataset by Olist

Classificação: Uso Interno e Portfólio Analítico

**Resumo Executivo**

A Olist cresce mais rápido do que sua infraestrutura logística consegue acompanhar, e esse descompasso está corroendo silenciosamente o ativo mais valioso de um marketplace: a confiança do comprador. Este relatório demonstra que o problema não é pontual nem sazonal no sentido esperado: em março de 2018, a taxa de atraso atingiu 19%, quase três vezes a média anual, revelando que picos de demanda expõem fragilidades estruturais na cadeia de entrega. Mais crítico ainda: 57% das avaliações negativas da plataforma não vêm de pedidos atrasados, vêm de pedidos entregues dentro do prazo. Isso significa que resolver a logística, sozinho, não resolverá a insatisfação do cliente.

A Olist registrou quase 100 mil pedidos entre setembro de 2016 e agosto de 2018, com volume transacionado (GMV) superior a R$15,8 milhões. A operação demonstra solidez no crescimento, o número de pedidos mais que dobrou de 2017 para 2018, mas revela gargalos logísticos que demandam atenção imediata para proteger e prosperar a experiência do cliente e a reputação da marca.

**Destaques Positivos**

Crescimento de 135% no volume de pedidos entre jan-ago de 2017 e o mesmo período de 2018

Taxa de entrega de 97%, apenas 3% dos pedidos ficaram sem entrega registrada

93,2% dos pedidos entregues chegaram dentro do prazo estimado

Avaliação média de 4,09 / 5, com 57,8% dos clientes atribuindo nota máxima

Cartão de crédito responde por 75,8% das transações, indicando maturidade digital da base de clientes

**Pontos de Atenção**

6.534 pedidos entregues com atraso, 43,8% desses com atraso superior a 8 dias

Frete representa 14,2% do GMV; estados do Norte e Nordeste pagam até 3x mais que SP

Concentração geográfica: 68,6% dos pedidos vêm do Sudeste, risco de dependência regional

14,7% dos clientes avaliaram com notas 1 ou 2, sinal de insatisfação relevante

2.963 pedidos sem entrega registrada (3% do total) sem causa identificável na base

**2. Contexto do Negócio**

A Olist é um marketplace brasileiro que conecta vendedores de pequeno e médio porte a consumidores finais. Diferentemente de grandes plataformas diretas, a empresa atua como intermediadora logística e comercial, o que torna o desempenho de entrega e a experiência pós-venda elementos muito importantes de seu modelo de negócio.

Este relatório analisa o desempenho operacional da plataforma sob três perspectivas fundamentais:

Eficiência logística: cumprimento de prazos, tempo de entrega e cobertura geográfica.

Performance financeira: volume transacionado, ticket médio, composição de receita e custo de frete.

Experiência do cliente: taxa de satisfação e distribuição de avaliações.

Os dados abrangem 24 meses de operação (set/2016 a ago/2018), período que inclui o crescimento acelerado do e-commerce brasileiro e a expansão da base de clientes da Olist para além do eixo Sudeste.

**3. Principais Indicadores (KPIs)**

<table>
<tr><td>Total de Pedidos</td><td>99.441 (Set/2016 – Ago/2018)</td><td>GMV (Produtos + Frete)</td><td>R$ 15,8M (Volume total transacionado)</td></tr>
<tr><td>Ticket Médio</td><td>R$ 159,83 (Por pedido entregue)</td><td>Frete Médio</td><td>R$ 22,90 (14,2% do GMV)</td></tr>
<tr><td>Taxa de Entrega</td><td>97,0% (96.470 pedidos entregues)</td><td>SLA no Prazo</td><td>93,23% (Sobre pedidos entregues)</td></tr>
<tr><td>Taxa de Atraso</td><td>6,77% (6.534 pedidos atrasados)</td><td>Avaliação Média</td><td>4,09/5 (57,8% nota máxima)</td></tr>
</table>

O ticket médio de R$ 159,83 foi calculado exclusivamente sobre os 96.470 pedidos com entrega confirmada, excluindo pedidos cancelados, não entregues e os 8 registros classificados como inconsistentes (status "entregue" sem data de entrega registrada). A base apresenta 965 pedidos acima do percentil 99 (valor ≥ R$ 1.052,40), com o maior pedido registrado em R$ 13.664,08. A manutenção desses valores na análise eleva o ticket médio em R$ 15,18 em relação à média sem outliers (R$ 144,65). Optou-se por preservá-los, pois representam transações legítimas de alto valor; contudo, análises de comportamento típico do consumidor devem considerar a mediana (R$ 104,56) como métrica mais representativa. O GMV reportado (R$ 15,8M) difere da soma de pagamentos registrados (R$ 16,0M); a diferença de R$ 165K requer investigação adicional na camada de pagamentos parcelados e eventuais juros capturados pelo sistema, não podendo ser atribuída exclusivamente ao uso de vouchers sem confirmação nos dados de origem.

**Interpretação dos KPIs**

GMV de R$ 15,8 milhões: representa o total movimentado pela plataforma, incluindo produtos (85,8%) e frete (14,2%). Este é o principal indicador de escala do negócio. A receita efetivamente transacionada via pagamentos soma R$ 16,0 milhões, diferença justificada pelo uso de vouchers e créditos pelos clientes, o que indica um programa de fidelização ativo.

Ticket médio de R$ 159,83: calculado sobre os 96.470 pedidos efetivamente entregues, o valor indica compras de médio porte. Há uma assimetria expressiva: a mediana do ticket é R$ 104,56, bem abaixo da média, sinalizando que pedidos de alto valor puxam a média para cima, enquanto a maioria dos clientes realiza compras menores.

Taxa de entrega de 97%: dos 99.441 pedidos, 96.470 foram entregues. Os 2.963 não entregues (3%) merecem investigação: podem representar cancelamentos pós-pagamento, falhas na transportadora ou fraudes. Embora 3% pareça baixo, em escala representa potencial impacto financeiro e de reputação relevante.

SLA de 93,2%: a cada 100 pedidos entregues, aproximadamente 7 chegam com atraso. O número absoluto de 6.534 pedidos atrasados, combinado com a severidade,  43,8% desses com atraso superior a 8 dias, revela que o problema não é apenas frequência, mas também profundidade dos atrasos.

**4. Principais Insights**

**4.1 Crescimento Acelerado com Riscos Operacionais**

O volume de pedidos cresceu 135% na comparação entre janeiro-agosto de 2017 e o mesmo período de 2018 (de 22.968 para 53.991 pedidos). Esse crescimento expressivo é positivo do ponto de vista comercial, mas impõe pressão sobre a operação logística. O pico de novembro de 2017 (7.544 pedidos em um único mês) sugere que datas sazonais como Black Friday geram picos que a rede de entregas pode ter dificuldade de absorver sem queda de qualidade.

**4.2 Concentração Geográfica: Oportunidade e Vulnerabilidade**

<table>
<tr><td>Estado</td><td>Qtd. Pedidos</td><td>% do Total</td></tr>
<tr><td>São Paulo (SP)</td><td>41.746</td><td>42,0%</td></tr>
<tr><td>Rio de Janeiro (RJ)</td><td>12.852</td><td>12,9%</td></tr>
<tr><td>Minas Gerais (MG)</td><td>11.635</td><td>11,7%</td></tr>
<tr><td>Rio Grande do Sul (RS)</td><td>5.466</td><td>5,5%</td></tr>
<tr><td>Paraná (PR)</td><td>5.045</td><td>5,1%</td></tr>
<tr><td>Demais estados</td><td>22.697</td><td>22,8%</td></tr>
</table>

O Sudeste concentra 68,6% dos pedidos. São Paulo isoladamente representa 42% da operação. Essa concentração tem dois lados: por um lado, facilita a logística e reduz o frete médio para a maioria dos clientes; por outro, representa vulnerabilidade estratégica, qualquer disrupção operacional nessa região impacta diretamente quase metade do negócio. Estados como Nordeste e Norte, apesar do menor volume, apresentam ticket médio mais alto (acima de R$ 210), indicando potencial de expansão com rentabilidade atrativa.

**4.3 Perfil de Atrasos: Problema Estrutural, Não Pontual**

<table>
<tr><td>Faixa de Atraso</td><td>Pedidos</td><td>% dos Atrasados</td></tr>
<tr><td>Atraso de 1 a 3 dias (leve)</td><td>1.870</td><td>28,6%</td></tr>
<tr><td>Atraso de 4 a 7 dias (moderado)</td><td>1.802</td><td>27,6%</td></tr>
<tr><td>Atraso de 8 a 15 dias (grave)</td><td>1.637</td><td>25,1%</td></tr>
<tr><td>Atraso acima de 15 dias (crítico)</td><td>1.225</td><td>18,7%</td></tr>
</table>

A primeira leitura da tabela sugere uniformidade entre as faixas de atraso, mas a análise temporal revela um padrão radicalmente diferente. A taxa de atraso não é estável: em novembro de 2017 (pico de Black Friday, com 7.544 pedidos), a taxa de atraso saltou para 12,4%, quase o dobro da média anual de 6,8%. Em fevereiro e março de 2018, os índices atingiram 14,1% e 19,0%, respectivamente, sugerindo que o crescimento acelerado do primeiro trimestre de 2018 superou a capacidade de absorção das transportadoras parceiras. Isso revela que o problema não é contínuo e distribuído igualmente: ele é sazonal e escalável, ou seja, quanto maior o volume de pedidos, maior a proporção de atrasos. A implicação estratégica é direta: a operação logística atual não suporta crescimento sem degradação de SLA, e qualquer iniciativa de expansão de vendas deve ser acompanhada de capacidade logística proporcional.

Adicionalmente, a análise regional dos atrasos expõe um segundo padrão crítico: estados do Nordeste apresentam taxas de atraso entre 2 e 3 vezes superiores à média nacional. Alagoas lidera com 21,4% de taxa de atraso, seguida por Maranhão (17,4%) e Sergipe (15,2%). Esses são os mesmos estados com o maior ticket médio da base, clientes que compram mais e são entregues pior. O custo de frete elevado que esses clientes já pagam, combinado com a alta taxa de atraso, cria a combinação mais danosa possível para retenção: experiência cara e decepcionante.

**4.4 Desigualdade no Custo de Frete por Região**

<table>
<tr><td>Estado</td><td>Frete Médio</td><td>Relação com SP</td></tr>
<tr><td>São Paulo (SP)</td><td>R$ 17,22</td><td>Referência</td></tr>
<tr><td>Minas Gerais (MG)</td><td>R$ 23,28</td><td>+35%</td></tr>
<tr><td>Rio de Janeiro (RJ)</td><td>R$ 23,78</td><td>+38%</td></tr>
<tr><td>Pará (PA)</td><td>R$ 39,94</td><td>+132%</td></tr>
<tr><td>Maranhão (MA)</td><td>R$ 42,66</td><td>+148%</td></tr>
<tr><td>Paraíba (PB)</td><td>R$ 48,62</td><td>+182%</td></tr>
<tr><td>Roraima (RR)</td><td>R$ 48,59</td><td>+182%</td></tr>
</table>

Clientes do Norte e Nordeste pagam até 3 vezes mais frete que clientes de São Paulo. Embora isso reflita a realidade logística do Brasil, o impacto na conversão de vendas nessas regiões é direto: o frete elevado pode ser fator decisivo de abandono de carrinho. Ao mesmo tempo, esses estados apresentam o maior ticket médio da base, sugerindo que, quando o cliente compra, compra em maior quantidade ou valor, possivelmente para diluir o custo de frete.

**4.5 Domínio do Cartão de Crédito e Dependência de um Canal**

<table>
<tr><td>Forma de Pagamento</td><td>Pedidos</td><td>Volume (R$)</td></tr>
<tr><td>Cartão de Crédito</td><td>75.387 (75,8%)</td><td>R$ 12.452.802,61 (78,6%)</td></tr>
<tr><td>Boleto Bancário</td><td>19.784 (19,9%)</td><td>R$ 2.842.240,29 (17,9%)</td></tr>
<tr><td>Voucher/Crédito</td><td>2.739 (2,8%)</td><td>R$ 333.259,16 (2,1%)</td></tr>
<tr><td>Cartão de Débito</td><td>1.527 (1,5%)</td><td>R$ 215.107,72 (1,4%)</td></tr>
</table>

O cartão de crédito responde por 78,6% do volume financeiro transacionado. Essa dominância é positiva (maior capacidade de compra, parcelamento) mas representa um risco de concentração de canal: qualquer instabilidade em adquirentes ou bandeiras teria impacto imediato e severo na operação. O boleto, com quase 20% dos pedidos, mantém relevância, especialmente para públicos sem acesso a crédito ou que preferem pagamento à vista.

**4.6 A Raiz da Insatisfação Não é Onde Parece**

O cruzamento dos dados de pedidos com as 99.224 avaliações registradas revela um achado contraintuitivo que redefine as prioridades de ação. Pedidos entregues com atraso têm avaliação média de 2,27, contra 4,29 dos pedidos entregues no prazo. A correlação é clara e a diferença é economicamente relevante: 61% dos pedidos atrasados resultam em nota 1 ou 2, contra apenas 9,2% dos pedidos no prazo.

No entanto, ao analisar a origem de todas as avaliações negativas da plataforma, emerge o dado mais crítico desta análise: 57,3% das 14.575 notas ruins vieram de pedidos entregues dentro do prazo. Apenas 27,4% vieram de pedidos atrasados, e 15,3% de pedidos não entregues. Isso significa que, mesmo que a Olist eliminasse 100% dos atrasos amanhã, aproximadamente 8.346 avaliações negativas permaneceriam intactas, originadas em problemas de outra natureza: qualidade do produto, comportamento do vendedor, embalagem ou atendimento pós-venda.

O ticket médio dos pedidos entregues no prazo com nota ruim é R$ 190,8, acima da média geral, indicando que são clientes de maior valor que estão insatisfeitos com algo que não é o prazo. A conclusão operacional é inequívoca: a Olist possui dois problemas de satisfação independentes, e a agenda logística, por mais urgente que seja, resolve apenas o menor deles em termos absolutos.

**4.7 Concentração de Risco no Vendedor: O Ponto Cego da Análise**

O modelo de marketplace da Olist introduz uma variável de risco que os dados agregados ocultam: a concentração de performance por vendedor. Em plataformas com esse modelo, é comum que 10% a 20% dos vendedores sejam responsáveis por 60% a 80% dos problemas de SLA e satisfação. A base de dados disponível não permite realizar esse corte com precisão, o que representa a principal limitação analítica deste projeto.

A relevância desse gap é direta: se os 19% de taxa de atraso de março de 2018 são causados por um grupo específico de vendedores com baixa capacidade operacional, a solução é cirúrgica, suspensão, penalização ou suporte a esses vendedores. Se o problema é distribuído uniformemente entre toda a base de sellers, a solução é estrutural e cara. Sem essa análise, qualquer recomendação logística opera no escuro. A inclusão de dados de vendedor em futuras iterações deste projeto é a melhoria analítica de maior impacto potencial.

**5. Pontos de Atenção**

<table>
<tr><td>Risco Operacional — Atrasos com Alta Gravidade</td><td>6.534 pedidos entregues com atraso, sendo 2.862 com atraso superior a 8 dias. Em operações de e-commerce, atrasos acima de uma semana além do prometido têm correlação direta com churn de clientes e notas baixas. A distribuição uniforme entre as faixas de atraso indica ausência de um padrão sazonais,  o problema ocorre durante todo o ano.</td></tr>
<tr><td>Risco de Experiência — 14,7% de Clientes Insatisfeitos</td><td>14.575 avaliações com notas 1 ou 2 representam quase 1 em cada 7 clientes expressando insatisfação. Em uma plataforma marketplace, a reputação é ativo central. Clientes insatisfeitos tendem a não retornar, aumentam o custo de aquisição (CAC) para repor a base e podem gerar impacto negativo em canais digitais (redes sociais, reviews públicos).</td></tr>
<tr><td>Risco Financeiro - Frete Elevado no Norte e Nordeste</td><td>O custo de frete em estados como Paraíba e Roraima supera R$ 48,00 por pedido, 182% acima da média de São Paulo. Isso pode estar suprimindo o volume de pedidos nessas regiões e reduzindo a conversão de visitantes em compradores. A análise indica que, mesmo assim, o ticket médio nessas regiões é significativamente mais alto, o que valida o potencial latente desse mercado.</td></tr>
<tr><td>Limitação dos Dados - 2.963 Pedidos Não Entregues</td><td>A base não permite identificar com precisão a causa dos pedidos sem entrega registrada. Podem representar cancelamentos por inadimplência, devoluções, falhas de transportadora ou pedidos fraudulentos. Sem essa classificação, é impossível saber se são pedidos recuperáveis ou perdas definitivas, o que impede uma resposta estratégica adequada.</td></tr>
</table>

**6. Recomendações Estratégicas**

**6.1 Logística: Atacar Picos Sazonais e Rotas Críticas (Prioridade Alta)**

A revisão da operação logística deve começar pelos dois vetores identificados: sazonalidade e concentração regional. Em novembro de 2017, a taxa de atraso atingiu 12,4%; em março de 2018, 19%, três vezes a média anual. Não é possível resolver esse problema com monitoramento reativo; é necessário provisionar capacidade logística antecipada para períodos de alto volume.

Regionalmente, Alagoas (21,4% de atraso), Maranhão (17,4%) e Sergipe (15,2%) demandam atenção imediata: são os estados com maior taxa de atraso e maior ticket médio, o que amplifica o impacto financeiro de cada entrega frustrada.

Impacto estimado: Os 1.225 pedidos com atraso crítico (&gt;15 dias) envolvem R$ 222.962 em receita e um churn estimado de 490 clientes (assumindo 40% de abandono pós-experiência crítica). A cada compra futura perdida desses 490 clientes, a operação deixa de capturar aproximadamente R$ 78.300 em receita incremental, apenas na próxima transação. Reduzir a taxa de atraso crítico de 1,3% para 0,5% representaria retenção de ~770 pedidos e R$47.000 em receita em risco, além do efeito positivo indireto sobre o NPS da plataforma.

Ações: (1) Mapear os pedidos atrasados por transportadora e rota para os meses de maior incidência. (2) Negociar SLAs com penalidade contratual para parceiros com taxa de atraso acima de 10%. (3) Implementar alerta automático para clientes com risco de atraso estimado, com comunicação proativa antes da data prometida.

**6.2 Experiência do Cliente: Duas Agendas, Não Uma (Prioridade Alta)**

A análise de cruzamento pedidos × avaliações revela que a insatisfação tem duas origens independentes: pedidos atrasados (27,4% das notas ruins) e pedidos entregues no prazo com problema de outra natureza (57,3% das notas ruins). Tratar ambas como uma única agenda logística é um erro estratégico.

Agenda 1 - Logística (27,4%): Implementar cupom automático de R$20 a R$30 para pedidos com atraso confirmado acima de 5 dias. Com 6.534 pedidos atrasados e uma estimativa de uso de 60% dos cupons, o custo seria ~R$117.000. O retorno esperado em retenção (evitar churn de 30% dos afetados) resulta em ~1.960 clientes mantidos e aproximadamente R$313.000 em receita futura preservada, ROI estimado de 2,7x.

Agenda 2 - Produto/Vendedor (57,3%): Os 8.346 clientes com nota ruim em pedidos entregues no prazo indicam problemas que a logística não resolve. A análise dos comentários dessas avaliações deve ser o próximo passo analítico para identificar se o problema está na qualidade dos produtos, na comunicação do vendedor ou na experiência de pós-venda. Sem esse diagnóstico, qualquer ação de recuperação nesse segmento operará sem base.

**6.3 Expansão Regional: Oportunidade de Alto Retorno, Execução Cirúrgica (Prioridade Média-Alta)**

O Norte e Nordeste apresentam o maior ticket médio da base (acima de R$210) com o maior custo de frete e a maior taxa de atraso. A equação atual é: cliente de maior valor, experiência pior. Isso cria uma oportunidade direta, se a experiência melhorar, o volume pode crescer sem necessidade de descontos agressivos, pois o cliente já demonstra disposição de pagar mais.

Estimativa de oportunidade: Se a participação do Norte/Nordeste crescer de ~10% para 15% do total de pedidos (aproximadamente 5.000 pedidos adicionais no horizonte do dataset), com ticket médio de R$210, o GMV incremental seria de R$1.050.000, sem nenhuma mudança na base de clientes atual do Sudeste.

Ações: (1) Priorizar negociação com transportadoras locais para as rotas AL, MA, SE e CE. (2) Testar frete subsidiado de R$15 para pedidos acima de R$150 nessas regiões, o ticket médio elevado absorve parcialmente o subsídio. (3) Priorizar categorias de produto com alto giro e peso reduzido, que minimizam o custo base de frete.

**6.4 Financeiro: Investigar Não Entregas e Diversificar Pagamento (Prioridade Média)**

Os 2.963 pedidos não entregues representam 3% do total e têm avaliação média de 1,76,  a mais baixa de qualquer categoria. A classificação por causa (cancelamento, fraude, falha de transportadora, devolução) deve ser implementada no pipeline de dados como campo padrão, não como análise ad hoc. Sem essa categorização, é impossível saber se são perdas definitivas ou pedidos recuperáveis com ação comercial.

A diversificação de canais de pagamento deve considerar o cenário atual de mercado: o Pix, inexistente no período dos dados (2016-2018), hoje responde por grande parte das transações de e-commerce no Brasil. A ausência de Pix na plataforma no período analisado não é uma crítica,  é um contexto histórico que explica a relevância de 20% do boleto na base.

**6.5 Inteligência de Dados: Priorizar Granularidade por Vendedor (Prioridade Alta)**

A limitação analítica mais crítica deste projeto é a ausência de análise por vendedor. Em uma próxima versão, o cruzamento entre seller_id, taxa de atraso, ticket médio e avaliação do cliente permitiria identificar os vendedores de alta performance (baixo atraso, alto ticket, nota alta) e os de alto risco (atraso elevado, notas baixas). Essa análise transforma o diagnóstico de "problema estrutural de logística" em "lista de ações por vendedor", passando de estratégia para execução.

A migração para uma ferramenta de BI interativa (Power BI ou Tableau) é recomendada para o longo prazo, com dashboard monitorando em tempo real: taxa de atraso por mês, avaliação média por vendedor e taxa de entrega por região.

**7. Conclusão Executiva**

A Olist opera em crescimento real e acelerado: mais de 100 mil pedidos em 24 meses, crescimento de 135% no volume e taxa de entrega de 97%. Esses números validam o modelo de negócio. O problema não está na escala, está no que a escala revela.

Dois diagnósticos emergem desta análise e devem orientar a agenda executiva dos próximos 12 meses:

Diagnóstico 1 - O modelo logístico não escala junto com o volume. Em meses de alto crescimento, a taxa de atraso triplica. Isso não é sazonalidade gerenciável: é um limite de capacidade que se torna mais visível à medida que a plataforma cresce. Continuar investindo em aquisição de clientes sem resolver esse gargalo é antecipar ondas de insatisfação proporcionais ao crescimento.

Diagnóstico 2 - A insatisfação tem raízes além da logística. 57% das avaliações negativas vêm de pedidos entregues no prazo. Isso indica que produto, vendedor ou experiência pós-venda são responsáveis por mais da metade da insatisfação, e esse problema está completamente fora do radar logístico. Sem investigar os comentários dessas avaliações, qualquer iniciativa de melhoria de satisfação trabalhará com menos da metade do diagnóstico.

**Decisão Mínima Viável**

Se apenas uma ação puder ser tomada nos próximos 30 dias, ela deve ser: cruzar as 8.346 avaliações negativas de pedidos entregues no prazo com os comentários dos clientes e identificar as 3 principais causas de insatisfação não logística. Esse levantamento custa tempo analítico, não investimento financeiro, e tem o potencial de redirecionar toda a agenda de qualidade da plataforma para onde o impacto real está concentrado. É o ponto de alavanca de maior retorno sobre esforço desta análise.

**Direcionamento por Horizonte**

No curto prazo (0-3 meses): diagnosticar as causas de insatisfação não logística; implementar alerta proativo para pedidos em risco de atraso nas regiões críticas (AL, MA, SE, CE); e mapear os períodos sazonais de pico para provisionar capacidade logística antecipada.

No médio prazo (3-9 meses): implementar o programa de recuperação de clientes com atraso crítico; testar frete subsidiado no Norte/Nordeste para pedidos acima de R$150; e estruturar a análise por vendedor como componente padrão do pipeline de dados.

No longo prazo (9+ meses): migrar o monitoramento para BI interativo com atualização automática; implementar segmentação RFM da base de clientes; e revisar os SLAs contratuais com transportadoras baseado nos dados de performance regional coletados.

*Relatório elaborado com base em dados públicos do Brazilian E-Commerce Public Dataset by*

*Olist (Kaggle). Análise original desenvolvida no Microsoft Excel com Power Query, Tabelas Dinâmicas e indicadores KPI.*
