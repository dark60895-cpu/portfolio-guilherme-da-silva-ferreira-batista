# Sistema de Gestão para Indústria e Comércio de Bolsas e Acessórios
**Projeto Acadêmico — Análise e Desenvolvimento de Sistemas (ADS)**  
**Empresa Analisada:** Contrasti Bolsas e Acessórios Ltda (Marca Comercial: *Lingiardi*)

---

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

- **Nome e natureza da organização:** Contrasti Bolsas e Acessórios Ltda - Fabricação de Bolsas. Empresa com fins lucrativos que opera simultaneamente como indústria e comércio de bolsas e acessórios de couro (produção própria e canais multicanal).
- **Contexto e porte:** Operação de pequeno a médio porte com fabricação artesanal (envolvendo ateliê próprio e facções terceirizadas). Volume médio de pedidos por mês entre 50 a 70, aumentando nas datas comemorativas (Dia das Mães e Natal).
- **Problemas e necessidades identificados:** O levantamento de requisitos aponta processos hoje descentralizados/manuais: controle de estoque de insumos (couro, ferragens, zíperes) sem rastreabilidade de lote; ausência de regra formal de crédito para vendas a prazo; cálculo de custo/preço de venda não padronizado (ficha técnica); acompanhamento de produção sem visibilidade de status; e falta de integração entre vendas, estoque e financeiro.
- **Justificativa da escolha:** Uma empresa que o grupo sabia que teria acesso fácil e que consideramos de porte ideal, nem tão simples e nem complexa demais para o escopo do trabalho.
- **Evidências da organização:** 
  - **Endereço Sede:** Rua Alpiste, 116 - Jd. Eliane - São Paulo - SP.
  - **Contato na empresa:** Osmar Lingiardi | Telefone: (11) 97334-4846 | E-mail: osmar@specia.com.br.

---

## 2. Processos de Negócio

Principais processos mapeados (extraídos do levantamento de requisitos, um por bloco do questionário):

1. **Cadastro e gestão de clientes** — Cadastro com múltiplos endereços, perfil de preço (varejo/atacado) e aprovação de crédito.
2. **Cadastro e gestão de fornecedores** — Dados do fornecedor, categoria de insumo e histórico de compras/avaliação.
3. **Ficha técnica e cadastro de produtos** — Modelo → variação (SKU) → lista de materiais (BOM) com cálculo de custo.
4. **Gestão de estoque e compras** — Entrada de insumo (XML de NF-e ou manual), controle por unidade de medida, estoque mínimo e baixa automática.
5. **Ordem de produção e chão de fábrica** — Abertura de OP, execução por etapa (Modelagem → Corte → Preparação/Colagem → Costura → Montagem/Ferragens → Acabamento/Revisão → Embalagem), com Kanban e controle de aproveitamento de couro.
6. **Vendas, pedidos e faturamento** — Pedido multicanal, forma de pagamento, comissão e emissão de NF-e/NFC-e.
7. **Expedição e pós-venda** — Picking & packing, integração logística e garantia (RMA).
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

> As 21 entidades cobrem as 8 seções do levantamento de requisitos.

### CLIENTE
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_cliente | Identificador interno do cliente | Chave primária |
| razao_social_nome | Razão social (PJ) ou nome completo (PF) | Obrigatório |
| cnpj_cpf | Documento fiscal do cliente | Usado na consulta de restrição de crédito |
| inscricao_estadual | Registro estadual do cliente | Obrigatório apenas se perfil = Atacado/Lojista |
| email_nfe | E-mail para envio da nota fiscal eletrônica | — |
| telefone_whatsapp | Contato principal | — |
| nome_comprador_responsavel | Pessoa de contato para compras | — |
| perfil_cliente | Classificação comercial | Domínio: Varejo Final / Atacado-Lojista — define a régua de preço |
| limite_credito | Teto de faturamento via boleto | Referência observada: R$10.000,00 |
| status_aprovacao_financeira | Situação de aprovação de crédito | Domínio: Pendente / Aprovado / Reprovado — obrigatório para novo lojista |

### ENDERECO_CLIENTE *(entidade fraca — depende de CLIENTE)*
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_endereco | Identificador do endereço | Chave primária |
| cod_cliente | Cliente ao qual o endereço pertence | Chave estrangeira → CLIENTE |
| tipo_endereco | Finalidade do endereço | Domínio: Matriz / Entrega / Cobrança |
| logradouro | Rua/avenida e número | — |
| cidade | Cidade | — |
| uf | Unidade federativa | — |
| cep | Código postal | — |

