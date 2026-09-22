## Metadados

| Nome | RGM |
|---|---|
| Guilherme da Silva Ferreira Batista | 47302518 |
| Guilherme Petrucelli Domingos | 47270161 |
| Jaime Luiz de Oliveira Neto | 47336951 |
| Matheus Montagner | 47209470 |
| Vinicius Marques de Melo | 47213426 |

---

## 1. Caracterização da Organização

- **Nome e natureza da organização:** Contrasti Bolsas e Acessórios Ltda - Fabricação de Bolsas. O questionário usado como base trata de uma "Indústria e Comércio de Bolsas de Couro" — uma empresa com fins lucrativos que fabrica e vende bolsas de couro (produção própria + venda direta e por canais diversos).
- **Contexto e porte:** com fins lucrativos; opera simultaneamente como indústria e comércio (venda em loja física, e-commerce, WhatsApp e representantes externos). O uso de facções terceirizadas e o controle de aproveitamento de couro por corte sugerem uma operação de pequeno a médio porte, com produção sob encomenda/lote (não em larga escala industrial). Volume médio de pedidos por mês entre 50 a 70, aumentando nas datas comemorativas (Dia das Mães e Natal).
- **Problemas e necessidades identificados:** o levantamento de requisitos aponta processos hoje prováveis de estarem descentralizados/manuais: controle de estoque de insumos (couro, ferragens, zíperes) sem rastreabilidade de lote; ausência de regra formal de crédito para vendas a prazo; cálculo de custo/preço de venda não padronizado (ficha técnica); acompanhamento de produção sem visibilidade de status; e falta de integração entre vendas, estoque e financeiro (títulos a pagar/receber gerados manualmente).
- **Justificativa da escolha:** Uma empresa que a gente sabia que ia ter acesso fácil e que consideramos de porte médio, não deixando nem tão simples e nem tão complicado o nosso trabalho.
- **Evidências da organização:** Endereço: Rua Alpiste, 116 - Jd. Eliane - São Paulo - SP. Contato na empresa: Osmar Lingiardi, telefone para contato: 11 97334-4846, email: osmar@specia.com.br.

## 2. Processos de Negócio

**Principais processos mapeados** (extraídos do levantamento de requisitos, um por bloco do questionário):

1. **Cadastro e gestão de clientes** — cadastro com múltiplos endereços, perfil de preço (varejo/atacado) e aprovação de crédito.
2. **Cadastro e gestão de fornecedores** — dados do fornecedor, categoria de insumo e histórico de compras/avaliação.
3. **Ficha técnica e cadastro de produtos** — modelo → variação (SKU) → lista de materiais (BOM) com cálculo de custo.
4. **Gestão de estoque e compras** — entrada de insumo (XML de NF-e ou manual), controle por unidade de medida, estoque mínimo e baixa automática.
5. **Ordem de produção e chão de fábrica** — abertura de OP, execução por etapa (Modelagem → Corte → Preparação/Colagem → Costura → Montagem/Ferragens → Acabamento/Revisão → Embalagem), com Kanban e controle de aproveitamento de couro.
6. **Vendas, pedidos e faturamento** — pedido multicanal, forma de pagamento, comissão e emissão de NF-e/NFC-e.
7. **Expedição e pós-venda** — picking & packing, integração logística e garantia (RMA).
8. **Gestão financeira e relatórios gerenciais** — Contas a Pagar/Receber automáticos e indicadores (Curva ABC, DRE, fluxo de caixa).

---

## 3. Requisitos do Sistema

### 3.1 Requisitos Funcionais

| # | O sistema deve permitir... |
|---|---|
| RF01 | Cadastrar clientes com razão social/nome, CNPJ/CPF, inscrição estadual (quando lojista), e-mail para NF-e, telefone/WhatsApp e comprador responsável. |
| RF02 | Registrar múltiplos endereços por cliente (matriz, entrega, cobrança). |
| RF03 | Diferenciar clientes por perfil (Varejo Final × Atacado/Lojista) com regras de preço distintas. |
| RF04 | Consultar restrição de CPF/CNPJ e aplicar limite de crédito para boleto, exigindo aprovação financeira para novos lojistas. |
| RF05 | Cadastrar fornecedores com categoria de insumo, contato do vendedor e prazo médio de entrega. |
| RF06 | Registrar entrada de insumos por lote, vinculando fornecedor, cor/tonalidade e preço pago (rastreabilidade). |
| RF07 | Manter histórico de compras e avaliação por fornecedor (preço, variação, pontualidade de entrega). |
| RF08 | Cadastrar produtos por modelo e variação (SKU), combinando cor, tipo de couro e tipo de ferragem. |
| RF09 | Manter a Ficha Técnica (BOM) de cada SKU, com a quantidade de cada insumo necessária. |
| RF10 | Calcular automaticamente o custo total e sugerir preço de venda a partir da Ficha Técnica, mão de obra e markup. |
| RF11 | Registrar entrada de matéria-prima via XML de NF-e ou digitação manual. |
| RF12 | Controlar o estoque de insumos nas unidades apropriadas (dm²/m², unidade/metro, kg/L), com estoque mínimo e alerta automático. |
| RF13 | Baixar automaticamente o estoque de insumos na abertura da Ordem de Produção. |
| RF14 | Abrir Ordens de Produção vinculadas a um SKU e acompanhar o status em painel Kanban (Aguardando/Em Corte/Em Costura/Finalizado). |
| RF15 | Registrar, por etapa de produção, o artesão/facção responsável e o percentual de aproveitamento/perda de couro na etapa de Corte. |
| RF16 | Registrar pedidos de venda por canal (loja física, e-commerce, WhatsApp, representante), com forma de pagamento e parcelamento. |
| RF17 | Calcular comissão de vendedores/representantes sobre pedidos faturados, com percentuais diferenciados por canal. |
| RF18 | Emitir NF-e (venda mercantil) e NFC-e (cupom varejo), nativamente ou por integração. |
| RF19 | Controlar separação, conferência (código de barras) e embalagem (checklist + dust bag) antes do envio. |
| RF20 | Integrar com serviços de logística (Correios, Melhor Envio, transportadoras parceiras) para etiquetas e rastreio. |
| RF21 | Registrar trocas, devoluções e garantias (RMA) vinculadas ao item efetivamente entregue. |
| RF22 | Gerar lançamentos automáticos de Contas a Receber (vendas faturadas) e Contas a Pagar (compras). |
| RF23 | Gerar relatórios gerenciais: Curva ABC, margem de lucro por modelo, DRE gerencial, fluxo de caixa previsto × realizado, estoque parado. |
| RF24 | Restringir o acesso por perfil de usuário (Vendedor, Gerente de Produção, Financeiro, Administrador). |

