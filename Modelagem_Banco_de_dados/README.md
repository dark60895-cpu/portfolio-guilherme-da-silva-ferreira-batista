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
* **Contexto e porte:** Empresa com fins lucrativos que atua na fabricação própria e comercialização de bolsas e acessórios de couro.
* **Evidências da organização:** 
  * **Endereço:** Rua Alpiste, 116 - Jd. Eliane - São Paulo - SP.
  * **Contato:** Osmar Lingiard | **Telefone:** (11) 97334-4846 | **E-mail:** osmar@specia.com.br.

---

## 2. Processos de Negócio
Mapeamento do fluxo central (da matéria-prima ao pedido faturado):
1. **Entrada de matéria-prima:** Gestão de fornecedores e insumos (couro, zíperes, fivelas e forros) com controle de estoque e custo unitário de referência.
2. **Engenharia de produto (Ficha Técnica):** Lista de materiais por produto, determinando a quantidade necessária e o percentual de perda técnica no corte.
3. **Formação de preço:** Cálculo derivado do custo de matéria-prima e mão de obra, aplicando o *markup* para sugerir o preço de tabela.
4. **Hierarquia de Produtos (Especialização TD):** Classificação dos produtos em Bolsas (com atributos como tamanho, cor, tipo de alça e peças de composição) e Acessórios.
5. **Saída comercial:** Cadastro de clientes com controle de limite de crédito e aprovação financeira, além do registro de pedidos de venda com preços congelados no momento da transação.

---

## 3. Requisitos do Sistema
* **RF01:** Cadastrar clientes com dados fiscais, perfil (Varejo Final ou Atacado/Lojista), limite de crédito e status de aprovação financeira.
* **RF02:** Cadastrar fornecedores vinculados às categorias de insumos e prazos médios de entrega.
* **RF03:** Gerenciar insumos controlando estoque mínimo, estoque atual e custo unitário de referência.
* **RF04:** Manter a Ficha Técnica (BOM) associando produtos aos insumos necessários e ao percentual de perda técnica.
* **RF05:** Calcular automaticamente o custo de matéria-prima e o preço de tabela do produto.
* **RF06:** Registrar pedidos de venda multicanal associados a clientes, validando formas de pagamento, parcelamentos, limites de crédito e congelando o preço unitário praticado.

---

## 4. Regras de Negócio
* A inscrição estadual é obrigatória apenas quando o perfil do cliente é Atacado/Lojista.
* O custo da matéria-prima é calculado somando, para cada insumo da ficha, a quantidade multiplicada por $(1 + \text{perda})$ e pelo custo unitário.
* O preço de tabela é derivado da soma do custo de matéria-prima com o custo de mão de obra, multiplicado pelo *markup*.
* Para compras via boleto, o cliente precisa estar com o status Aprovado e o valor total do pedido deve respeitar o limite de crédito.
* O preço unitário praticado e os descontos no item do pedido ficam congelados no momento da venda, não sendo afetados por reajustes posteriores.

---

## 5. Dicionário de Dados Conceitual (9 Entidades)

### FORNECEDOR
* `id_fornecedor`: Chave primária, gerada pelo sistema.
* `razao social`: Nome empresarial (obrigatório).
* `cnpj`: Obrigatório e único (protegido pela LGPD).
* `inscricao estadual`: Registro estadual (opcional).
* `email`: E-mail comercial.
* `telefone`: Contato direto.
* `contato_vendedor`: Nome da pessoa de referência.
* `categoria_insumo`: Domínio restrito (Curtume/Couro, Ferragens/Fivelas, Zíperes/Aviamentos, Embalagens/Caixas).
* `prazo_medio_entrega_dias`: Prazo de entrega em dias.

