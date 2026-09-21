# Sistema de Gestão para Indústria e Comércio de Bolsas e Acessórios
**Projeto Acadêmico — Análise e Desenvolvimento de Sistemas (ADS)**  
**Empresa Analisada:** Contrasti Bolsas e Acessórios Ltda (Marca Comercial: *Lingiardi*)

---

## Metadados do Grupo

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
- **Contexto e porte:** Operação de pequeno a médio porte com fabricação artesanal (envolvendo ateliê próprio e facções terceirizadas). Volume médio de 50 a 70 pedidos mensais, com picos sazonais no Dia das Mães e Natal.
- **Problemas e necessidades identificados:** Processos anteriormente descentralizados ou manuais, incluindo controle de estoque de insumos sem rastreabilidade de lote, ausência de regras formais de crédito, cálculo de custo sem padronização, acompanhamento de produção sem visibilidade clara e falta de integração entre vendas e financeiro.
- **Evidências da organização:** 
  - **Endereço Sede Fiscal:** Rua Alpiste, 116 - Jd. Eliane - São Paulo - SP.
  - **Endereço Ateliê / Showroom:** R. Dr. João Ribeiro, 185 — Penha de França, São Paulo - SP.
  - **Contato:** Osmar Lingiardi | Telefone: (11) 97334-4846 | E-mail: osmar@specia.com.br.

---

## 2. Processos de Negócio

1. **Cadastro e gestão de clientes** — Múltiplos endereços, perfis de preço (varejo/atacado) e aprovação de crédito.
2. **Cadastro e gestão de fornecedores** — Dados cadastrais, categorias de insumos e histórico de entregas.
3. **Ficha técnica e cadastro de produtos** — Estrutura de modelo, variação (SKU) e lista de materiais (BOM).
4. **Gestão de estoque e compras** — Entrada de insumos, controle por unidades de medida e alertas de estoque mínimo.
5. **Ordem de produção e chão de fábrica** — Acompanhamento por painel Kanban e controle de aproveitamento de couro no corte.
6. **Vendas, pedidos e faturamento** — Pedidos multicanal, formas de pagamento e emissão de notas fiscais.
7. **Expedição e pós-venda** — Conferência por código de barras, logística integrada e controle de RMA/garantias.
8. **Gestão financeira** — Contas a pagar e receber integradas às operações de compra e venda.

---

## 3. Requisitos do Sistema

### 3.1 Requisitos Funcionais (Resumo Principal)
* **RF01 a RF04:** Gestão completa de clientes, múltiplos endereços, segmentação de perfis (Varejo x Atacado) e controle de limite de crédito.
* **RF05 a RF07:** Cadastro de fornecedores, controle de entradas de insumos por lote e histórico de pontualidade.
* **RF08 a RF10:** Gestão de produtos por SKU, manutenção de Ficha Técnica (BOM) e cálculo automatizado de custos e preços de venda.
* **RF11 a RF15:** Controle de estoque de matérias-primas, baixa automática na abertura de Ordens de Produção (OP) e monitoramento via Kanban por etapas produtivas.
* **RF16 a RF24:** Vendas multicanal, emissão de documentos fiscais, logística de expedição, automação financeira (Contas a Pagar/Receber) e controle de acesso por perfis de usuário.

### 3.2 Requisitos Não Funcionais (RNF)
* **RNF01 (Desempenho):** Consultas rápidas de estoque e ficha técnica para não atrasar a abertura de OPs.
* **RNF02 (Segurança):** Conformidade com a LGPD e controle rígido de acesso por perfil.
* **RNF03 a RNF06 (Disponibilidade e Integridade):** Alta disponibilidade nos módulos de produção e transações atômicas para garantir a integridade entre estoque, financeiro e faturamento.

---

## 4. Regras de Negócio

- **Rastreabilidade de Lote:** Todo insumo recebido (couro, ferragens) é vinculado a um lote de origem para garantir o controle rigoroso de cor e tonalidade entre as peças de uma mesma coleção.
- **Herança (T, D):** A especialização da superclasse `PRODUTO` nas subclasses `BOLSA` e `ACESSORIO` é do tipo Total (todo produto pertence a uma categoria) e Disjunta (um item não pode ser simultaneamente bolsa e acessório).
- **Controle Financeiro:** O preço unitário praticado no item do pedido é congelado no ato da compra para preservar o histórico financeiro da empresa.

---

## 5. Dicionário de Dados Conceitual (Entidades Principais)

1. **FORNECEDOR:** `CNPJ (PK)`, `Nome`, `Telefone`.
2. **INSUMO:** `Cod_Insumo (PK)`, `Descricao`, `Unidade_Medida`.
3. **LOTE_INSUMO:** `Codigo_Lote (PK)`, `Cor_Tonalidade`, `Quantidade`.
4. **PRODUTO (Superclasse):** `ID_Produto (PK)`, `Nome`, `Preco_Venda`.
5. **BOLSA (Subclasse):** `ID_Produto (PK, FK)`, `Tamanho`, `Cor`, `Tipo_Alca`.
6. **ACESSORIO (Subclasse):** `ID_Produto (PK, FK)`, `Tipo_Peca`.
7. **PEDIDO:** `Codigo_Pedido (PK)`, `Data_Compra`, `Valor_Total`.

---

## 6. Diagrama Entidade-Relacionamento (DER em Notação de Chen)

O diagrama conceitual foi estruturado em formato horizontal (da esquerda para a direita), priorizando a clareza acadêmica e eliminando ruídos operacionais secundários para evidenciar o ciclo produtivo principal da fábrica.

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