### 3.2 Requisitos Não Funcionais

| # | Característica de qualidade |
|---|---|
| RNF01 | **Desempenho:** consultas de estoque e ficha técnica devem responder rápido o suficiente para não atrasar a abertura de uma Ordem de Produção. |
| RNF02 | **Segurança:** controle de acesso por perfil (RF24) e proteção de dados pessoais de clientes/fornecedores (LGPD). |
| RNF03 | **Disponibilidade:** o módulo de estoque/OP precisa estar disponível no horário de produção, já que a baixa de insumo é automática e bloqueante. |
| RNF04 | **Usabilidade:** o painel Kanban de produção deve ser operável pelos artesãos/facções sem treinamento extenso. |
| RNF05 | **Integridade transacional:** baixa de estoque, geração de título financeiro e emissão de nota fiscal devem ocorrer de forma atômica (tudo ou nada). |
| RNF06 | **Auditabilidade:** histórico de preços por fornecedor e de status de OP deve ser preservado para consultas gerenciais futuras. |

---

## 4. Regras de Negócio

- **Regras operacionais:**
  - Inscrição Estadual só é obrigatória quando o perfil do cliente é Atacado/Lojista.
  - Toda variação de produto (SKU) pertence a exatamente um modelo; um modelo pode ter uma ou várias variações.
  - A baixa de estoque de insumo ocorre automaticamente na abertura da Ordem de Produção (reserva de material necessário).
  - Todo insumo recebido é registrado com lote e fornecedor de origem, para garantir rastreabilidade de cor/textura entre peças da mesma coleção.
  - Comissão padrão de vendedor é de 5% sobre pedidos faturados, com percentual diferenciado entre canal de atacado e varejo.
  - Um chamado de garantia (RMA) está sempre associado a um item de pedido específico, nunca ao pedido inteiro.
  - Um pedido só é expedido depois de checklist de saída e conferência dos itens (leitura de código de barras).

- **Restrições organizacionais:**
  - Exigência legal de emissão de NF-e (venda mercantil) ou NFC-e (cupom fiscal varejo), conforme o canal de venda.
  - Dados pessoais de clientes e fornecedores (nome, CPF/CNPJ, contato) exigem tratamento conforme a LGPD.
  - O prazo médio de entrega de cada fornecedor impacta diretamente o planejamento da produção (lead time de insumo).
  - O pagamento de artesãos/facções terceirizadas depende do registro fiel de qual etapa cada um executou — sem esse registro não há como calcular a remuneração por produção.

---

## 5. Dicionário de Dados Conceitual (Preliminar)

> Formato oficial da disciplina: por entidade, `Atributo | Descrição | Regra de negócio associada`. As 21 entidades abaixo cobrem as 8 seções do levantamento de requisitos.

### CLIENTE
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_cliente | Identificador interno do cliente | Chave primária |
| razao_social_nome | Razão social (PJ) ou nome completo (PF) | Obrigatório |
| cnpj_cpf | Documento fiscal do cliente | Usado na consulta de restrição de crédito |
| inscricao_estadual | Registro estadual do cliente | Obrigatório apenas se perfil = Atacado/Lojista |
| email_nfe | E-mail para envio da nota fiscal eletrônica | Obrigatório; é o canal de entrega da NF-e/NFC-e ao cliente |
| telefone_whatsapp | Contato principal | Usado também como canal de venda quando o pedido é feito via WhatsApp |
| nome_comprador_responsavel | Pessoa de contato para compras | Preenchido principalmente para clientes Atacado/Lojista |
| perfil_cliente | Classificação comercial | Domínio: Varejo Final / Atacado-Lojista — define a régua de preço |
| limite_credito | Teto de faturamento via boleto | Referência observada: R$10.000,00 |
| status_aprovacao_financeira | Situação de aprovação de crédito | Domínio: Pendente / Aprovado / Reprovado — obrigatório para novo lojista |

### ENDERECO_CLIENTE *(entidade fraca — depende de CLIENTE)*
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_endereco | Identificador do endereço | Chave primária |
| cod_cliente | Cliente ao qual o endereço pertence | Chave estrangeira → CLIENTE |
| tipo_endereco | Finalidade do endereço | Domínio: Matriz / Entrega / Cobrança |
| logradouro | Rua/avenida e número | Obrigatório |
| cidade | Cidade | Obrigatório |
| uf | Unidade federativa | Obrigatório; sigla de 2 letras (ex.: SP) |
| cep | Código postal | Obrigatório; usado no cálculo de frete e na etiqueta de expedição |

### FORNECEDOR
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_fornecedor | Identificador do fornecedor | Chave primária |
| razao_social | Nome empresarial do fornecedor | Obrigatório. Nome utilizado nas compras e histórico |
| cnpj | Documento fiscal | Obrigatório e único. Dado protegido pela LGPD |
| inscricao_estadual | Registro estadual | Opcional |
| categoria_insumo | Tipo de insumo fornecido | Domínio: Curtume/Couro, Ferragens/Fivelas, Zíperes/Aviamentos, Embalagens/Caixas |
| contato_vendedor | Pessoa de contato comercial | Nome da pessoa que atende a empresa |
| prazo_medio_entrega_dias | Lead time médio de entrega | Usado no planejamento de produção para compras antecipadas |

