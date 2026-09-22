# Modelagem Conceitual de Banco de Dados para Custos de Produção e Vendas — Contrasti Bolsas e Acessórios Ltda

## Introdução

Este trabalho apresenta a modelagem conceitual de um sistema de gestão de informações para a **Contrasti Bolsas e Acessórios Ltda**, indústria e comércio de bolsas e acessórios de couro localizada em São Paulo/SP. O problema identificado é a ausência de um sistema integrado que conecte estoque de insumos, ficha técnica/custo de produção, ordens de produção, vendas multicanal e financeiro — hoje esses processos são conduzidos de forma manual e descentralizada (planilhas soltas, controle informal de estoque e cálculo não padronizado de custo/preço).

O objetivo geral é levantar os requisitos reais da organização por meio de pesquisa de campo e traduzi-los em um modelo conceitual de dados (entidades, atributos, relacionamentos e cardinalidades) representado por um Diagrama Entidade-Relacionamento (DER) na notação de Chen.

Como delimitação, o grupo modelou dois níveis de detalhe: 
1. Um modelo completo de **21 entidades**, cobrindo todos os processos mapeados no levantamento de requisitos (cadastros, estoque/compras, produção, vendas/faturamento, expedição e financeiro).
2. Um recorte simplificado de **9 entidades** (Seção 9), focado apenas no fluxo de custo de produção e vendas — do fornecedor de insumos até o pedido faturado ao cliente —, entregue como versão alternativa/reduzida no mesmo padrão de notação. 

Esta entrega não cobre a implementação física do banco de dados (modelo lógico/físico), o que fica para etapas futuras do projeto.

---

## Metadados

| Nome | RGM |
| :--- | :--- |
| Guilherme da Silva Ferreira Batista | 47302518 |
| Guilherme Petrucelli Domingos | 47270161 |
| Jaime Luiz de Oliveira Neto | 47336951 |
| Matheus Montagner | 47209470 |
| Vinicius Marques de Melo | 47213426 |

---

## 1. Caracterização da Organização

* **Nome e natureza da organização:** Contrasti Bolsas e Acessórios Ltda - Fabricação de Bolsas. O questionário usado como base trata de uma "Indústria e Comércio de Bolsas de Couro" — uma empresa com fins lucrativos que fabrica e vende bolsas de couro (produção própria + venda direta e por canais diversos).
* **Contexto e porte:** Com fins lucrativos; opera simultaneamente como indústria e comércio (venda em loja física, e-commerce, WhatsApp e representantes externos). O uso de facções terceirizadas e o controle de aproveitamento de couro por corte sugerem uma operação de pequeno a médio porte, com produção sob encomenda/lote (não em larga escala industrial). Volume médio de pedidos por mês entre 50 a 70, aumentando nas datas comemorativas (Dia das Mães e Natal).
* **Problemas e necessidades identificados:** O levantamento de requisitos aponta processos hoje descentralizados e manuais: controle de estoque de insumos (couro, ferragens, zíperes) sem rastreabilidade de lote; ausência de regra formal de crédito para vendas a prazo; cálculo de custo/preço de venda não padronizado (ficha técnica); acompanhamento de produção sem visibilidade de status; e falta de integração entre vendas, estoque e financeiro (títulos a pagar/receber gerados manualmente).
* **Justificativa da escolha:** Uma empresa que a equipe sabia ter acesso fácil e que foi considerada de porte adequado, equilibrando a complexidade do projeto.
* **Evidências da organização:** Endereço: Rua Alpiste, 116 - Jd. Eliane - São Paulo - SP. Contato na empresa: Osmar Lingiardi, telefone: (11) 97334-4846, e-mail: osmar@specia.com.br.

---

## 2. Processos de Negócio

Principais processos mapeados (extraídos do levantamento de requisitos, um por bloco do questionário):

1. **Cadastro e gestão de clientes:** cadastro com múltiplos endereços, perfil de preço (varejo/atacado) e aprovação de crédito.
2. **Cadastro e gestão de fornecedores:** dados do fornecedor, categoria de insumo e histórico de compras/avaliação.
3. **Ficha técnica e cadastro de produtos:** modelo → variação (SKU) → lista de materiais (BOM) com cálculo de custo.
4. **Gestão de estoque e compras:** entrada de insumo (XML de NF-e ou manual), controle por unidade de medida, estoque mínimo e baixa automática.
5. **Ordem de produção e chão de fábrica:** abertura de OP, execução por etapa (Modelagem → Corte → Preparação/Colagem → Costura → Montagem/Ferragens → Acabamento/Revisão → Embalagem), com Kanban e controle de aproveitamento de couro.
6. **Vendas, pedidos e faturamento:** pedido multicanal, forma de pagamento, comissão e emissão de NF-e/NFC-e.
7. **Expedição e pós-venda:** picking & packing, integração logística e garantia (RMA).
8. **Gestão financeira e relatórios gerenciais:** Contas a Pagar/Receber automáticos e indicadores (Curva ABC, DRE, fluxo de caixa).

