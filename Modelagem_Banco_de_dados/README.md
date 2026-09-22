# Modelagem de Banco de Dados — Contrasti Bolsas e Acessórios (Modelo de 9 Entidades)

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
5. [Dicionário de Dados Conceitual (9 Entidades)](#5-dicionário-de-dados-conceitual-9-entidades)
6. [Modelagem Conceitual](#6-modelagem-conceitual-e-relacionamentos)
7. [Diagrama Entidade-Relacionamento (DER)](#7-diagrama-entidade-relacionamento-der)
8. [Justificativa Técnica](#8-justificativa-técnica)
9. [Uso de Inteligência Artificial](#9-uso-de-inteligência-artificial)

---

## 1. Caracterização da Organização
* **Nome e natureza da organização:** Contrasti Bolsas e Acessórios Ltda - Fabricação de Bolsas (Indústria e Comércio de Bolsas de Couro).
* **Contexto e porte:** Empresa com fins lucrativos que atua na fabricação própria e comercialização de bolsas e acessórios de couro[cite: 4].
* **Evidências da organização:** 
  * **Endereço:** Rua Alpiste, 116 - Jd. Eliane - São Paulo - SP.
  * **Contato:** Osmar Lingiard | **Telefone:** (11) 97334-4846 | **E-mail:** osmar@specia.com.br.

---

## 2. Processos de Negócio
Mapeamento do fluxo central (da matéria-prima ao pedido faturado)[cite: 4]:
1. **Entrada de matéria-prima:** Gestão de fornecedores e insumos (couro, zíperes, fivelas e forros) com controle de estoque e custo unitário de referência[cite: 4].
2. **Engenharia de produto (Ficha Técnica):** Lista de materiais por produto, determinando a quantidade necessária e o percentual de perda técnica no corte[cite: 4].
3. **Formação de preço:** Cálculo derivado do custo de matéria-prima e mão de obra, aplicando o *markup* para sugerir o preço de tabela[cite: 4].
4. **Hierarquia de Produtos (Especialização TD):** Classificação dos produtos em Bolsas (com atributos como tamanho, cor, tipo de alça e peças de composição) e Acessórios[cite: 4].
5. **Saída comercial:** Cadastro de clientes com controle de limite de crédito e aprovação financeira, além do registro de pedidos de venda com preços congelados no momento da transação[cite: 4].

---

## 3. Requisitos do Sistema
* **RF01:** Cadastrar clientes com dados fiscais, perfil (Varejo Final ou Atacado/Lojista), limite de crédito e status de aprovação financeira[cite: 4].
* **RF02:** Cadastrar fornecedores vinculados às categorias de insumos e prazos médios de entrega[cite: 4].
* **RF03:** Gerenciar insumos controlando estoque mínimo, estoque atual e custo unitário de referência[cite: 4].
* **RF04:** Manter a Ficha Técnica (BOM) associando produtos aos insumos necessários e ao percentual de perda técnica[cite: 4].
* **RF05:** Calcular automaticamente o custo de matéria-prima e o preço de tabela do produto[cite: 4].
* **RF06:** Registrar pedidos de venda multicanal associados a clientes, validando formas de pagamento, parcelamentos, limites de crédito e congelando o preço unitário praticado[cite: 4].

---

## 4. Regras de Negócio
* A inscrição estadual é obrigatória apenas quando o perfil do cliente é Atacado/Lojista[cite: 4].
* O custo da matéria-prima é calculado somando, para cada insumo da ficha, a quantidade multiplicada por $(1 + \text{perda})$ e pelo custo unitário[cite: 4].
* O preço de tabela é derivado da soma do custo de matéria-prima com o custo de mão de obra, multiplicado pelo *markup*[cite: 4].
* Para compras via boleto, o cliente precisa estar com o status Aprovado e o valor total do pedido deve respeitar o limite de crédito[cite: 4].
* O preço unitário praticado e os descontos no item do pedido ficam congelados no momento da venda, não sendo afetados por reajustes posteriores[cite: 4].

---

## 5. Dicionário de Dados Conceitual (9 Entidades)

### FORNECEDOR[cite: 4]
* `id_fornecedor`: Chave primária, gerada pelo sistema[cite: 4].
* `razao social`: Nome empresarial (obrigatório)[cite: 4].
* `cnpj`: Obrigatório e único (protegido pela LGPD)[cite: 4].
* `inscricao estadual`: Registro estadual (opcional)[cite: 4].
* `email`: E-mail comercial[cite: 4].
* `telefone`: Contato direto[cite: 4].
* `contato_vendedor`: Nome da pessoa de referência[cite: 4].
* `categoria_insumo`: Domínio restrito (Curtume/Couro, Ferragens/Fivelas, Zíperes/Aviamentos, Embalagens/Caixas)[cite: 4].
* `prazo_medio_entrega_dias`: Prazo de entrega em dias[cite: 4].

### INSUMO[cite: 4]
* `id_insumo`: Chave primária[cite: 4].
* `nome_insumo`: Nome do material (obrigatório)[cite: 4].
* `categoria_insumo`: Grupo do material[cite: 4].
* `unidade_medida`: Domínio ($\text{dm}^2$, $\text{m}^2$, unidade, metro, kg, litro)[cite: 4].
* `estoque_minimo`: Saldo mínimo para alerta de recompra[cite: 4].
* `estoque_atual`: Saldo atual em estoque[cite: 4].
* `custo_unitario`: Custo por unidade de medida[cite: 4].

### FICHA_TECNICA (Associativa)[cite: 4]
* `quantidade_necessaria`: Quantidade por peça na unidade do insumo[cite: 4].
* `percentual_perda`: Perda técnica no corte (fração de 0 a 1)[cite: 4].

### PRODUTO[cite: 4]
* `id_produto`: Chave primária[cite: 4].
* `nome_modelo`: Nome do modelo (obrigatório)[cite: 4].
* `custo_mao_obra`: Valor informado para corte e costura[cite: 4].
* `markup`: Multiplicador de margem (> 1)[cite: 4].
* `custo_materia_prima` (derivado): Custo dos insumos calculado[cite: 4].
* `preco_tabela` (derivado): Preço de venda sugerido[cite: 4].

### BOLSA (Especialização)[cite: 4]
* `tamanho`: Dimensão da bolsa[cite: 4].
* `cor`: Cor da bolsa[cite: 4].
* `tipo_alca`: Tipo de alça[cite: 4].
* `pecas_composicao`: Atributo multivalorado (Tampa, Frente, Costa, Fundo, Orla)[cite: 4].

### ACESSORIO (Especialização)[cite: 4]
* `tipo_peca`: Tipo do acessório (obrigatório)[cite: 4].

### CLIENTE[cite: 4]
* `id_cliente`: Chave primária[cite: 4].
* `nome`: Razão social ou nome[cite: 4].
* `cpf_cnpj`: CPF ou CNPJ único[cite: 4].
* `inscricao_estadual`: Obrigatória se Atacado/Lojista[cite: 4].
* `email`: E-mail para envio de NF-e[cite: 4].
* `telefone`: Contato principal[cite: 4].
* `nome_comprador_responsavel`: Contato de compras[cite: 4].
* `perfil_cliente`: Varejo Final ou Atacado/Lojista[cite: 4].
* `limite credito`: Teto de compra a prazo[cite: 4].
* `status_aprovacao_financeira`: Pendente, Aprovado ou Reprovado[cite: 4].

### PEDIDO[cite: 4]
* `id_pedido`: Chave primária[cite: 4].
* `data_pedido`: Data do registro[cite: 4].
* `canal_venda`: Loja Física, E-commerce, WhatsApp ou Representante[cite: 4].
* `forma_pagamento`: PIX, Cartão ou Boleto[cite: 4].
* `condicao_parcelamento`: Prazos ou parcelas[cite: 4].
* `status_pedido`: Situação do pedido[cite: 4].
* `valor_total` (derivado): Total do pedido calculado[cite: 4].

### ITEM_PEDIDO (Associativa)[cite: 4]
* `quantidade`: Unidades vendidas[cite: 4].
* `preco_unitario_praticado`: Preço congelado na venda[cite: 4].
* `desconto`: Desconto do item em R$[cite: 4].

---

## 6. Modelagem Conceitual e Relacionamentos
* **Entidades:** 9 entidades centrais, incorporando a especialização Total e Disjunta (TD) de `PRODUTO` em `BOLSA` e `ACESSORIO`[cite: 4].
* **Relacionamentos Principais:**
  * `fornece` (N:N entre `FORNECEDOR` e `INSUMO`)[cite: 4]
  * `compõe` e `é detalhado em` (resolvem a lista de materiais em `FICHA_TECNICA`)[cite: 4]
  * `é vendido em` e `contém` (ligam `PRODUTO`, `ITEM_PEDIDO` e `PEDIDO`)[cite: 4]
  * `realiza` (liga `CLIENTE` a `PEDIDO`)[cite: 4]

---

## 7. Diagrama Entidade-Relacionamento (DER)
* O DER correspondente a este recorte de 9 entidades (com notação de Chen, cardinalidades em min/max, atributos derivados em elipse tracejada e especialização TD) encontra-se na pasta **`diagrams/`** como `DER_Diagrama_9_Entidades.pdf`[cite: 4].

---

## 8. Justificativa Técnica
* O modelo de 9 entidades foca estritamente no núcleo de suprimentos, engenharia de produto, formação de preços e faturamento comercial, simplificando a complexidade operacional para atender perfeitamente aos requisitos centrais do escopo sem incluir entidades de suporte avançado[cite: 4].

---

## 9. Uso de Inteligência Artificial
* Documentação do uso de ferramentas de IA (Claude) para a estruturação e formatação do dicionário de dados e diagramas conceituais alinhados ao recorte de 9 entidades[cite: 4].