### INSUMO
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_insumo | Identificador do insumo | Chave primária |
| descricao_insumo | Nome do insumo (ex.: Couro Bovino Caramelo) | Obrigatório |
| categoria_insumo | Categoria do insumo | Utiliza o mesmo domínio de categoria_insumo do FORNECEDOR |
| unidade_medida | Unidade de controle de estoque | Domínio: dm², m², unidade, metro, kg, litro |
| estoque_minimo | Saldo mínimo configurável | Dispara alerta automático de recompra |
| saldo_estoque_atual | Saldo atual em estoque | Atualizado a cada entrada/baixa |

### LOTE_INSUMO *(entidade fraca — depende de INSUMO, FORNECEDOR e COMPRA)*
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_lote | Identificador do lote | Chave primária |
| cod_insumo | Insumo recebido | Chave estrangeira → INSUMO |
| cod_fornecedor | Fornecedor de origem | Chave estrangeira → FORNECEDOR |
| cod_compra | Compra que originou o lote | Chave estrangeira → COMPRA |
| numero_lote | Identificação do lote (ex.: 2026-A) | Garante rastreabilidade de cor/textura |
| data_recebimento | Data de entrada no estoque | Dispara a atualização do saldo_estoque_atual do insumo |
| quantidade_recebida | Quantidade recebida | Deve estar na mesma unidade_medida cadastrada no INSUMO |
| cor_tonalidade | Cor/tonalidade do insumo (couro) | Preenchida principalmente para couro, garantindo uniformidade na coleção |
| preco_pago | Preço pago nesse recebimento | Alimenta o histórico de preço por fornecedor |

### COMPRA
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_compra | Identificador da compra | Chave primária |
| cod_fornecedor | Fornecedor da compra | Chave estrangeira → FORNECEDOR |
| numero_compra | Número/identificação da compra | Sequencial, gerado pelo sistema |
| data_compra | Data da compra | Usada no cálculo do prazo médio de entrega do fornecedor |
| forma_entrada | Origem do lançamento | Domínio: XML de NF-e / Manual |
| valor_total | Valor total da compra | Soma dos lotes de insumo; origina o lançamento em CONTAS_PAGAR |
| taxa_pontualidade_entrega | Percentual de pontualidade apurado | Alimenta a avaliação histórica do fornecedor |

### CONTAS_PAGAR
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_titulo_pagar | Identificador do título | Chave primária |
| cod_compra | Compra que gerou o título | Chave estrangeira → COMPRA |
| valor | Valor do título | Herdado do valor_total da COMPRA que originou o título |
| data_vencimento | Data de vencimento | Define o prazo limite para pagamento ao fornecedor |
| data_pagamento | Data em que foi pago | Nulo até a baixa |
| status_pagamento | Situação do título | Domínio: Aberto / Pago / Atrasado |

### MODELO_PRODUTO
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_modelo | Identificador do modelo | Chave primária |
| nome_modelo | Nome do modelo (ex.: Bolsa Tote) | Obrigatório |
| descricao | Descrição do modelo | Texto livre, usado em catálogo e material de venda |

### VARIACAO_PRODUTO *(SKU)*
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_sku | Identificador da variação | Chave primária |
| cod_modelo | Modelo ao qual pertence | Chave estrangeira → MODELO_PRODUTO |
| codigo_sku | Código comercial (ex.: SKU-BOLSA-TOTE-CAR-UNI) | Gerado a partir da combinação modelo + cor + tipo de couro/ferragem |
| cor | Cor da variação | Obrigatório; compõe o codigo_sku |
| tipo_couro | Tipo de couro utilizado | Ex.: Bovino Vaqueta, Mestiço |
| tipo_ferragem | Tipo de ferragem utilizada | Ex.: Dourada, Prata, Escovada |
| preco_venda_sugerido | Preço de venda calculado | (insumos + mão de obra + rateio) × markup |
| tempo_estimado_mao_obra_min | Tempo estimado de produção | Referência para o planejamento de capacidade e prazo de entrega |

### ITEM_FICHA_TECNICA *(entidade associativa — BOM)*
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_sku | Variação de produto | Chave primária composta + FK → VARIACAO_PRODUTO |
| cod_insumo | Insumo utilizado | Chave primária composta + FK → INSUMO |
| quantidade_necessaria | Quantidade do insumo por unidade produzida | Ex.: dm² de couro, unidades de zíper, metros de linha |
| unidade_medida_item | Unidade da quantidade necessária | Deve ser a mesma unidade_medida cadastrada no INSUMO correspondente |

### ARTESAO_FACCAO
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_artesao | Identificador do artesão/facção | Chave primária |
| nome | Nome do artesão ou razão social da facção | Obrigatório |
| tipo_vinculo | Natureza do vínculo | Domínio: Artesão Interno / Facção Terceirizada |
| telefone | Contato | Usado para agendamento e acompanhamento das etapas de produção |

### ETAPA_PRODUCAO *(catálogo)*
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_etapa | Identificador da etapa | Chave primária |
| nome_etapa | Nome da etapa | Modelagem, Corte, Preparação/Colagem, Costura, Montagem/Ferragens, Acabamento/Revisão, Embalagem |
| ordem_sequencial | Posição da etapa no fluxo produtivo | Define a sequência do Kanban |