### FORNECEDOR
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_fornecedor | Identificador do fornecedor | Chave primária |
| razao_social | Nome empresarial do fornecedor | — |
| cnpj | Documento fiscal | — |
| inscricao_estadual | Registro estadual | — |
| categoria_insumo | Tipo de insumo fornecido | Domínio: Curtume/Couro, Ferragens/Fivelas, Zíperes/Aviamentos, Embalagens/Caixas |
| contato_vendedor | Pessoa de contato comercial | — |
| prazo_medio_entrega_dias | Lead time médio de entrega | Usado no planejamento de produção |

### INSUMO
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_insumo | Identificador do insumo | Chave primária |
| descricao_insumo | Nome do insumo (ex.: Couro Bovino Caramelo) | — |
| categoria_insumo | Categoria do insumo | — |
| unidade_medida | Unidade de controle de estoque | Domínio: dm², m², unidade, metro, kg, litro |
| estoque_minimo | Saldo mínimo configurável | Dispara alerta automático de recompra |
| saldo_estoque_atual | Saldo atual em estoque | Atualizado a cada entrada/baixa |

### LOTE_INSUMO *(entidade fraca)*
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_lote | Identificador do lote | Chave primária |
| cod_insumo | Insumo recebido | Chave estrangeira → INSUMO |
| cod_fornecedor | Fornecedor de origem | Chave estrangeira → FORNECEDOR |
| cod_compra | Compra que originou o lote | Chave estrangeira → COMPRA |
| numero_lote | Identificação do lote (ex.: 2026-A) | Garante rastreabilidade de cor/textura |
| data_recebimento | Data de entrada no estoque | — |
| quantidade_recebida | Quantidade recebida | — |
| cor_tonalidade | Cor/tonalidade do insumo (couro) | — |
| preco_pago | Preço pago nesse recebimento | Alimenta o histórico de preço por fornecedor |

### COMPRA
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_compra | Identificador da compra | Chave primária |
| cod_fornecedor | Fornecedor da compra | Chave estrangeira → FORNECEDOR |
| numero_compra | Número/identificação da compra | — |
| data_compra | Data da compra | — |
| forma_entrada | Origem do lançamento | Domínio: XML de NF-e / Manual |
| valor_total | Valor total da compra | — |
| taxa_pontualidade_entrega | Percentual de pontualidade apurado | Alimenta a avaliação histórica do fornecedor |

### CONTAS_PAGAR
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_titulo_pagar | Identificador do título | Chave primária |
| cod_compra | Compra que gerou o título | Chave estrangeira → COMPRA |
| valor | Valor do título | — |
| data_vencimento | Data de vencimento | — |
| data_pagamento | Data em que foi pago | Nulo até a baixa |
| status_pagamento | Situação do título | Domínio: Aberto / Pago / Atrasado |

### MODELO_PRODUTO
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_modelo | Identificador do modelo | Chave primária |
| nome_modelo | Nome do modelo (ex.: Bolsa Tote) | — |
| descricao | Descrição do modelo | — |

### VARIACAO_PRODUTO *(SKU)*
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_sku | Identificador da variação | Chave primária |
| cod_modelo | Modelo ao qual pertence | Chave estrangeira → MODELO_PRODUTO |
| codigo_sku | Código comercial (ex.: SKU-BOLSA-TOTE-CAR-UNI) | — |
| cor | Cor da variação | — |
| tipo_couro | Tipo de couro utilizado | Ex.: Bovino Vaqueta, Mestiço |
| tipo_ferragem | Tipo de ferragem utilizada | Ex.: Dourada, Prata, Escovada |
| preco_venda_sugerido | Preço de venda calculado | (insumos + mão de obra + rateio) × markup |
| tempo_estimado_mao_obra_min | Tempo estimado de produção | — |

### ITEM_FICHA_TECNICA *(entidade associativa — BOM)*
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_sku | Variação de produto | Chave primária composta + FK → VARIACAO_PRODUTO |
| cod_insumo | Insumo utilizado | Chave primária composta + FK → INSUMO |
| quantidade_necessaria | Quantidade do insumo por unidade produzida | Ex.: dm² de couro, unidades de zíper, metros de linha |
| unidade_medida_item | Unidade da quantidade necessária | — |

