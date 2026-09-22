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

## 5. Dicionário de Dados Conceitual (Preliminar — 21 Entidades)

*(As 21 entidades cobrem as 8 seções do levantamento de requisitos. O dicionário detalhado segue a mesma estrutura conceitual validada).*

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
