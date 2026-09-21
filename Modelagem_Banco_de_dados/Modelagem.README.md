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

- **Nome e natureza da organização:** Contrasti Bolsas e Acessórios Ltda (CNPJ: 49.269.600/0001-49) - Fabricação de Bolsas. Empresa com fins lucrativos (EPP) que opera simultaneamente como indústria e comércio de bolsas e acessórios de couro legítimo, realizando tanto a produção própria quanto vendas multicanal.
- **Contexto e porte:** Operação de pequeno a médio porte com fabricação artesanal (ateliê de produção própria localizado na Penha de França, SP, e suporte de facções terceirizadas). O volume médio opera entre 50 a 70 pedidos mensais, com picos sazonais importantes no Dia das Mães e no Natal.
- **Problemas e necessidades identificados:** O levantamento de requisitos aponta que os processos operacionais eram descentralizados e manuais, destacando:
  - Controle de estoque de insumos (couro, ferragens, zíperes) sem rastreabilidade de lote por tonalidade.
  - Ausência de regras formais de análise e limite de crédito para vendas a prazo a lojistas.
  - Cálculo de custo e preço de venda sem padronização estruturada (falta de Ficha Técnica / BOM).
  - Acompanhamento de chão de fábrica sem visibilidade gerencial de status.
  - Falta de integração sistêmica entre os módulos de vendas, controle de estoque e financeiro (geração manual de títulos a pagar e receber).
- **Justificativa da escolha:** Escolhida por ser uma empresa de um dos integrantes do grupo com acesso facilitado a dados reais de funcionamento, apresentando um porte ideal (nem excessivamente simples e nem complexa demais), permitindo modelar com precisão o cenário de manufatura e comércio.
- **Evidências e Dados da Organização:** 
  - **Sede Fiscal:** Rua Alpiste, 116 - Jd. Eliane - São Paulo - SP.
  - **Ateliê / Showroom:** Rua Dr. João Ribeiro, 185 — Penha de França, São Paulo - SP.
  - **Proprietário / Contato Principal:** Osmar Lingiardi.
  - **Telefone:** (11) 97334-4846.
  - **E-mail:** osmar@specia.com.br.

---

## 2. Processos de Negócio

Principais processos mapeados (extraídos do levantamento de requisitos, cobrindo os blocos operacionais):

1. **Cadastro e gestão de clientes** — Cadastro completo com múltiplos endereços (matriz, entrega e cobrança), segmentação de perfil de preço (Varejo Final × Atacado/Lojista) e fluxo de aprovação de crédito.
2. **Cadastro e gestão de fornecedores** — Dados cadastrais de curtumes e fornecedores de ferragens/aviamentos, categorias de insumos e histórico de entregas.
3. **Ficha técnica e cadastro de produtos** — Estrutura de modelo de bolsa, variações por SKU (cor, tipo de couro, tipo de ferragem) e Lista de Materiais (BOM) para cálculo exato de custos.
4. **Gestão de estoque e compras** — Entrada de insumos (via XML de NF-e ou manual), controle por unidades de medida específicas (dm², metros, unidades), pontos de estoque mínimo e baixa automática.
5. **Ordem de produção e chão de fábrica** — Abertura de OPs, controle de execução por etapas produtivas (Modelagem, Corte, Colagem, Costura, Montagem, Acabamento e Embalagem) via painel Kanban, e monitoramento do percentual de aproveitamento do couro.
6. **Vendas, pedidos e faturamento** — Gestão de pedidos multicanal (loja física, e-commerce, WhatsApp, representantes), cálculo automatizado de comissões e emissão fiscal (NF-e e NFC-e).
7. **Expedição e pós-venda** — Controle de separação (*picking & packing*), conferência por código de barras, checklist com inclusão de *dust bag* e gestão de RMA (garantias e trocas).
8. **Gestão financeira e relatórios gerenciais** — Contas a Pagar e Contas a Receber geradas automaticamente a partir de compras e vendas faturadas, além de indicadores gerenciais (Curva ABC, margem de lucro por modelo, fluxo de caixa).

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
  - Todo insumo recebido é registrado com lote e fornecedor de origem, para garantir rastreabilidade de cor e tonalidade do couro entre peças da mesma coleção.
  - Comissão padrão de vendedor é de 5% sobre pedidos faturados, com percentual diferenciado entre canal de atacado e varejo.
  - Um chamado de garantia (RMA) está sempre associado a um item de pedido específico, nunca ao pedido inteiro.
  - Um pedido só é expedido depois de checklist de saída e conferência dos itens via leitura de código de barras.