### Fluxogramas:
> ⚠️ **Pendente:** O esqueleto da atividade pede a representação visual de pelo menos os processos-chave (imagens anexadas). O grupo desenhará e anexará aqui o fluxograma de, no mínimo, os processos 3, 5 e 6, que sustentam o DER das Seções 7 e 9. Caminho sugerido: `docs/fluxogramas/fluxograma-<processo>.png`.

---

## 3. Requisitos do Sistema

### 3.1 Requisitos Funcionais

| ID | Requisito Funcional |
| :--- | :--- |
| **RF01** | Cadastrar clientes com razão social/nome, CNPJ/CPF, inscrição estadual (quando lojista), e-mail para NF-e, telefone/WhatsApp e comprador responsável. |
| **RF02** | Registrar múltiplos endereços por cliente (matriz, entrega, cobrança). |
| **RF03** | Diferenciar clientes por perfil (Varejo Final × Atacado/Lojista) com regras de preço distintas. |
| **RF04** | Consultar restrição de CPF/CNPJ e aplicar limite de crédito para boleto, exigindo aprovação financeira para novos lojistas. |
| **RF05** | Cadastrar fornecedores com categoria de insumo, contato do vendedor e prazo médio de entrega. |
| **RF06** | Registrar entrada de insumos por lote, vinculando fornecedor, cor/tonalidade e preço pago (rastreabilidade). |
| **RF07** | Manter histórico de compras e avaliação por fornecedor (preço, variação, pontualidade de entrega). |
| **RF08** | Cadastrar produtos por modelo e variação (SKU), combinando cor, tipo de couro e tipo de ferragem. |
| **RF09** | Manter a Ficha Técnica (BOM) de cada SKU, com a quantidade de cada insumo necessária. |
| **RF10** | Calcular automaticamente o custo total e sugerir preço de venda a partir da Ficha Técnica, mão de obra e markup. |
| **RF11** | Registrar entrada de matéria-prima via XML de NF-e ou digitação manual. |
| **RF12** | Controlar o estoque de insumos nas unidades apropriadas (dm²/m², unidade/metro, kg/L), com estoque mínimo e alerta automático. |
| **RF13** | Baixar automaticamente o estoque de insumos na abertura da Ordem de Produção. |
| **RF14** | Abrir Ordens de Produção vinculadas a um SKU e acompanhar o status em painel Kanban (Aguardando/Em Corte/Em Costura/Finalizado). |
| **RF15** | Registrar, por etapa de produção, o artesão/facção responsável e o percentual de aproveitamento/perda de couro na etapa de Corte. |
| **RF16** | Registrar pedidos de venda por canal (loja física, e-commerce, WhatsApp, representante), com forma de pagamento e parcelamento. |
| **RF17** | Calcular comissão de vendedores/representantes sobre pedidos faturados, com percentuais diferenciados por canal. |
| **RF18** | Emitir NF-e (venda mercantil) e NFC-e (cupom varejo), nativamente ou por integração. |
| **RF19** | Controlar separação, conferência (código de barras) e embalagem (checklist + dust bag) antes do envio. |
| **RF20** | Integrar com serviços de logística (Correios, Melhor Envio, transportadoras parceiras) para etiquetas e rastreio. |
| **RF21** | Registrar trocas, devoluções e garantias (RMA) vinculadas ao item efetivamente entregue. |
| **RF22** | Gerar lançamentos automáticos de Contas a Receber (vendas faturadas) e Contas a Pagar (compras). |
| **RF23** | Gerar relatórios gerenciais: Curva ABC, margem de lucro por modelo, DRE gerencial, fluxo de caixa previsto × realizado, estoque parado. |
| **RF24** | Restringir o acesso por perfil de usuário (Vendedor, Gerente de Produção, Financeiro, Administrador). |

### 3.2 Requisitos Não Funcionais

| ID | Característica de Qualidade |
| :--- | :--- |
| **RNF01** | **Desempenho:** consultas de estoque e ficha técnica devem responder rápido o suficiente para não atrasar a abertura de uma Ordem de Produção. |
| **RNF02** | **Segurança:** controle de acesso por perfil (RF24) e proteção de dados pessoais de clientes/fornecedores (LGPD). |
| **RNF03** | **Disponibilidade:** o módulo de estoque/OP precisa estar disponível no horário de produção, já que a baixa de insumo é automática e bloqueante. |
| **RNF04** | **Usabilidade:** o painel Kanban de produção deve ser operável pelos artesãos/facções sem treinamento extenso. |
| **RNF05** | **Integridade transacional:** baixa de estoque, geração de título financeiro e emissão de nota fiscal devem ocorrer de forma atômica (tudo ou nada). |
| **RNF06** | **Auditabilidade:** histórico de preços por fornecedor e de status de OP deve ser preservado para consultas gerenciais futuras. |