### ARTESAO_FACCAO
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_artesao | Identificador do artesão/facção | Chave primária |
| nome | Nome do artesão ou razão social da facção | — |
| tipo_vinculo | Natureza do vínculo | Domínio: Artesão Interno / Facção Terceirizada |
| telefone | Contato | — |

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
| numero_op | Número da OP | — |
| data_abertura | Data de abertura | Dispara a reserva/baixa automática de insumos |
| quantidade_produzir | Quantidade a produzir | — |
| status_kanban | Status atual da OP | Domínio: Aguardando / Em Corte / Em Costura / Finalizado |

### EXECUCAO_ETAPA *(entidade associativa)*
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_execucao | Identificador da execução | Chave primária |
| cod_op | Ordem de produção | Chave estrangeira → ORDEM_PRODUCAO |
| cod_etapa | Etapa executada | Chave estrangeira → ETAPA_PRODUCAO |
| cod_artesao | Responsável pela execução | Chave estrangeira → ARTESAO_FACCAO — base do pagamento por produção |
| data_inicio | Início da execução | — |
| data_fim | Fim da execução | — |
| percentual_aproveitamento | % de aproveitamento do couro | Aplicável à etapa de Corte |
| percentual_perda | % de perda/retalho | Aplicável à etapa de Corte |

### USUARIO
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_usuario | Identificador do usuário | Chave primária |
| nome | Nome do usuário | — |
| login | Login de acesso | — |
| senha_hash | Senha (armazenada com hash) | — |
| perfil_acesso | Perfil de acesso ao sistema | Domínio: Vendedor / Gerente de Produção / Financeiro / Administrador |
| tipo_vendedor | Natureza do vínculo comercial | Domínio: Interno / Representante Externo (nulo se não for vendedor) |
| percentual_comissao | Percentual de comissão | Padrão 5%, diferenciado por atacado/varejo |

### PEDIDO_VENDA
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_pedido | Identificador do pedido | Chave primária |
| cod_cliente | Cliente do pedido | Chave estrangeira → CLIENTE |
| cod_usuario | Vendedor/representante responsável | Chave estrangeira → USUARIO |
| numero_pedido | Número do pedido | — |
| data_pedido | Data do pedido | — |
| canal_venda | Canal de venda | Domínio: Loja Física / E-commerce / WhatsApp / Representante |
| forma_pagamento | Forma de pagamento | Domínio: PIX / Cartão / Boleto |
| condicao_parcelamento | Condição de parcelamento | Ex.: 30/60/90 dias (boleto atacado) |
| status_pedido | Situação do pedido | — |

### ITEM_PEDIDO *(entidade associativa)*
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_item_pedido | Identificador do item | Chave primária |
| cod_pedido | Pedido ao qual pertence | Chave estrangeira → PEDIDO_VENDA |
| cod_sku | Produto vendido | Chave estrangeira → VARIACAO_PRODUTO |
| quantidade | Quantidade vendida | — |
| preco_unitario_praticado | Preço unitário praticado | — |
| desconto | Desconto aplicado | — |

### NOTA_FISCAL
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_nf | Identificador da nota | Chave primária |
| cod_pedido | Pedido faturado | Chave estrangeira → PEDIDO_VENDA |
| numero_nf | Número da nota | — |
| tipo_nf | Tipo de documento fiscal | Domínio: NF-e (mercantil) / NFC-e (cupom varejo) |
| data_emissao | Data de emissão | — |
| valor_total | Valor total da nota | — |

### CONTAS_RECEBER
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_titulo_receber | Identificador do título | Chave primária |
| cod_pedido | Pedido que gerou o título | Chave estrangeira → PEDIDO_VENDA |
| valor | Valor do título | — |
| data_vencimento | Data de vencimento | — |
| data_recebimento | Data em que foi recebido | — |
| status_recebimento | Situação do título | Domínio: Aberto / Recebido / Atrasado |

### EXPEDICAO *(1:1 com pedido)*
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_expedicao | Identificador da expedição | Chave primária |
| cod_pedido | Pedido expedido | Chave estrangeira → PEDIDO_VENDA |
| data_envio | Data de envio | — |
| transportadora | Transportadora utilizada | Ex.: Correios, Melhor Envio, parceira |
| codigo_rastreio | Código de rastreamento | — |
| checklist_conferencia | Conferência realizada | Via leitura de código de barras |
| dust_bag_incluso | Saquinho protetor incluído | — |