### ORDEM_PRODUCAO
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_op | Identificador da ordem de produção | Chave primária |
| cod_sku | Variação de produto a ser produzida | Chave estrangeira → VARIACAO_PRODUTO |
| numero_op | Número da OP | Sequencial, usado como referência no painel Kanban |
| data_abertura | Data de abertura | Dispara a reserva/baixa automática de insumos |
| quantidade_produzir | Quantidade a produzir | Número inteiro maior que zero |
| status_kanban | Status atual da OP | Domínio: Aguardando / Em Corte / Em Costura / Finalizado |

### EXECUCAO_ETAPA *(entidade associativa)*
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_execucao | Identificador da execução | Chave primária |
| cod_op | Ordem de produção | Chave estrangeira → ORDEM_PRODUCAO |
| cod_etapa | Etapa executada | Chave estrangeira → ETAPA_PRODUCAO |
| cod_artesao | Responsável pela execução | Chave estrangeira → ARTESAO_FACCAO — base do pagamento por produção |
| data_inicio | Início da execução | Marca o começo da etapa para o artesão/facção responsável |
| data_fim | Fim da execução | Junto com data_inicio, mede a duração da etapa e alimenta o pagamento por produção |
| percentual_aproveitamento | % de aproveitamento do couro | Aplicável à etapa de Corte |
| percentual_perda | % de perda/retalho | Aplicável à etapa de Corte |

### USUARIO
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_usuario | Identificador do usuário | Chave primária |
| nome | Nome do usuário | Obrigatório |
| login | Login de acesso | Obrigatório e único |
| senha_hash | Senha (armazenada com hash) | Nunca armazenada em texto puro; usada apenas para autenticação |
| perfil_acesso | Perfil de acesso ao sistema | Domínio: Vendedor / Gerente de Produção / Financeiro / Administrador |
| tipo_vendedor | Natureza do vínculo comercial | Domínio: Interno / Representante Externo (nulo se não for vendedor) |
| percentual_comissao | Percentual de comissão | Padrão 5%, diferenciado por atacado/varejo |

### PEDIDO_VENDA
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_pedido | Identificador do pedido | Chave primária |
| cod_cliente | Cliente do pedido | Chave estrangeira → CLIENTE |
| cod_usuario | Vendedor/representante responsável | Chave estrangeira → USUARIO |
| numero_pedido | Número do pedido | Sequencial, exibido ao cliente e usado na NF-e/NFC-e |
| data_pedido | Data do pedido | Obrigatória; ponto de partida do fluxo de faturamento e expedição |
| canal_venda | Canal de venda | Domínio: Loja Física / E-commerce / WhatsApp / Representante |
| forma_pagamento | Forma de pagamento | Domínio: PIX / Cartão / Boleto |
| condicao_parcelamento | Condição de parcelamento | Ex.: 30/60/90 dias (boleto atacado) |
| status_pedido | Situação do pedido | Reflete o andamento do pedido (ex.: aberto, em produção, faturado, expedido) |

### ITEM_PEDIDO *(entidade associativa)*
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_item_pedido | Identificador do item | Chave primária |
| cod_pedido | Pedido ao qual pertence | Chave estrangeira → PEDIDO_VENDA |
| cod_sku | Produto vendido | Chave estrangeira → VARIACAO_PRODUTO |
| quantidade | Quantidade vendida | Número inteiro maior que zero |
| preco_unitario_praticado | Preço unitário praticado | Fica congelado no momento da venda: reajustes posteriores não alteram pedidos antigos |
| desconto | Desconto aplicado | Opcional. Não pode ser maior que quantidade × preco_unitario_praticado do item |

### NOTA_FISCAL
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_nf | Identificador da nota | Chave primária |
| cod_pedido | Pedido faturado | Chave estrangeira → PEDIDO_VENDA |
| numero_nf | Número da nota | Sequencial, atribuído pelo sistema emissor/SEFAZ |
| tipo_nf | Tipo de documento fiscal | Domínio: NF-e (mercantil) / NFC-e (cupom varejo) |
| data_emissao | Data de emissão | Referência legal da venda |
| valor_total | Valor total da nota | Igual ao valor_total do PEDIDO_VENDA faturado |

### CONTAS_RECEBER
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_titulo_receber | Identificador do título | Chave primária |
| cod_pedido | Pedido que gerou o título | Chave estrangeira → PEDIDO_VENDA |
| valor | Valor do título | Herdado do valor_total do PEDIDO_VENDA |
| data_vencimento | Data de vencimento | No boleto, segue a condicao_parcelamento do pedido |
| data_recebimento | Data em que foi recebido | Nulo até a baixa |
| status_recebimento | Situação do título | Domínio: Aberto / Recebido / Atrasado |

### EXPEDICAO *(1:1 com pedido)*
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_expedicao | Identificador da expedição | Chave primária |
| cod_pedido | Pedido expedido | Chave estrangeira → PEDIDO_VENDA |
| data_envio | Data de envio | Data em que o pedido foi efetivamente despachado |
| transportadora | Transportadora utilizada | Ex.: Correios, Melhor Envio, parceira |
| codigo_rastreio | Código de rastreamento | Fornecido pela transportadora para acompanhamento da entrega |
| checklist_conferencia | Conferência realizada | Via leitura de código de barras |
| dust_bag_incluso | Saquinho protetor incluído | Indica se o item padrão do checklist de embalagem foi incluído |

### RMA_GARANTIA
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_rma | Identificador do chamado | Chave primária |
| cod_item_pedido | Item associado à garantia | Chave estrangeira → ITEM_PEDIDO (não ao pedido inteiro) |
| data_abertura | Data de abertura do chamado | Obrigatória; inicia o prazo de análise do RMA |
| motivo_defeito | Motivo relatado | Descrição do defeito ou motivo informado pelo cliente |
| status_rma | Situação do chamado | Domínio: Aberto / Em Reparo / Devolvido |
| data_devolucao | Data de devolução ao cliente | Preenchida quando o item reparado/trocado retorna ao cliente |

---