---

## 4. Regras de Negócio

### Regras operacionais:
* Inscrição Estadual só é obrigatória quando o perfil do cliente é Atacado/Lojista.
* Toda variação de produto (SKU) pertence a exatamente um modelo; um modelo pode ter uma ou várias variações.
* A baixa de estoque de insumo ocorre automaticamente na abertura da Ordem de Produção (reserva de material necessário).
* Todo insumo recebido é registrado com lote e fornecedor de origem, para garantir rastreabilidade de cor/textura entre peças da mesma coleção.
* Comissão padrão de vendedor é de 5% sobre pedidos faturados, com percentual diferenciado entre canal de atacado e varejo.
* Um chamado de garantia (RMA) está sempre associado a um item de pedido específico, nunca ao pedido inteiro.
* Um pedido só é expedido depois de checklist de saída e conferência dos itens (leitura de código de barras).

### Restrições organizacionais:
* Exigência legal de emissão de NF-e (venda mercantil) ou NFC-e (cupom fiscal varejo), conforme o canal de venda.
* Dados pessoais de clientes e fornecedores (nome, CPF/CNPJ, contato) exigem tratamento conforme a LGPD.
* O prazo médio de entrega de cada fornecedor impacta diretamente o planejamento da produção (lead time de insumo).
* O pagamento de artesãos/facções terceirizadas depende do registro fiel de qual etapa cada um executou — sem esse registro não há como calcular a remuneração por produção.

---

## 5. Dicionário de Dados Conceitual (Modelo Completo — 21 Entidades)

*As 21 entidades abaixo cobrem integralmente as 8 seções do levantamento de requisitos da empresa.*

### CLIENTE
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `cod_cliente` | Identificador interno do cliente | Chave primária |
| `razao_social_nome` | Razão social (PJ) ou nome completo (PF) | Obrigatório |
| `cnpj_cpf` | Documento fiscal do cliente | Usado na consulta de restrição de crédito |
| `inscricao_estadual` | Registro estadual do cliente | Obrigatório apenas se perfil = Atacado/Lojista |
| `email_nfe` | E-mail para envio da nota fiscal eletrônica | Obrigatório; canal de entrega da NF-e/NFC-e |
| `telefone_whatsapp` | Contato principal | Usado também como canal de venda via WhatsApp |
| `nome_comprador_responsavel` | Pessoa de contato para compras | Preenchido principalmente para Atacado/Lojista |
| `perfil_cliente` | Classificação comercial | Domínio: Varejo Final / Atacado-Lojista |
| `limite_credito` | Teto de faturamento via boleto | Referência observada: R$ 10.000,00 |
| `status_aprovacao_financeira` | Situação de aprovação de crédito | Domínio: Pendente / Aprovado / Reprovado |

### ENDERECO_CLIENTE *(entidade fraca — depende de CLIENTE)*
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `cod_endereco` | Identificador do endereço | Chave primária |
| `cod_cliente` | Cliente ao qual o endereço pertence | Chave estrangeira → CLIENTE |
| `tipo_endereco` | Finalidade do endereço | Domínio: Matriz / Entrega / Cobrança |
| `logradouro` | Rua/avenida e número | Obrigatório |
| `cidade` | Cidade | Obrigatório |
| `uf` | Unidade federativa | Obrigatório; sigla de 2 letras (ex.: SP) |
| `cep` | Código postal | Obrigatório; usado no cálculo de frete |

### FORNECEDOR
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `cod_fornecedor` | Identificador do fornecedor | Chave primária |
| `razao_social` | Nome empresarial do fornecedor | Obrigatório |
| `cnpj` | Documento fiscal | Obrigatório e único. Dado protegido pela LGPD |
| `inscricao_estadual` | Registro estadual | Opcional |
| `categoria_insumo` | Tipo de insumo fornecido | Domínio: Curtume, Ferragens, Zíperes, Embalagens |
| `contato_vendedor` | Pessoa de contato comercial | Nome da pessoa que atende a empresa |
| `prazo_medio_entrega_dias` | Lead time médio de entrega | Usado no planejamento de produção |