### RMA_GARANTIA
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_rma | Identificador do chamado | Chave primária |
| cod_item_pedido | Item associado à garantia | Chave estrangeira → ITEM_PEDIDO (não ao pedido inteiro) |
| data_abertura | Data de abertura do chamado | — |
| motivo_defeito | Motivo relatado | — |
| status_rma | Situação do chamado | Domínio: Aberto / Em Reparo / Devolvido |
| data_devolucao | Data de devolução ao cliente | — |

---

## 6. Modelagem Conceitual (Entidades, Atributos, Relacionamentos)

- **Entidades reconhecidas (21 no total):** As 21 entidades listadas na Seção 5, agrupadas nos blocos de negócio identificados no levantamento. Duas são **entidades fracas** (ENDERECO_CLIENTE, LOTE_INSUMO) e três são **entidades associativas** que resolvem relacionamentos N:N com atributos próprios (ITEM_FICHA_TECNICA, EXECUCAO_ETAPA, ITEM_PEDIDO).
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

---

## 7. Diagrama Entidade-Relacionamento (DER em Notação de Chen)

O diagrama conceitual foi estruturado em formato horizontal (da esquerda para a direita), priorizando a clareza acadêmica e seguindo fielmente o padrão visual exigido.

```mermaid
flowchart LR
    %% Estilos visuais limpos
    classDef entidade fill:#F8F8FF,stroke:#000080,stroke-width:1.5px,color:#000080,font-weight:bold;
    classDef rel fill:#FFFAF0,stroke:#FF8C00,stroke-width:1.5px,color:#FF8C00,font-weight:bold;
    classDef attr fill:#F0FFF0,stroke:#2E8B57,stroke-width:1px,color:#2E8B57;
    classDef espec fill:#FFFACD,stroke:#DAA520,stroke-width:1.5px,color:#DAA520,font-weight:bold;

    %% Fluxo Principal
    E_FORN[FORNECEDOR]:::entidade
    R_FORN{fornece}:::rel
    E_INS[INSUMO]:::entidade
    R_GER{gera lote}:::rel
    E_LOTE[LOTE_INSUMO]:::entidade
    R_CONT{contém}:::rel
    E_PROD[PRODUTO]:::entidade
    
    R_ESP{Especialização<br/>( T , D )}:::espec
    E_BOLSA[BOLSA]:::entidade
    E_ACES[ACESSORIO]:::entidade
    
    R_VEND{vende}:::rel
    E_PED[PEDIDO]:::entidade

    %% Conexões
    E_FORN -- "1:N" --- R_FORN --- "1:N" --> E_INS
    E_INS -- "1:N" --- R_GER --- "1:N" --> E_LOTE
    E_LOTE -- "1:N" --- R_CONT --- "1:N" --> E_PROD
    
    E_PROD --- R_ESP
    R_ESP --> E_BOLSA
    R_ESP --> E_ACES

    E_PROD -- "1:N" --- R_VEND --- "0:N" --> E_PED

    %% Atributos (Caixinhas Verdes)
    A_F1[CNPJ (PK)]:::attr
    A_F2[Nome]:::attr
    E_FORN --- A_F1
    E_FORN --- A_F2

    A_I1[Cod_Insumo (PK)]:::attr
    A_I2[Descricao]:::attr
    A_I3[Unidade_Medida]:::attr
    E_INS --- A_I1
    E_INS --- A_I2
    E_INS --- A_I3

    A_L1[Codigo_Lote (PK)]:::attr
    A_L2[Cor_Tonalidade]:::attr
    A_L3[Quantidade]:::attr
    E_LOTE --- A_L1
    E_LOTE --- A_L2
    E_LOTE --- A_L3

    A_P1[ID_Produto (PK)]:::attr
    A_P2[Nome]:::attr
    A_P3[Preco_Venda]:::attr
    E_PROD --- A_P1
    E_PROD --- A_P2
    E_PROD --- A_P3

    A_B1[Tamanho]:::attr
    A_B2[Cor]:::attr
    A_B3[Tipo_Alca]:::attr
    E_BOLSA --- A_B1
    E_BOLSA --- A_B2
    E_BOLSA --- A_B3

    A_A1[Tipo_Peca]:::attr
    E_ACES --- A_A1

    A_PE1[Codigo_Pedido (PK)]:::attr
    A_PE2[Data_Compra]:::attr
    A_PE3[Valor_Total]:::attr
    E_PED --- A_PE1
    E_PED --- A_PE2
    E_PED --- A_PE3