## 6. Modelagem Conceitual (Entidades, Atributos, Relacionamentos)

- **Entidades reconhecidas (21 no total):** as 21 entidades listadas na Seção 5, agrupadas nos 6 blocos de negócio identificados no levantamento — Clientes; Fornecedores/Insumos/Compras; Produtos/Ficha Técnica; Produção; Vendas/Faturamento; Expedição/Financeiro/Pós-venda. Duas são **entidades fracas** (ENDERECO_CLIENTE, LOTE_INSUMO — só existem em função de outra entidade) e três são **entidades associativas** que resolvem relacionamentos N:N com atributos próprios (ITEM_FICHA_TECNICA, EXECUCAO_ETAPA, ITEM_PEDIDO).
- **Atributos e classificações:** detalhados por entidade na Seção 5, com chave primária, chaves estrangeiras e domínios de valor explícitos (ex.: `perfil_cliente`, `status_kanban`, `tipo_nf`).
- **Relacionamentos pertinentes (22 no total):**

| # | Entidade A | Card. A | Verbo | Card. B | Entidade B |
|---|---|---|---|---|---|
| 01 | CLIENTE | (1,1) | possui | (0,N) | ENDERECO_CLIENTE |
| 02 | FORNECEDOR | (1,1) | fornece | (0,N) | LOTE_INSUMO |
| 03 | INSUMO | (1,1) | é recebido como | (0,N) | LOTE_INSUMO |
| 04 | COMPRA | (1,1) | gera | (0,N) | LOTE_INSUMO |
| 05 | FORNECEDOR | (1,1) | recebe pedido de | (0,N) | COMPRA |
| 06 | COMPRA | (1,1) | gera | (0,N) | CONTAS_PAGAR |
| 07 | MODELO_PRODUTO | (1,1) | possui | (1,N) | VARIACAO_PRODUTO |
| 08 | VARIACAO_PRODUTO | (1,1) | é detalhada em | (0,N) | ITEM_FICHA_TECNICA |
| 09 | INSUMO | (1,1) | compõe | (0,N) | ITEM_FICHA_TECNICA |
| 10 | VARIACAO_PRODUTO | (1,1) | é produzida em | (0,N) | ORDEM_PRODUCAO |
| 11 | ORDEM_PRODUCAO | (1,1) | se desdobra em | (0,N) | EXECUCAO_ETAPA |
| 12 | ETAPA_PRODUCAO | (1,1) | classifica | (0,N) | EXECUCAO_ETAPA |
| 13 | ARTESAO_FACCAO | (1,1) | realiza | (0,N) | EXECUCAO_ETAPA |
| 14 | LOTE_INSUMO | (0,N) | é consumido em | (0,N) | ORDEM_PRODUCAO |
| 15 | CLIENTE | (1,1) | realiza | (0,N) | PEDIDO_VENDA |
| 16 | USUARIO | (1,1) | atende | (0,N) | PEDIDO_VENDA |
| 17 | PEDIDO_VENDA | (1,1) | contém | (1,N) | ITEM_PEDIDO |
| 18 | VARIACAO_PRODUTO | (1,1) | é vendida em | (0,N) | ITEM_PEDIDO |
| 19 | PEDIDO_VENDA | (1,1) | gera | (0,N) | NOTA_FISCAL |
| 20 | PEDIDO_VENDA | (1,1) | gera | (0,N) | CONTAS_RECEBER |
| 21 | PEDIDO_VENDA | (1,1) | possui | (0,1) | EXPEDICAO |
| 22 | ITEM_PEDIDO | (1,1) | pode gerar | (0,N) | RMA_GARANTIA |

- **Restrições e políticas organizacionais aplicadas ao modelo:** o limite de crédito e a aprovação financeira (Seção 4) aparecem como atributos de CLIENTE que condicionam a cardinalidade opcional de PEDIDO_VENDA; a rastreabilidade de lote exigida pela organização motivou LOTE_INSUMO ser entidade própria (e não apenas um atributo de INSUMO); o pagamento por produção exigiu que EXECUCAO_ETAPA carregasse o artesão responsável em vez de um campo solto na Ordem de Produção.

---

## 7. Diagrama Entidade-Relacionamento (DER)

**Arquivos anexados:** `DER_Bolsas_Couro.png` (e `DER_Bolsas_Couro.svg`, versão vetorial da mesma imagem) — gerados a partir do modelo desta Seção 6.

O diagrama segue a notação de Chen usada pelo BrModeloWeb: entidades como **retângulos**, relacionamentos como **losangos** rotulados com o verbo, atributos como **elipses** presas à entidade (ou ao relacionamento, no único caso de atributo de relacionamento do modelo — `quantidade_consumida` em "consumido em"), com a **chave primária sublinhada**. Entidade fraca (ENDERECO_CLIENTE) e seu relacionamento identificador ("possui") aparecem com borda dupla. As cardinalidades (mín,máx) ficam junto de cada ponta da linha, exatamente como na tabela da Seção 6.

Uma diferença importante em relação ao dicionário da Seção 5: seguindo a notação conceitual pura, o diagrama **não mostra atributos de chave estrangeira** (ex.: `cod_cliente` dentro de ENDERECO_CLIENTE) — essa informação já está representada pela própria linha do relacionamento. As FKs voltam a aparecer no dicionário porque ali o objetivo é documentar a estrutura completa, inclusive o que se tornará chave estrangeira na Entrega 2.

O modelo já nasce pensando em escalabilidade: entidades como USUARIO e ETAPA_PRODUCAO são catálogos abertos (novos perfis/etapas não exigem redesenho), e o bloco financeiro (CONTAS_PAGAR/CONTAS_RECEBER) está desacoplado o suficiente para integrar com um módulo de BI na Entrega 2 sem alterar as entidades operacionais.

---

## 8. Justificativa Técnica