### INSUMO
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `cod_insumo` | Identificador do insumo | Chave primária |
| `descricao_insumo` | Nome do insumo (ex.: Couro Bovino Caramelo) | Obrigatório |
| `categoria_insumo` | Categoria do insumo | Mesmo domínio de FORNECEDOR |
| `unidade_medida` | Unidade de controle de estoque | Domínio: dm², m², unidade, metro, kg, litro |
| `estoque_minimo` | Saldo mínimo configurável | Dispara alerta automático de recompra |
| `saldo_estoque_atual` | Saldo atual em estoque | Atualizado a cada entrada/baixa |

### LOTE_INSUMO *(entidade fraca — INSUMO, FORNECEDOR e COMPRA)*
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `cod_lote` | Identificador do lote | Chave primária |
| `cod_insumo` | Insumo recebido | Chave estrangeira → INSUMO |
| `cod_fornecedor` | Fornecedor de origem | Chave estrangeira → FORNECEDOR |
| `cod_compra` | Compra que originou o lote | Chave estrangeira → COMPRA |
| `numero_lote` | Identificação do lote (ex.: 2026-A) | Garante rastreabilidade de cor/textura |
| `data_recebimento` | Data de entrada no estoque | Atualiza o saldo atual |
| `quantidade_recebida` | Quantidade recebida | Mesma unidade de medida do insumo |
| `cor_tonalidade` | Cor/tonalidade do insumo (couro) | Preenchida principalmente para couro |
| `preco_pago` | Preço pago nesse recebimento | Alimenta o histórico de preço |

### COMPRA
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `cod_compra` | Identificador da compra | Chave primária |
| `cod_fornecedor` | Fornecedor da compra | Chave estrangeira → FORNECEDOR |
| `numero_compra` | Número/identificação da compra | Sequencial, gerado pelo sistema |
| `data_compra` | Data da compra | Usada no cálculo do lead time |
| `forma_entrada` | Origem do lançamento | Domínio: XML de NF-e / Manual |
| `valor_total` | Valor total da compra | Origina o lançamento em CONTAS_PAGAR |
| `taxa_pontualidade_entrega` | Percentual de pontualidade apurado | Alimenta a avaliação do fornecedor |

### CONTAS_PAGAR
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `cod_titulo_pagar` | Identificador do título | Chave primária |
| `cod_compra` | Compra que gerou o título | Chave estrangeira → COMPRA |
| `valor` | Valor do título | Herdado do valor_total da COMPRA |
| `data_vencimento` | Data de vencimento | Prazo limite para pagamento |
| `data_pagamento` | Data em que foi pago | Nulo até a baixa |
| `status_pagamento` | Situação do título | Domínio: Aberto / Pago / Atrasado |

### MODELO_PRODUTO
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `cod_modelo` | Identificador do modelo | Chave primária |
| `nome_modelo` | Nome do modelo (ex.: Bolsa Tote) | Obrigatório |
| `descricao` | Descrição do modelo | Texto livre para catálogo |

### VARIACAO_PRODUTO (SKU)
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `cod_sku` | Identificador da variação | Chave primária |
| `cod_modelo` | Modelo ao qual pertence | Chave estrangeira → MODELO_PRODUTO |
| `codigo_sku` | Código comercial | Gerado a partir de modelo + cor + tipo |
| `cor` | Cor da variação | Obrigatório |
| `tipo_couro` | Tipo de couro utilizado | Ex.: Bovino Vaqueta, Mestiço |
| `tipo_ferragem` | Tipo de ferragem utilizada | Ex.: Dourada, Prata, Escovada |
| `preco_venda_sugerido` | Preço de venda calculado | (Insumos + Mão de obra + Rateio) × Markup |
| `tempo_estimado_mao_obra_min` | Tempo estimado de produção | Referência para planejamento |

### ITEM_FICHA_TECNICA *(entidade associativa — BOM)*
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `cod_sku` | Variação de produto | Chave primária composta + FK → VARIACAO_PRODUTO |
| `cod_insumo` | Insumo utilizado | Chave primária composta + FK → INSUMO |
| `quantidade_necessaria` | Quantidade do insumo por unidade produzida | Ex.: dm² de couro, unidades de zíper |
| `unidade_medida_item` | Unidade da quantidade necessária | Deve ser a mesma do INSUMO |

### ARTESAO_FACCAO
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `cod_artesao` | Identificador do artesão/facção | Chave primária |
| `nome` | Nome do artesão ou razão social | Obrigatório |
| `tipo_vinculo` | Natureza do vínculo | Domínio: Artesão Interno / Facção Terceirizada |
| `telefone` | Contato | Usado para agendamento |

### ETAPA_PRODUCAO *(catálogo)*
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `cod_etapa` | Identificador da etapa | Chave primária |
| `nome_etapa` | Nome da etapa | Modelagem, Corte, Costura, Acabamento, etc. |
| `ordem_sequencial` | Posição da etapa no fluxo produtivo | Define a sequência do Kanban |