### INSUMO
* `id_insumo`: Chave primária.
* `nome_insumo`: Nome do material (obrigatório).
* `categoria_insumo`: Grupo do material.
* `unidade_medida`: Domínio ($\text{dm}^2$, $\text{m}^2$, unidade, metro, kg, litro).
* `estoque_minimo`: Saldo mínimo para alerta de recompra.
* `estoque_atual`: Saldo atual em estoque.
* `custo_unitario`: Custo por unidade de medida.

### FICHA_TECNICA (Associativa)
* `quantidade_necessaria`: Quantidade por peça na unidade do insumo.
* `percentual_perda`: Perda técnica no corte (fração de 0 a 1).

### PRODUTO
* `id_produto`: Chave primária.
* `nome_modelo`: Nome do modelo (obrigatório).
* `custo_mao_obra`: Valor informado para corte e costura.
* `markup`: Multiplicador de margem (> 1).
* `custo_materia_prima` (derivado): Custo dos insumos calculado.
* `preco_tabela` (derivado): Preço de venda sugerido.

### BOLSA (Especialização)
* `tamanho`: Dimensão da bolsa.
* `cor`: Cor da bolsa.
* `tipo_alca`: Tipo de alça.
* `pecas_composicao`: Atributo multivalorado (Tampa, Frente, Costa, Fundo, Orla).

### ACESSORIO (Especialização)
* `tipo_peca`: Tipo do acessório (obrigatório).

### CLIENTE
* `id_cliente`: Chave primária.
* `nome`: Razão social ou nome.
* `cpf_cnpj`: CPF ou CNPJ único.
* `inscricao_estadual`: Obrigatória se Atacado/Lojista.
* `email`: E-mail para envio de NF-e.
* `telefone`: Contato principal.
* `nome_comprador_responsavel`: Contato de compras.
* `perfil_cliente`: Varejo Final ou Atacado/Lojista.
* `limite credito`: Teto de compra a prazo.
* `status_aprovacao_financeira`: Pendente, Aprovado ou Reprovado.

### PEDIDO
* `id_pedido`: Chave primária.
* `data_pedido`: Data do registro.
* `canal_venda`: Loja Física, E-commerce, WhatsApp ou Representante.
* `forma_pagamento`: PIX, Cartão ou Boleto.
* `condicao_parcelamento`: Prazos ou parcelas.
* `status_pedido`: Situação do pedido.
* `valor_total` (derivado): Total do pedido calculado.

### ITEM_PEDIDO (Associativa)
* `quantidade`: Unidades vendidas.
* `preco_unitario_praticado`: Preço congelado na venda.
* `desconto`: Desconto do item em R$.

---

## 6. Modelagem Conceitual e Relacionamentos
* **Entidades:** 9 entidades centrais, incorporando a especialização Total e Disjunta (TD) de `PRODUTO` em `BOLSA` e `ACESSORIO`.
* **Relacionamentos Principais:**
  * `fornece` (N:N entre `FORNECEDOR` e `INSUMO`)
  * `compõe` e `é detalhado em` (resolvem a lista de materiais em `FICHA_TECNICA`)
  * `é vendido em` e `contém` (ligam `PRODUTO`, `ITEM_PEDIDO` e `PEDIDO`)
  * `realiza` (liga `CLIENTE` a `PEDIDO`)

---

## 7. Diagrama Entidade-Relacionamento (DER)
* O DER correspondente a este recorte de 9 entidades (com notação de Chen, cardinalidades em min/max, atributos derivados em elipse tracejada e especialização TD) encontra-se na pasta **`diagrams/`** como `DER_Diagrama_9_Entidades.pdf`.

---

## 8. Justificativa Técnica
* O modelo de 9 entidades foca estritamente no núcleo de suprimentos, engenharia de produto, formação de preços e faturamento comercial, simplificando a complexidade operacional para atender perfeitamente aos requisitos centrais do escopo sem incluir entidades de suporte avançado.

---

## 9. Uso de Inteligência Artificial
* Documentação do uso de ferramentas de IA (Claude) para a estruturação e formatação do dicionário de dados e diagramas conceituais alinhados ao recorte de 9 entidades.