- **Por que 21 entidades e não menos:** cada entidade corresponde a um substantivo com identidade própria e ciclo de vida distinto no levantamento de requisitos (ex.: um LOTE_INSUMO não é apenas uma característica de INSUMO — ele tem data de recebimento, fornecedor e preço próprios, e precisa ser referenciado individualmente pela Ordem de Produção para rastreabilidade). Fundir entidades como CLIENTE e ENDERECO_CLIENTE em uma tabela só quebraria a regra explícita de múltiplos endereços por cliente (requisito 1.2 do levantamento).
- **Por que entidades fracas:** ENDERECO_CLIENTE e LOTE_INSUMO não têm existência independente — um endereço sem cliente ou um lote sem insumo/fornecedor não fazem sentido no domínio. Modelá-las como fracas (chave dependente) evita chaves substitutas artificiais e deixa a dependência explícita no diagrama.
- **Por que entidades associativas em vez de relacionamentos N:N "soltos":** ITEM_FICHA_TECNICA, EXECUCAO_ETAPA e ITEM_PEDIDO carregam atributos próprios (quantidade, percentual de aproveitamento, preço praticado) que não pertencem a nenhuma das duas entidades que conectam — a notação exige reificá-los como entidade para acomodar esses atributos e permitir que outras entidades (como RMA_GARANTIA) referenciem um item específico, e não o par inteiro.
- **Por que essas cardinalidades e não outras:** cada cardinalidade (0,N) reflete uma regra observada no levantamento (ex.: cliente pode ou não ter pedidos, então CLIENTE–PEDIDO_VENDA é (1,1)-(0,N)); já PEDIDO_VENDA–ITEM_PEDIDO é (1,N) do lado do item porque um pedido sem nenhum item não existe operacionalmente.
- **Alternativas descartadas:** cogitou-se tratar Endereço como atributo multivalorado simples de Cliente (sem entidade própria) — descartado porque endereço tem atributos compostos (logradouro/cidade/UF/CEP) e tipo (matriz/entrega/cobrança), o que exige estrutura própria. Cogitou-se também um único relacionamento genérico "Produto consome Insumo" sem reificação — descartado porque a quantidade necessária por insumo é informação de primeira classe para o cálculo de custo (RF10).

---

## 9. Anexo — Modelo Conceitual Simplificado (9 Entidades)

> Recorte simplificado do modelo completo de 21 entidades (Seções 5 a 7), cobrindo apenas o fluxo de custo de produção e vendas: do fornecedor de insumos até o pedido faturado ao cliente. Preparado como entrega alternativa/reduzida, no mesmo padrão de notação (Chen) e cardinalidade (mín,máx) do modelo completo.

**Arquivos anexados:** `Dicionario_Dados_9_Entidades.html` e `Dicionario_Dados_9_Entidades.pdf` (dicionário de dados, mesmo conteúdo em dois formatos) e `DER_Diagrama_9_Entidades.pdf` (diagrama).

### 9.1 Entidades e relacionamentos

9 entidades — FORNECEDOR, INSUMO, FICHA_TECNICA, PRODUTO, BOLSA, ACESSORIO, CLIENTE, PEDIDO e ITEM_PEDIDO — ligadas por 7 relacionamentos nomeados (`abastece`, `constitui`, `detalha-se em`, `integra`, `compreende`, `efetua`) mais a especialização Total e Disjunta de PRODUTO em BOLSA/ACESSORIO. FICHA_TECNICA e ITEM_PEDIDO são entidades associativas, identificadas respectivamente por PRODUTO+INSUMO e por PEDIDO+PRODUTO. 

### 9.2 Análise de consistência

- **Coerência dicionário ↔ diagrama:** todos os atributos do dicionário aparecem no DER e as cardinalidades batem entre as duas fontes. FKs corretamente omitidas do desenho conceitual (representadas pela linha do relacionamento).
- **N:N tratado corretamente:** FORNECEDOR–INSUMO é N:N (um insumo pode ter vários fornecedores) e o N:N entre PRODUTO e INSUMO é resolvido pela entidade associativa FICHA_TECNICA, com dois relacionamentos nomeados separadamente (`constitui` e `detalha-se em`).
- **Derivados sinalizados:** `custo_materia_prima`, `preco_tabela` e `valor_total` estão marcados como calculados, com fórmula explícita — evita modelar valor derivado como dado primário.
- **Especialização TD bem definida:** círculo "TD" com linha dupla; regra de disjunção explícita (produto-acessório não pode ter dados de bolsa).

**Pontos de atenção (limitações conhecidas do recorte, não erros estruturais):**
1. `categoria_insumo` é repetido em FORNECEDOR e INSUMO com o mesmo domínio de valores, mas sem relacionamento formal que imponha a correspondência — é regra de consistência a ser garantida em nível de aplicação, não pelo DER conceitual puro.
2. A cardinalidade FORNECEDOR (1,N) — INSUMO (0,N) permite insumo cadastrado sem nenhum fornecedor vinculado; plausível (insumo novo, ainda não comprado), mas vale validar com a organização.
3. `inscricao_estadual` obrigatória apenas quando `perfil_cliente = Atacado/Lojista` é uma regra condicional que a notação de Chen não expressa estruturalmente — está documentada em texto no dicionário, e deve virar constraint (CHECK) no modelo lógico.
4. `custo_unitario` em INSUMO guarda apenas o último preço de compra (sem histórico por lote, diferente do modelo de 21 entidades, onde vem de `preco_pago` em LOTE_INSUMO) — simplificação proposital e já documentada como fora de escopo.

### 9.3 Dicionário de Dados (9 Entidades)