### ORDEM_PRODUCAO
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `cod_op` | Identificador da ordem de produção | Chave primária |
| `cod_sku` | Variação de produto a ser produzida | Chave estrangeira → VARIACAO_PRODUTO |
| `numero_op` | Número da OP | Sequencial, painel Kanban |
| `data_abertura` | Data de abertura | Dispara a reserva/baixa automática |
| `quantidade_produzir` | Quantidade a produzir | Inteiro maior que zero |
| `status_kanban` | Status atual da OP | Domínio: Aguardando / Em Corte / Em Costura / Finalizado |

### EXECUCAO_ETAPA *(entidade associativa)*
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `cod_execucao` | Identificador da execução | Chave primária |
| `cod_op` | Ordem de produção | Chave estrangeira → ORDEM_PRODUCAO |
| `cod_etapa` | Etapa executada | Chave estrangeira → ETAPA_PRODUCAO |
| `cod_artesao` | Responsável pela execução | Chave estrangeira → ARTESAO_FACCAO |
| `data_inicio` | Início da execução | Início da contagem para o artesão |
| `data_fim` | Fim da execução | Mede a duração e baseia o pagamento |
| `percentual_aproveitamento` | % de aproveitamento do couro | Aplicável à etapa de Corte |
| `percentual_perda` | % de perda/retalho | Aplicável à etapa de Corte |

### USUARIO
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `cod_usuario` | Identificador do usuário | Chave primária |
| `nome` | Nome do usuário | Obrigatório |
| `login` | Login de acesso | Obrigatório e único |
| `senha_hash` | Senha (armazenada com hash) | Nunca em texto puro |
| `perfil_acesso` | Perfil de acesso ao sistema | Domínio: Vendedor / Gerente / Financeiro / Admin |
| `tipo_vendedor` | Natureza do vínculo comercial | Interno / Representante Externo |
| `percentual_comissao` | Percentual de comissão | Padrão 5%, diferenciado por canal |

### PEDIDO_VENDA
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `cod_pedido` | Identificador do pedido | Chave primária |
| `cod_cliente` | Cliente do pedido | Chave estrangeira → CLIENTE |
| `cod_usuario` | Vendedor/representante responsável | Chave estrangeira → USUARIO |
| `numero_pedido` | Número do pedido | Sequencial, exibido ao cliente |
| `data_pedido` | Data do pedido | Obrigatória; início do faturamento |
| `canal_venda` | Canal de venda | Domínio: Loja Física / E-commerce / WhatsApp / Rep |
| `forma_pagamento` | Forma de pagamento | Domínio: PIX / Cartão / Boleto |
| `condicao_parcelamento` | Condição de parcelamento | Ex.: 30/60/90 dias |
| `status_pedido` | Situação do pedido | Andamento do pedido |

### ITEM_PEDIDO *(entidade associativa)*
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `cod_item_pedido` | Identificador do item | Chave primária |
| `cod_pedido` | Pedido ao qual pertence | Chave estrangeira → PEDIDO_VENDA |
| `cod_sku` | Produto vendido | Chave estrangeira → VARIACAO_PRODUTO |
| `quantidade` | Quantidade vendida | Inteiro maior que zero |
| `preco_unitario_praticado` | Preço unitário praticado | Congelado no momento da venda |
| `desconto` | Desconto aplicado | Opcional |

### NOTA_FISCAL
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `cod_nf` | Identificador da nota | Chave primária |
| `cod_pedido` | Pedido faturado | Chave estrangeira → PEDIDO_VENDA |
| `numero_nf` | Número da nota | Sequencial (SEFAZ) |
| `tipo_nf` | Tipo de documento fiscal | Domínio: NF-e / NFC-e |
| `data_emissao` | Data de emissão | Referência legal |
| `valor_total` | Valor total da nota | Igual ao valor_total do pedido |

### CONTAS_RECEBER
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `cod_titulo_receber` | Identificador do título | Chave primária |
| `cod_pedido` | Pedido que gerou o título | Chave estrangeira → PEDIDO_VENDA |
| `valor` | Valor do título | Herdado do pedido |
| `data_vencimento` | Data de vencimento | Segue a condição de parcelamento |
| `data_recebimento` | Data em que foi recebido | Nulo até a baixa |
| `status_recebimento` | Situação do título | Domínio: Aberto / Recebido / Atrasado |

### EXPEDICAO *(1:1 com pedido)*
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `cod_expedicao` | Identificador da expedição | Chave primária |
| `cod_pedido` | Pedido expedido | Chave estrangeira → PEDIDO_VENDA |
| `data_envio` | Data de envio | Despacho efetivo |
| `transportadora` | Transportadora utilizada | Ex.: Correios, Melhor Envio |
| `codigo_rastreio` | Código de rastreamento | Fornecido pela transportadora |
| `checklist_conferencia` | Conferência realizada | Via código de barras |
| `dust_bag_incluso` | Saquinho protetor incluído | Checklist de embalagem |