- **Restrições organizacionais:**
  - Exigência legal de emissão de NF-e (venda mercantil) ou NFC-e (cupom fiscal varejo), conforme o canal de venda.
  - Dados pessoais de clientes e fornecedores exigem tratamento estrito conforme a LGPD.
  - O prazo médio de entrega de cada fornecedor impacta diretamente o planejamento do lead time de produção.
  - O pagamento de artesãos e facções terceirizadas depende do registro fiel da execução de cada etapa produtiva.

---

## 5. Dicionário de Dados Conceitual (Entidades Principais)

*(O projeto contempla um total de 21 entidades integradas cobrindo todo o ciclo operacional da empresa. Abaixo estão as entidades estruturais fundamentais do fluxo principal).*

### CLIENTE
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_cliente | Identificador interno do cliente | Chave primária |
| razao_social_nome | Razão social (PJ) ou nome completo (PF) | Obrigatório |
| cnpj_cpf | Documento fiscal do cliente | Usado na consulta de restrição de crédito |
| perfil_cliente | Classificação comercial | Domínio: Varejo Final / Atacado-Lojista |
| limite_credito | Teto de faturamento via boleto | Referência: R$ 10.000,00 |

### FORNECEDOR
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_fornecedor | Identificador do fornecedor | Chave primária |
| razao_social | Nome empresarial do curtume/fornecedor | — |
| cnpj | Documento fiscal | — |
| categoria_insumo | Tipo de insumo fornecido | Couro, Ferragens, Zíperes, Embalagens |

### INSUMO
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_insumo | Identificador do insumo | Chave primária |
| descricao_insumo | Nome do insumo (ex.: Couro Bovino Caramelo) | — |
| unidade_medida | Unidade de controle | dm², metros, unidades |

### LOTE_INSUMO *(entidade fraca)*
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_lote | Identificador do lote | Chave primária |
| numero_lote | Identificação do lote do fabricante | Garante rastreabilidade de tonalidade do couro |
| cor_tonalidade | Cor/tonalidade específica da pele | — |
| quantidade | Quantidade recebida | — |

### PRODUTO *(Superclasse)*
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_produto | Identificador do produto | Chave primária |
| nome | Nome base do produto | — |
| preco_venda | Preço sugerido de comercialização | Calculado via Ficha Técnica e markup |

### BOLSA *(Subclasse)*
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_produto | Identificador do produto | Chave primária e Estrangeira |
| tamanho | Dimensões da bolsa | P, M, G |
| cor | Cor predominante | — |
| tipo_alca | Modelo da alça | Transversal, Ombro, Mão |

### ACESSORIO *(Subclasse)*
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_produto | Identificador do produto | Chave primária e Estrangeira |
| tipo_peca | Classificação do acessório | Carteira, Cinto, Chaveiro |

### PEDIDO_VENDA
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| cod_pedido | Identificador do pedido | Chave primária |
| data_pedido | Data da compra | — |
| valor_total | Valor total da transação comercial | — |

---

## 6. Modelagem Conceitual (Entidades, Atributos, Relacionamentos)

- **Entidades reconhecidas:** 21 entidades estruturadas em blocos lógicos (Clientes, Suprimentos, Engenharia/Produtos, Chão de Fábrica, Comercial e Financeiro/Pós-venda).
- **Regra de Especialização:** Aplicada na superclasse `PRODUTO` com restrição **Total e Disjunta (T, D)**, garantindo que todo item do catálogo pertença de forma exclusiva às subclasses `BOLSA` ou `ACESSORIO`.

---

## 7. Diagrama Entidade-Relacionamento (DER em Notação de Chen)

O diagrama conceitual foi estruturado em formato horizontal (da esquerda para a direita), priorizando a clareza acadêmica e exibindo as entidades principais, relacionamentos em losangos laranjas e atributos em caixas verdes.

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