#### FORNECEDOR
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| id_fornecedor | Identificador | Chave primária, gerada pelo sistema. É única e nunca é reutilizada. |
| razao social. | Nome empresarial | Obrigatório. É o nome que aparece nas compras e no histórico de avaliação do fornecedor. |
| cnpj | CNPJ | Obrigatório e único: não pode haver dois fornecedores com o mesmo CNPJ. Dado protegido pela LGPD. |
| inscricao estadual | Registro estadual | Opcional. Preenchida quando o fornecedor possui inscrição estadual. |
| email | E-mail comercial | Opcional. Canal de contato comercial com o fornecedor. |
| telefone | Telefone | Opcional. Contato direto com o fornecedor. |
| contato_vendedor | Vendedor de referência | Nome da pessoa que atende a empresa dentro do fornecedor. |
| categoria_insumo | Tipo de insumo vendido | Obrigatório. Aceita apenas: Curtume/Couro, Ferragens/Fivelas, Zíperes/Aviamentos ou Embalagens/Caixas. |
| prazo_medio_entrega_dias | Prazo de entrega (dias) | Número inteiro maior que zero. Impacta o planejamento da produção: quanto maior o prazo, mais cedo o insumo precisa ser comprado. |

#### INSUMO
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| id_insumo | Identificador | Chave primária, gerada pelo sistema. |
| nome_insumo | Nome do material | Obrigatório. Ex.: Couro Bovino Caramelo, zíper, fivela, forro. |
| categoria_insumo | Grupo do material | Usa o mesmo conjunto de categorias do fornecedor, o que permite saber quais fornecedores podem vender o insumo. |
| unidade_medida | Unidade de controle | Obrigatória. Aceita: dm², m², unidade, metro, kg ou litro. Estoque e ficha técnica usam sempre essa mesma unidade. |
| estoque_minimo | Saldo mínimo | Quando estoque_atual ficar igual ou abaixo do mínimo, o sistema gera alerta automático de recompra. |
| estoque_atual | Saldo em estoque | Nunca pode ser negativo. Aumenta a cada compra recebida e diminui quando o insumo é consumido na produção. |
| custo_unitario | Custo por unidade de medida | Maior que zero. Guarda o último custo de compra e é o valor usado no cálculo do custo de matéria-prima. No modelo de 21 entidades ele vem do preco_pago do lote. |

#### FICHA_TECNICA (associativa; identificada por PRODUTO + INSUMO)
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| quantidade_necessaria | Quantidade por peça | Maior que zero e na unidade_medida do insumo (ex.: dm² de couro, unidades de zíper). Cada insumo aparece uma única vez na ficha de um produto. |
| percentual_perda | Perda técnica no corte | Informada em fração, de 0 a 1 (10% = 0,10). Entra no custo como quantidade x (1 + perda), pois o couro perdido no corte também é pago. |

#### PRODUTO
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| id_produto | Identificador | Chave primária, gerada pelo sistema. É a mesma chave usada em BOLSA e ACESSORIO. |
| nome_modelo | Nome do modelo | Obrigatório. Ex.: Bolsa Tote. |
| custo_mao_obra | Mão de obra por peça | Valor informado, não calculado: representa corte e costura. É somado ao custo de matéria-prima na formação do preço. |
| markup | Multiplicador de margem | Valor informado, maior que 1 (ex.: 2,5 significa preço 2,5 vezes o custo). |
| custo_materia_prima (derivado) | Custo dos insumos | Não é digitado. Soma, para cada insumo da ficha, de quantidade x (1 + perda) x custo_unitario. |
| preco_tabela (derivado) | Preço de venda sugerido | Não é digitado. É (custo de matéria-prima + custo de mão de obra) x markup, recalculado sempre que um desses valores mudar. |

#### BOLSA (especialização)
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| tamanho | Dimensão da bolsa | Obrigatório para bolsa. Herda também todos os atributos de PRODUTO. |
| cor | Cor | Obrigatória para bolsa. |
| tipo_alca | Tipo de alça | Obrigatório para bolsa. |
| pecas_composicao (multivalorado) | Peças que formam a bolsa | Uma bolsa tem várias peças, no mínimo uma (Tampa, Frente, Costa, Fundo, Orla). |

#### ACESSORIO (especialização)
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| tipo_peca | Tipo do acessório | Obrigatório. Ex.: cinto, porta-cartões. Um produto que é acessório não pode ter dados de bolsa (especialização disjunta). |

#### CLIENTE
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| id_cliente | Identificador | Chave primária, gerada pelo sistema. |
| nome | Razão social ou nome | Obrigatório. Razão social quando pessoa juridica; nome completo quando pessoa física. |
| cpf_cnpj | CPF ou CNPJ | Obrigatório e único. É usado na consulta de restrição de crédito. Dado protegido pela LGPD. |
| inscricao_estadual | Registro estadual | Obrigatória somente quando perfil_cliente = Atacado/Lojista. Para Varejo Final fica vazia. |
| email | E-mail | Usado para enviar a NF-e ao cliente. |
| telefone | Telefone/WhatsApp | Contato principal do cliente. |
| nome_comprador_responsavel | Contato de compras | Pessoa que faz os pedidos em nome do cliente, principalmente no atacado. |
| perfil_cliente | Tipo de cliente | Aceita: Varejo Final ou Atacado/Lojista. Define a régua de preço aplicada e se a inscrição estadual é obrigatória. |
| limite credito | Teto de compra a prazo | Valor em R$ (referência observada: R$ 10.000,00). Vale só para pedidos em boleto: o valor do pedido não pode ultrapassar o limite. |
| status_aprovacao_financeira | Situação do crédito | Aceita: Pendente, Aprovado ou Reprovado. Todo novo lojista começa Pendente e só pode comprar em boleto depois de Aprovado. |