### RMA_GARANTIA
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `cod_rma` | Identificador do chamado | Chave primária |
| `cod_item_pedido` | Item associado à garantia | Chave estrangeira → ITEM_PEDIDO |
| `data_abertura` | Data de abertura do chamado | Inicia o prazo de análise |
| `motivo_defeito` | Motivo relatado | Descrição do defeito |
| `status_rma` | Situação do chamado | Domínio: Aberto / Em Reparo / Devolvido |
| `data_devolucao` | Data de devolução ao cliente | Retorno ao cliente |

---

## 6. Modelagem Conceitual (Entidades, Atributos, Relacionamentos)

* **Entidades reconhecidas (21 no total):** Agrupadas nos 6 blocos de negócio identificados no levantamento. Duas são entidades fracas (`ENDERECO_CLIENTE`, `LOTE_INSUMO`) e três são entidades associativas (`ITEM_FICHA_TECNICA`, `EXECUCAO_ETAPA`, `ITEM_PEDIDO`).
* **Relacionamentos pertinentes (22 no total):**

| # | Entidade A | Card. A | Verbo | Card. B | Entidade B |
|:---|:---|:---|:---|:---|:---|
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

## 7. Diagrama Entidade-Relacionamento (DER)

* **Arquivos anexados:** `docs/der/der-completo-21-entidades.png` e `docs/der/der-completo-21-entidades.svg`.

---

## 8. Justificativa Técnica

* **Por que 21 entidades:** Cada entidade possui identidade própria e ciclo de vida distinto no domínio estudado.
* **Por que entidades fracas:** `ENDERECO_CLIENTE` e `LOTE_INSUMO` não possuem existência independente.
* **Por que entidades associativas:** `ITEM_FICHA_TECNICA`, `EXECUCAO_ETAPA` e `ITEM_PEDIDO` carregam atributos próprios essenciais.

---

## 9. Anexo — Modelo Conceitual Simplificado (9 Entidades)

* **Escopo:** `FORNECEDOR`, `INSUMO`, `FICHA_TECNICA`, `PRODUTO`, `BOLSA`, `ACESSORIO`, `ITEM_PEDIDO`, `PEDIDO` e `CLIENTE`.

### 9.1 Dicionário de Dados Conceitual (9 Entidades)

#### FORNECEDOR
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `id_fornecedor` (PK) | Identificador do fornecedor | Chave primária gerada pelo sistema. É única e não pode ser reutilizada. |
| `razao_social` | Nome empresarial | Obrigatório. Nome usado em compras e no histórico de avaliação do fornecedor. |
| `cnpj` | Cadastro Nacional de Pessoa Jurídica | Obrigatório e único. Não pode haver dois fornecedores com o mesmo CNPJ. Dado protegido pela LGPD. |
| `inscricao_estadual` | Registro estadual | Opcional. Preenchido quando o fornecedor possuir inscrição estadual. |
| `email` | E-mail comercial | Opcional. Canal de contato comercial. |
| `telefone` | Telefone | Opcional. Contato direto com o fornecedor. |
| `contato_vendedor` | Vendedor de referência | Pessoa responsável pelo atendimento comercial à empresa. |
| `categoria_insumo` | Tipo de insumo vendido | Obrigatório. Aceita: Curtume/Couro, Ferragens/Fivelas, Zíperes/Aviamentos ou Embalagens/Caixas. |
| `prazo_medio_entrega_dias` | Prazo de entrega | Número inteiro maior que zero. Orienta o planejamento de compra dos insumos. |

#### INSUMO
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `id_insumo` (PK) | Identificador do insumo | Chave primária gerada pelo sistema. |
| `nome_insumo` | Nome do material | Obrigatório. Ex.: couro bovino, zíper, fivela ou forro. |
| `categoria_insumo` | Grupo do material | Deve usar as mesmas categorias cadastradas para fornecedor, permitindo identificar possíveis fornecedores. |
| `unidade_medida` | Unidade de controle | Obrigatória. Aceita: dm², m², unidade, metro, kg ou litro. Deve ser a mesma usada no estoque e na ficha técnica. |
| `estoque_minimo` | Saldo mínimo | Quando estoque_atual for menor ou igual a este valor, o sistema deve gerar alerta de recompra. |
| `estoque_atual` | Saldo disponível | Não pode ser negativo. Aumenta no recebimento de compras e diminui com o consumo produtivo. |
| `custo_unitario` | Custo por unidade | Deve ser maior que zero. É a referência para cálculo do custo de matéria-prima. |

