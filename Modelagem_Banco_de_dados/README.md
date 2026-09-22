# Modelagem de Banco de Dados — Contrasti Bolsas e Acessórios

## Metadados do Grupo

| Nome | RGM |
|---|---|
| Guilherme da Silva Ferreira Batista | 47302518 |
| Guilherme Petrucelli Domingos | 47270161 |
| Jaime Luiz de Oliveira Neto | 47336951 |
| Matheus Montagner | 47209470 |
| Vinicius Marques de Melo | 47213426 |

---

## Sumário
1. [Caracterização da Organização](#1-caracterização-da-organização)
2. [Processos de Negócio](#2-processos-de-negócio)
3. [Requisitos do Sistema](#3-requisitos-do-sistema)
4. [Regras de Negócio](#4-regras-de-negócio)
5. [Dicionário de Dados Conceitual (Preliminar)](#5-dicionário-de-dados-conceitual-preliminar)
6. [Modelagem Conceitual](#6-modelagem-conceitual-entidades-atributos-relacionamentos)
7. [Diagrama Entidade-Relacionamento (DER)](#7-diagrama-entidade-relacionamento-der)
8. [Justificativa Técnica](#8-justificativa-técnica)
9. [Anexo — Modelo Conceitual Simplificado (9 Entidades)](#9-anexo--modelo-conceitual-simplificado-9-entidades)
10. [Uso de Inteligência Artificial](#10-uso-de-inteligência-artificial)

---

## 1. Caracterização da Organização

* **Nome e natureza da organização:** Contrasti Bolsas e Acessórios Ltda - Fabricação de Bolsas. Empresa com fins lucrativos que fabrica e vende bolsas de couro (produção própria + venda direta e por canais diversos).
* **Contexto e porte:** Operação simultânea como indústria e comércio (loja física, e-commerce, WhatsApp e representantes). O uso de facções terceirizadas e o controle de aproveitamento de couro por corte indicam operação de pequeno a médio porte, com produção sob encomenda/lote. Volume médio mensal de 50 a 70 pedidos, com alta em datas comemorativas.
* **Problemas e necessidades identificados:** Processos descentralizados/manuais: controle de estoque de insumos sem rastreabilidade de lote; ausência de regra formal de crédito para vendas a prazo; cálculo de preço não padronizado; acompanhamento de produção sem visibilidade de status; falta de integração entre vendas, estoque e financeiro.
* **Justificativa da escolha:** Empresa de acesso facilitado e porte intermediário, ideal para o escopo do projeto.
* **Evidências da organização:** 
  * **Endereço:** Rua Alpiste, 116 - Jd. Eliane - São Paulo - SP.
  * **Contato:** Osmar Lingiard | **Telefone:** (11) 97334-4846 | **E-mail:** osmar@specia.com.br.
  * *Fotos/Links de comprovação:* PENDENTE de anexo no repositório.

---

## 2. Processos de Negócio

Principais processos mapeados:
1. **Cadastro e gestão de clientes:** Múltiplos endereços, perfil de preço (varejo/atacado) e aprovação de crédito.
2. **Cadastro e gestão de fornecedores:** Dados, categoria de insumo e histórico de avaliações.
3. **Ficha técnica e cadastro de produtos:** Modelo, variação (SKU) e lista de materiais (BOM) com cálculo de custos.
4. **Gestão de estoque e compras:** Entrada via XML de NF-e ou manual, unidades de medida, estoque mínimo e baixa automática.
5. **Ordem de produção e chão de fábrica:** Abertura de OP, execução por etapas via Kanban e controle de aproveitamento de couro.
6. **Vendas, pedidos e faturamento:** Pedidos multicanal, formas de pagamento, comissões e emissão de notas fiscais.
7. **Expedição e pós-venda:** Separação (*picking & packing*), conferência por código de barras, logística e RMA.
8. **Gestão financeira e relatórios:** Contas a pagar/receber automáticas e indicadores (Curva ABC, DRE, fluxo de caixa).

---

## 3. Requisitos do Sistema

### 3.1 Requisitos Funcionais
* **RF01:** Cadastrar clientes com razão social/nome, CNPJ/CPF, inscrição estadual, e-mail para NF-e, telefone/WhatsApp e comprador responsável.
* **RF02:** Registrar múltiplos endereços por cliente (matriz, entrega, cobrança).
* **RF03:** Diferenciar clientes por perfil (Varejo Final × Atacado/Lojista) com regras de preço distintas.
* **RF04:** Consultar restrição de CPF/CNPJ e aplicar limite de crédito para boleto, exigindo aprovação financeira para novos lojistas.
* **RF05:** Cadastrar fornecedores com categoria de insumo, contato do vendedor e prazo médio de entrega.
* **RF06:** Registrar entrada de insumos por lote, vinculando fornecedor, cor/tonalidade e preço pago (rastreabilidade).
* **RF07:** Manter histórico de compras e avaliação por fornecedor (preço, variação, pontualidade de entrega).
* **RF08:** Cadastrar produtos por modelo e variação (SKU), combinando cor, tipo de couro e tipo de ferragem.
* **RF09:** Manter a Ficha Técnica (BOM) de cada SKU, com a quantidade de cada insumo necessária.
* **RF10:** Calcular automaticamente o custo total e sugerir preço de venda a partir da Ficha Técnica, mão de obra e markup.
* **RF11:** Registrar entrada de matéria-prima via XML de NF-e ou digitação manual.
* **RF12:** Controlar o estoque de insumos nas unidades apropriadas, com estoque mínimo e alerta automático.
* **RF13:** Baixar automaticamente o estoque de insumos na abertura da Ordem de Produção.
* **RF14:** Abrir Ordens de Produção vinculadas a um SKU e acompanhar o status em painel Kanban.
* **RF15:** Registrar, por etapa de produção, o artesão/facção responsável e o percentual de aproveitamento/perda de couro no corte.
* **RF16:** Registrar pedidos de venda por canal (loja física, e-commerce, WhatsApp, representante) com pagamento e parcelamento.
* **RF17:** Calcular comissão de vendedores/representantes sobre pedidos faturados, com percentuais diferenciados por canal.
* **RF18:** Emitir NF-e (venda mercantil) e NFC-e (cupom varejo), nativamente ou por integração.
* **RF19:** Controlar separação, conferência (código de barras) e embalagem (checklist + dust bag) antes do envio.
* **RF20:** Integrar com serviços de logística (Correios, Melhor Envio, transportadoras parceiras) para etiquetas e rastreio.
* **RF21:** Registrar trocas, devoluções e garantias (RMA) vinculadas ao item efetivamente entregue.
* **RF22:** Gerar lançamentos automáticos de Contas a Receber (vendas faturadas) e Contas a Pagar (compras).
* **RF23:** Gerar relatórios gerenciais (Curva ABC, margem de lucro, DRE, fluxo de caixa, estoque parado).
* **RF24:** Restringir o acesso por perfil de usuário (Vendedor, Gerente de Produção, Financeiro, Administrador).

### 3.2 Requisitos Não Funcionais
* **RNF01 (Desempenho):** Consultas de estoque e ficha técnica devem responder rápido para não atrasar a produção.
* **RNF02 (Segurança):** Controle de acesso por perfil e proteção de dados pessoais (LGPD).
* **RNF03 (Disponibilidade):** O módulo de estoque/OP precisa estar disponível no horário produtivo, pois a baixa é bloqueante.
* **RNF04 (Usabilidade):** O painel Kanban deve ser operável por artesãos e facções sem treinamento extenso.
* **RNF05 (Integridade transacional):** Baixa de estoque, financeiro e emissão fiscal devem ocorrer de forma atômica.
* **RNF06 (Auditabilidade):** Histórico de preços por fornecedor e status de OP preservados para consultas futuras.

---

## 4. Regras de Negócio

* **Regras Operacionais:**
  * Inscrição Estadual é obrigatória apenas quando o perfil do cliente é Atacado/Lojista.
  * Toda variação de produto (SKU) pertence a exatamente um modelo.
  * A baixa de estoque de insumo ocorre automaticamente na abertura da Ordem de Produção.
  * Todo insumo recebido é registrado com lote e fornecedor de origem para rastreabilidade de cor/textura.
  * Comissão padrão de vendedor é de 5%, com percentuais diferenciados entre atacado e varejo.
  * Um chamado de garantia (RMA) está sempre associado a um item de pedido específico, nunca ao pedido inteiro.
  * Um pedido só é expedido após checklist de saída e leitura de código de barras.

* **Restrições Organizacionais:**
  * Exigência legal de emissão de NF-e ou NFC-e conforme o canal de venda.
  * Tratamento de dados pessoais conforme a LGPD.
  * O prazo médio de entrega (lead time) de cada fornecedor impacta o planejamento de compras.
  * O pagamento de artesãos/facções terceirizadas depende do registro fiel da etapa executada.

---

## 5. Dicionário de Dados Conceitual (Preliminar)

> *O modelo completo abrange 21 entidades estruturadas.*

* **CLIENTE:** `cod_cliente` (PK), `razao_social_nome`, `cnpj_cpf`, `inscricao_estadual`, `email_nfe`, `telefone_whatsapp`, `nome_comprador_responsavel`, `perfil_cliente`, `limite_credito`, `status_aprovacao_financeira`.
* **ENDERECO_CLIENTE:** `cod_endereco` (PK), `cod_cliente` (FK), `tipo_endereco`, `logradouro`, `cidade`, `uf`, `cep`.
* **FORNECEDOR:** `cod_fornecedor` (PK), `razao_social`, `cnpj`, `inscricao_estadual`, `categoria_insumo`, `contato_vendedor`, `prazo_medio_entrega_dias`.
* **INSUMO:** `cod_insumo` (PK), `descricao_insumo`, `categoria_insumo`, `unidade_medida`, `estoque_minimo`, `saldo_estoque_atual`.
* **LOTE_INSUMO:** `cod_lote` (PK), `cod_insumo` (FK), `cod_fornecedor` (FK), `cod_compra` (FK), `numero_lote`, `data_recebimento`, `quantidade_recebida`, `cor_tonalidade`, `preco_pago`.
* **COMPRA:** `cod_compra` (PK), `cod_fornecedor` (FK), `numero_compra`, `data_compra`, `forma_entrada`, `valor_total`, `taxa_pontualidade_entrega`.
* **CONTAS_PAGAR:** `cod_titulo_pagar` (PK), `cod_compra` (FK), `valor`, `data_vencimento`, `data_pagamento`, `status_pagamento`.
* **MODELO_PRODUTO:** `cod_modelo` (PK), `nome_modelo`, `descricao`.
* **VARIACAO_PRODUTO (SKU):** `cod_sku` (PK), `cod_modelo` (FK), `codigo_sku`, `cor`, `tipo_couro`, `tipo_ferragem`, `preco_venda_sugerido`, `tempo_estimado_mao_obra_min`.
* **ITEM_FICHA_TECNICA:** `cod_sku` (PK/FK), `cod_insumo` (PK/FK), `quantidade_necessaria`, `unidade_medida_item`.
* **ARTESAO_FACCAO:** `cod_artesao` (PK), `nome`, `tipo_vinculo`, `telefone`.
* **ETAPA_PRODUCAO:** `cod_etapa` (PK), `nome_etapa`, `ordem_sequencial`.
* **ORDEM_PRODUCAO:** `cod_op` (PK), `cod_sku` (FK), `numero_op`, `data_abertura`, `quantidade_produzir`, `status_kanban`.
* **EXECUCAO_ETAPA:** `cod_execucao` (PK), `cod_op` (FK), `cod_etapa` (FK), `cod_artesao` (FK), `data_inicio`, `data_fim`, `percentual_aproveitamento`, `percentual_perda`.
* **USUARIO:** `cod_usuario` (PK), `nome`, `login`, `senha_hash`, `perfil_acesso`, `tipo_vendedor`, `percentual_comissao`.
* **PEDIDO_VENDA:** `cod_pedido` (PK), `cod_cliente` (FK), `cod_usuario` (FK), `numero_pedido`, `data_pedido`, `canal_venda`, `forma_pagamento`, `condicao_parcelamento`, `status_pedido`.
* **ITEM_PEDIDO:** `cod_item_pedido` (PK), `cod_pedido` (FK), `cod_sku` (FK), `quantidade`, `preco_unitario_praticado`, `desconto`.
* **NOTA_FISCAL:** `cod_nf` (PK), `cod_pedido` (FK), `numero_nf`, `tipo_nf`, `data_emissao`, `valor_total`.
* **CONTAS_RECEBER:** `cod_titulo_receber` (PK), `cod_pedido` (FK), `valor`, `data_vencimento`, `data_recebimento`, `status_recebimento`.
* **EXPEDICAO:** `cod_expedicao` (PK), `cod_pedido` (FK), `data_envio`, `transportadora`, `codigo_rastreio`, `checklist_conferencia`, `dust_bag_incluso`.
* **RMA_GARANTIA:** `cod_rma` (PK), `cod_item_pedido` (FK), `data_abertura`, `motivo_defeito`, `status_rma`, `data_devolucao`.

---

## 6. Modelagem Conceitual (Entidades, Atributos, Relacionamentos)

* **Entidades reconhecidas (21 no total):** Divididas em blocos de clientes, suprimentos, produtos, produção, vendas e pós-venda. Inclui 2 entidades fracas (`ENDERECO_CLIENTE`, `LOTE_INSUMO`) e 3 entidades associativas (`ITEM_FICHA_TECNICA`, `EXECUCAO_ETAPA`, `ITEM_PEDIDO`).
* **Relacionamentos (22 no total):** Mapeados para garantir a integridade das regras operacionais, como limites de crédito, rastreabilidade de lotes e pagamentos por produção.

---

## 7. Diagrama Entidade-Relacionamento (DER)

* Os arquivos oficiais do DER em notação de Chen encontram-se salvos na pasta **`diagrams/`** do repositório.
* O modelo representa graficamente as 21 entidades, atributos, chaves primárias sublinhadas e cardinalidades exatas.

---

## 8. Justificativa Técnica

* **Abstração escolhida:** A adoção de 21 entidades justifica-se pela necessidade de isolar ciclos de vida distintos (ex: separar `LOTE_INSUMO` de `INSUMO` para rastrear cores e texturas com precisão). 
* **Uso de entidades associativas e fracas:** Evita chaves artificiais desnecessárias e acomoda atributos nativos de junção (como percentuais de perda no corte e preços praticados congelados no momento da venda).

---

## 9. Anexo — Modelo Conceitual Simplificado (9 Entidades)

Recorte simplificado cobrindo o fluxo de custos, produção e vendas (disponibilizado em PDF e HTML na pasta **`docs/`**):
* **Entidades do recorte:** `FORNECEDOR`, `INSUMO`, `FICHA_TECNICA`, `PRODUTO` (especializado em `BOLSA` e `ACESSORIO`), `CLIENTE`, `PEDIDO` e `ITEM_PEDIDO`.

---

## 10. Uso de Inteligência Artificial

Documentação obrigatória do suporte tecnológico:

| Item | Descrição do Registro |
|---|---|
| **Ferramenta e Etapa** | Claude (Anthropic) — Utilizado na estruturação dos requisitos, conversão para o modelo conceitual de 21 entidades, adequação à notação de Chen e redação da documentação. |
| **Motivação** | Agilizar a formatação dos dados do levantamento de campo para o padrão metodológico exigido pela disciplina. |
| **Prompts Principais** | Solicitações de criação e refinamento de dicionários de dados e esquemas de diagramas. |
| **Correções manuais** | Ajustes estruturais finos na remoção visual de chaves estrangeiras de diagramas conceituais puros e alinhamento às regras reais da empresa analisada. |