#### PEDIDO
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| id_pedido | Identificador | Chave primária, gerada pelo sistema. |
| data_pedido | Data do pedido | Obrigatória. Preenchida quando o pedido é registrado. |
| canal_venda | Canal de venda | Aceita: Loja Física, E-commerce, WhatsApp ou Representante. Define regras que dependem do canal, como comissão e tipo de nota fiscal. |
| forma_pagamento | Forma de pagamento | Aceita: PIX, Cartão ou Boleto. Boleto só é permitido se o cliente estiver com status Aprovado e o valor_total couber no limite credito. |
| condicao_parcelamento | Parcelas ou prazo | Depende da forma_pagamento. PIX: à vista. Cartão: número de parcelas (ex.: 3x). Boleto: prazos em dias (ex.: 30/60/90), usado no atacado. Valores de exemplo, a validar com a empresa. |
| status_pedido | Situação do pedido | Obrigatório. Muda conforme o pedido avança (ex.: aberto, faturado, expedido). Lista de valores a definir com a empresa. |
| valor_total (derivado) | Total do pedido | Não digitado. Soma, para cada item, de quantidade x preco_unitario_praticado - desconto. |

#### ITEM_PEDIDO (associativa; identificada por PEDIDO + PRODUTO)
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| quantidade | Unidades vendidas | Número inteiro maior que zero. Um produto aparece uma vez por pedido; para vender mais unidades, aumenta-se a quantidade. |
| preco_unitario_praticado | Preço na venda | Fica congelado no momento da venda: reajustes posteriores do preco_tabela não alteram pedidos antigos. Pode diferir do preço de tabela conforme o perfil do cliente. |
| desconto | Desconto do item (R$) | Opcional. Não pode ser maior que quantidade x preco_unitario_praticado do item. |

### 9.4 Uso de IA neste anexo

Este recorte de 9 entidades foi produzido em uma sessão de trabalho separada, a partir de dois PDFs já prontos (dicionário e diagrama) fornecidos ao Claude. A IA foi usada para: (1) revisar a consistência entre o dicionário e o diagrama (cardinalidades, atributos derivados, tratamento do N:N fornecedor–insumo, especialização TD) e apontar os pontos de atenção listados em 9.2; (2) converter o dicionário de dados também para HTML, preservando integralmente o conteúdo e a estrutura por entidade (o PDF original foi mantido junto, como formato alternativo); (3) redigir esta seção do README a partir dessa análise, mantendo o restante do documento (modelo de 21 entidades, Seções 1 a 8) inalterado. O diagrama foi mantido apenas em PDF, sem edição de conteúdo. Nenhum dado novo sobre a organização foi inventado; a análise é estrutural, sobre a modelagem já existente nos dois arquivos fornecidos.

---

## 10. Uso de Inteligência Artificial
*(documentação obrigatória)*

| Item | Registro |
|---|---|
| **Ferramenta e etapa** | Claude (Claude Code / Sonnet 5, Anthropic) — usado em quatro momentos: (1) leitura do questionário de requisitos e construção do modelo conceitual (entidades, atributos, relacionamentos, cardinalidades) e do dicionário de dados; (2) geração de um primeiro diagrama ER (notação "pé-de-galinha"); (3) preenchimento deste README a partir do esqueleto oficial da Entrega 1; (4) reconstrução do diagrama na notação de Chen (retângulo/losango/elipse) usada pelo BrModeloWeb, a pedido explícito do professor. |
| **Motivação** | Acelerar a tradução do levantamento de requisitos (documento em prosa/tabela) para um modelo estruturado (entidades/atributos/cardinalidades) e para o formato de entrega exigido pela disciplina — inclusive a notação específica pedida pelo professor. |
| **Prompt(s) utilizados** | "Preciso montar um diagrama e o dicionário desse diagrama, como se fosse no modelo BrModeloWeb, veja os dados que estão no arquivo (relatório) e conforme eles, monte um diagrama no formato do BrModeloWEb e o seu dicionário."; "Consegue gerar um arquivo pdf com esse arquivo que montou? Contendo somente o diagrama e o dicionário"; "Ajustar diagrama para o mesmo modelo de BrModeloWeb.". |
| **Resposta recebida** | Um modelo com 21 entidades e 22 relacionamentos; um dicionário de dados completo; este README preenchido; e, por fim, o DER redesenhado em notação de Chen genuína (entidades/relacionamentos/atributos como retângulo/losango/elipse, PK sublinhada, cardinalidades nas pontas), a versão que está de fato anexada na Seção 7. |
| **Fontes consultadas e verificadas** | Nenhuma fonte externa — o modelo foi derivado exclusivamente do texto do questionário de requisitos fornecido pelo grupo. Nenhum dado foi inventado sobre a organização em si. |
| **Trechos rejeitados ou corrigidos** | A primeira versão do diagrama usava notação "pé-de-galinha" por preferência da IA (legibilidade); o professor pediu explicitamente a notação de Chen/BrModeloWeb, então o diagrama foi refeito do zero. Nessa reconstrução, a IA também corrigiu um erro de fidelidade conceitual da primeira versão: atributos de chave estrangeira (ex.: `cod_cliente` em ENDERECO_CLIENTE) tinham sido deixados como atributos visíveis, o que não é correto em um diagrama conceitual puro — eles foram removidos das entidades e passaram a ser representados apenas pela linha do relacionamento. |
| **Justificativa da escolha final** | O grupo manteve a estrutura de 21 entidades por ela cobrir, de forma rastreável, as 8 seções do questionário original sem inventar processos não mencionados. |
| **Reflexão crítica** | O modelo reflete fielmente o texto do questionário, mas não substitui a pesquisa de campo exigida pela atividade — regras de negócio reais da organização escolhida podem divergir do que está aqui (valores de limite de crédito, percentuais de comissão etc. foram tratados como exemplos/referências, não como regras fixas). O grupo deve validar cada regra de negócio da Seção 4 com a organização real antes de assumi-las como definitivas. |

---

## Critérios Atitudinais (20%)
Avaliados por 360º entre os integrantes do grupo — não preenchido neste README.

---