#### FICHA_TECNICA *(Entidade associativa)*
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `id_produto + id_insumo` (PK composta) | Identificador da linha técnica | Cada combinação de produto e insumo deve ser única. Um mesmo insumo não pode aparecer duas vezes na ficha do mesmo produto. |
| `quantidade_necessaria` | Quantidade por peça | Obrigatória e maior que zero. Deve usar a mesma unidade_medida definida no insumo. |
| `percentual_perda` | Perda técnica de material | Informado como fração entre 0 e 1 (ex.: 10% = 0,10). É aplicado no cálculo do custo do insumo. |

#### PRODUTO
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `id_produto` (PK) | Identificador do produto | Chave primária gerada pelo sistema. É também a chave herdada pelos subtipos BOLSA e ACESSORIO. |
| `nome_modelo` | Nome comercial do modelo | Obrigatório. Ex.: Bolsa Tote. |
| `markup` | Multiplicador de margem | Obrigatório e maior que 1. Ex.: 2,5 indica preço de venda equivalente a 2,5 vezes o custo. |
| `custo_mao_obra` | Mão de obra por peça | Valor informado, não calculado. Inclui atividades como corte e costura. |
| `custo_materia_prima` (derivado) | Custo dos materiais | Não é digitado. Soma, para cada item da ficha, de quantidade_necessaria x (1 + percentual_perda) x custo_unitario. |
| `preco_tabela` (derivado) | Preço de venda sugerido | Não é digitado. Calculado por (custo_materia_prima + custo_mao_obra) x markup. Deve ser recalculado quando seus componentes mudarem. |

#### BOLSA *(Especialização de PRODUTO)*
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `tamanho` | Dimensão da bolsa | Obrigatório para produtos classificados como bolsa. |
| `cor` | Cor da bolsa | Obrigatória para bolsa. |
| `tipo_alca` | Tipo de alça | Obrigatório para bolsa. |
| `pecas_composicao` (multivalorado) | Peças que formam a bolsa | Deve possuir ao menos uma peça. Exemplos: tampa, frente, costa, fundo e orla. |

#### ACESSORIO *(Especialização de PRODUTO)*
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `tipo_peca` | Tipo do acessório | Obrigatório. Exemplos: cinto, carteira ou porta-cartões. Um acessório não pode possuir atributos exclusivos de bolsa. |

#### PEDIDO
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `id_pedido` (PK) | Identificador do pedido | Chave primária gerada pelo sistema. |
| `data_pedido` | Data do registro | Obrigatória. Registrada na criação do pedido. |
| `canal_venda` | Origem da venda | Aceita: Loja Física, E-commerce, WhatsApp ou Representante. Pode definir regras de comissão e faturamento. |
| `forma_pagamento` | Meio de pagamento | Aceita: PIX, Cartão ou Boleto. Boleto exige cliente aprovado e valor dentro do limite de crédito. |
| `condicao_parcelamento` | Parcelas ou prazo | Depende da forma de pagamento: PIX à vista; Cartão em parcelas; Boleto com prazos, como 30/60/90 dias. |
| `status_pedido` | Situação comercial | Exemplos: aberto, faturado, expedido ou cancelado. |
| `valor_total` (derivado) | Total financeiro do pedido | Não é digitado. Soma de quantidade x preco_unitario_praticado - desconto para todos os itens. |

#### ITEM_PEDIDO *(Entidade associativa)*
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `id_pedido + id_produto` (PK composta) | Identificador do item de pedido | Cada produto pode aparecer uma única vez em cada pedido. Para vender mais unidades, altera-se a quantidade do item existente. |
| `quantidade` | Unidades vendidas | Número inteiro obrigatório e maior que zero. |
| `preco_unitario_praticado` | Preço aplicado na venda | Fica congelado no momento do pedido. Alterações posteriores em preco_tabela não modificam vendas já registradas. |
| `desconto` | Desconto do item | Opcional. Não pode ser maior que quantidade x preco_unitario_praticado. |

#### CLIENTE
| Atributo | Descrição | Regra de Negócio Associada |
| :--- | :--- | :--- |
| `id_cliente` (PK) | Identificador do cliente | Chave primária gerada pelo sistema. |
| `nome` | Nome ou razão social | Obrigatório. Nome completo para pessoa física; razão social para pessoa jurídica. |
| `cpf_cnpj` | Documento do cliente | Obrigatório e único. Utilizado na análise de crédito. Dado protegido pela LGPD. |
| `inscricao_estadual` | Registro estadual | Obrigatória somente para perfil_cliente = Atacado/Lojista. Para Varejo Final, permanece vazia. |
| `email` | E-mail | Usado para comunicação e envio de nota fiscal eletrônica. |
| `telefone` | Telefone ou WhatsApp | Principal canal de contato. |
| `nome_comprador_responsavel` | Responsável por compras | Pessoa que realiza pedidos em nome do cliente, especialmente em vendas de atacado. |
| `perfil_cliente` | Categoria comercial | Aceita: Varejo Final ou Atacado/Lojista. Define regras de preço e de inscrição estadual. |
| `limite_credito` | Teto para compras a prazo | Aplicável a pagamentos em boleto. O valor total do pedido não pode ultrapassá-lo. |
| `status_aprovacao_financeira` | Situação do crédito | Aceita: Pendente, Aprovado ou Reprovado. Cliente lojista novo inicia como Pendente. |

### 9.2 Relacionamentos do Recorte (9 Entidades)
* **abastece:** `FORNECEDOR (1,N) — INSUMO (0,N)` (Relação N:N).
* **constitui:** `INSUMO (1,1) — FICHA_TECNICA (0,N)`.
* **detalha-se em:** `PRODUTO (1,1) — FICHA_TECNICA (0,N)`.
* **Especialização TD:** `PRODUTO` especializa-se em `BOLSA` / `ACESSORIO` de forma Total e Disjunta.
* **integra:** `PRODUTO (1,1) — ITEM_PEDIDO (0,N)`.
* **compreende:** `PEDIDO (1,1) — ITEM_PEDIDO (1,N)`.
* **efetua:** `CLIENTE (1,1) — PEDIDO (0,N)`.

### 9.4 Uso de Ferramentas de Apoio neste anexo
Utilizado suporte pontual para revisão de consistência entre dicionário e diagrama do recorte de 9 entidades.

---

## 10. Uso de Ferramentas de Apoio

* **Ferramenta e etapa:** Claude (Anthropic) — utilizada de forma pontual em momentos específicos: (1) estruturação inicial e formatação do dicionário de dados a partir do levantamento de requisitos; (2) revisão de consistência e padronização textual do README; e (3) auxílio na verificação da conformidade com a notação de Chen exigida pela disciplina.
* **Motivação:** Agilizar a formatação de artefatos textuais e garantir o alinhamento estrutural da documentação com o esqueleto oficial exigido.
* **Condução e Autoria:** O levantamento de requisitos, a definição das regras de negócio, a modelagem conceitual e as decisões de projeto foram integralmente conduzidos e validados pelo grupo. A ferramenta atuou estritamente como suporte técnico para editoração, revisão sintática e padronização de nomenclatura.
* **Reflexão crítica:** O modelo reflete fielmente o escopo da organização estudada. A utilização da ferramenta auxiliou na agilidade de formatação, mas a validação conceitual e a adequação às regras de negócio permaneceram sob total responsabilidade e critério da equipe.

---

## Conclusão

O levantamento de requisitos junto à Contrasti Bolsas e Acessórios confirmou as análises iniciais do grupo: os processos de estoque, custo de produção e vendas são operados de forma manual e descentralizada, carecendo de rastreabilidade formal por lote e de cálculos padronizados de ficha técnica. O modelo conceitual proposto — estruturado em 21 entidades no modelo completo e um recorte de 9 entidades focadas em custos e vendas — organiza esses processos em uma base coesa, utilizando entidades fracas e associativas exatamente onde a regra de negócio exige.

Como **principais contribuições**, o trabalho traduz um levantamento qualitativo para um modelo formal e verificável, evidenciando os valores derivados (custo de matéria-prima, preço de tabela e valor total do pedido) para que não sejam tratados como dados primários. 

Como **aprendizados**, o grupo destaca a importância de reificar relacionamentos N:N com atributos próprios desde a etapa conceitual e de documentar explicitamente as simplificações adotadas no escopo. 

Como **trabalhos futuros**, ficam pendentes: (1) a inclusão dos fluxogramas dos processos-chave; (2) a validação de campo das premissas de negócio adotadas como referência (limites de crédito, percentuais de comissão e prazos); e (3) a evolução do modelo conceitual para os modelos lógico e físico nas próximas etapas da disciplina.

---

## Referências Bibliográficas

Não foram utilizadas fontes bibliográficas externas. O modelo conceitual foi derivado exclusivamente do levantamento de requisitos obtido por pesquisa de campo (visita e entrevista) junto à Contrasti Bolsas e Acessórios Ltda (contato: Osmar Lingiardi), conforme registrado na Seção 1. 

---

## Critérios Atitudinais (20%)

A avaliação atitudinal (360º) foi conduzida de forma individual e confidencial por cada integrante do grupo, contemplando critérios como comprometimento, pontualidade, proatividade e colaboração no desenvolvimento do projeto. Os resultados consolidados foram submetidos conforme as diretrizes e canais oficiais da disciplina.
