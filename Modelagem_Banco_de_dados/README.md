# Modelação de Base de Dados — Contrasti Bolsas e Acessórios (Modelo de 9 Entidades)

## Metadados do Grupo

| Nome | RGM |
| --- | --- |
| Guilherme da Silva Ferreira Batista | 47302518 |
| Guilherme Petrucelli Domingos | 47270161 |
| Jaime Luiz de Oliveira Neto | 47336951 |
| Matheus Montagner | 47209470 |
| Vinicius Marques de Melo | 47213426 |

---

## Sumário

1. [Caracterização da Organização](https://www.google.com/search?q=%25231-caracteriza%25C3%25A7%25C3%25A3o-da-organiza%25C3%25A7%25C3%25A3o&utm_source=gemini)
2. [Processos de Negócio](https://www.google.com/search?q=%25232-processos-de-neg%25C3%25B3cio&utm_source=gemini)
3. [Requisitos do Sistema](https://www.google.com/search?q=%25233-requisitos-do-sistema&utm_source=gemini)
4. [Regras de Negócio](https://www.google.com/search?q=%25234-regras-de-neg%25C3%25B3cio&utm_source=gemini)
5. [Dicionário de Dados Conceitual (9 Entidades)](https://www.google.com/search?q=%25235-dicion%25C3%25A1rio-de-dados-conceitual-9-entidades&utm_source=gemini)
6. [Modelação Conceitual e Relacionamentos](https://www.google.com/search?q=%25236-modela%25C3%25A7%25C3%25A3o-conceitual-e-relacionamentos&utm_source=gemini)
7. [Diagrama Entidade-Relacionamento (DER)](https://www.google.com/search?q=%25237-diagrama-entidade-relacionamento-der&utm_source=gemini)
8. [Justificativa Técnica](https://www.google.com/search?q=%25238-justificativa-t%25C3%25A9cnica&utm_source=gemini)
9. [Uso de Inteligência Artificial](https://www.google.com/search?q=%25239-uso-de-intelig%25C3%25AAncia-artificial&utm_source=gemini)

---

## 1. Caracterização da Organização

* **Nome e natureza da organização:** Contrasti Bolsas e Acessórios Ltda - Fabricação de Bolsas (Indústria e Comércio de Bolsas de Couro).
* **Contexto e porte:** Empresa com fins lucrativos que atua na fabricação própria e comercialização de bolsas e acessórios de couro.
* **Evidências da organização:**
* **Endereço:** Rua Alpiste, 116 - Jd. Eliane - São Paulo - SP.
* **Contacto:** Osmar Lingiard | **Telefone:** (11) 97334-4846 | **E-mail:** osmar@specia.com.br.



---

## 2. Processos de Negócio

Mapeamento do fluxo central (da matéria-prima ao pedido faturado):

1. **Entrada de matéria-prima:** O fornecedor vende couro, zíperes, fivelas e forros, sendo que um mesmo insumo pode vir de vários fornecedores (N:N). O insumo armazena categoria, unidade de medida, estoque mínimo, estoque atual e custo unitário de referência.
2. **Engenharia de produto (Ficha Técnica):** Lista de materiais do produto com uma linha por insumo, determinando a quantidade necessária por peça e o percentual de perda técnica no corte.
3. **Formação de preço:** Cálculo derivado do custo de matéria-prima e mão de obra, aplicando o *markup* para sugerir o preço de tabela.
4. **Hierarquia de Produtos (Especialização TD):** Classificação dos produtos em Bolsas (tamanho, cor, tipo de alça e peças de composição) e Acessórios (tipo de peça).
5. **Saída comercial:** Clientes realizam pedidos de venda multicanal compostos por itens com preços e descontos congelados no momento da transação.

---

## 3. Requisitos do Sistema

* **RF01:** Cadastrar clientes com dados fiscais, perfil (Varejo Final ou Atacado/Lojista), limite de crédito e estatuto de aprovação financeira.
* **RF02:** Cadastrar fornecedores vinculados às categorias de insumos e prazos médios de entrega.
* **RF03:** Gerir insumos controlando estoque mínimo, estoque atual e custo unitário de referência.
* **RF04:** Manter a Ficha Técnica (BOM) associando produtos aos insumos necessários e ao percentual de perda técnica.
* **RF05:** Calcular automaticamente o custo de matéria-prima e o preço de tabela do produto.
* **RF06:** Registar pedidos de venda multicanal associados a clientes, validando formas de pagamento, parcelamentos, limites de crédito e congelando o preço unitário praticado.

---

## 4. Regras de Negócio

* A inscrição estadual é obrigatória apenas quando o perfil do cliente é Atacado/Lojista.
* O custo da matéria-prima é calculado somando, para cada insumo da ficha, a quantidade multiplicada por $(1 + \text{perda})$ e pelo custo unitário.
* O preço de tabela é derivado da soma do custo de matéria-prima com o custo de mão de obra, multiplicado pelo *markup*.
* Para compras via boleto, o cliente precisa estar com o estatuto Aprovado e o valor total do pedido deve respeitar o limite de crédito.
* O preço unitário praticado e os descontos no item do pedido ficam congelados no momento da venda, não sendo afetados por reajustes posteriores.

---

## 5. Dicionário de Dados Conceitual (9 Entidades)

### FORNECEDOR

| Atributo | Descrição | Regra de negócio associada |
| --- | --- | --- |
| `id_fornecedor` | Identificador | Chave primária, gerada pelo sistema. É única e nunca é reutilizada. |
| `razao social` | Nome empresarial | Obrigatório. É o nome que aparece nas compras e no histórico de avaliação. |
| `cnpj` | CNPJ | Obrigatório e único: não pode haver dois fornecedores com o mesmo CNPJ. Dado protegido pela LGPD. |
| `inscricao estadual` | Registo estadual | Opcional. Preenchida quando o fornecedor possui inscrição estadual. |
| `email` | E-mail comercial | Opcional. Canal de contacto comercial com o fornecedor. |
| `telefone` | Telefone | Opcional. Contacto direto com o fornecedor. |
| `contato_vendedor` | Vendedor de referência | Nome da pessoa que atende a empresa dentro do fornecedor. |
| `categoria_insumo` | Tipo de insumo vendido | Obrigatório. Aceita apenas: Curtume/Couro, Ferragens/Fivelas, Zíperes/Aviamentos ou Embalagens/Caixas. |
| `prazo_medio_entrega_dias` | Prazo de entrega (dias) | Número inteiro maior que zero. Impacta o planeamento da produção. |

### INSUMO

| Atributo | Descrição | Regra de negócio associada |
| --- | --- | --- |
| `id_insumo` | Identificador | Chave primária, gerada pelo sistema. |
| `nome_insumo` | Nome do material | Obrigatório. Ex.: Couro Bovino Caramelo, zíper, fivela, forro. |
| `categoria_insumo` | Grupo do material | Usa o mesmo conjunto de categorias do fornecedor. |
| `unidade_medida` | Unidade de controlo | Obrigatória. Aceita: $\text{dm}^2$, $\text{m}^2$, unidade, metro, kg ou litro. |
| `estoque_minimo` | Saldo mínimo | Quando o estoque atual fica igual ou abaixo do mínimo, o sistema gera alerta de recompra. |
| `estoque_atual` | Saldo em estoque | Nunca pode ser negativo. Aumenta a cada compra recebida e diminui quando consumido na produção. |
| `custo_unitario` | Custo por unidade de medida | Maior que zero. Guarda o último custo de compra como referência de cálculo. |

### FICHA_TECNICA (Associativa)

| Atributo | Descrição | Regra de negócio associada |
| --- | --- | --- |
| `quantidade_necessaria` | Quantidade por peça | Maior que zero e na unidade de medida do insumo. Cada insumo aparece uma única vez na ficha. |
| `percentual_perda` | Perda técnica no corte | Informada em fração, de 0 a 1 ($10\% = 0,10$). Entra no cálculo do custo. |

### PRODUTO

| Atributo | Descrição | Regra de negócio associada |
| --- | --- | --- |
| `id_produto` | Identificador | Chave primária, gerada pelo sistema. Mesma chave usada em BOLSA e ACESSORIO. |
| `nome_modelo` | Nome do modelo | Obrigatório. Ex.: Bolsa Tote. |
| `custo_mao_obra` | Mão de obra por peça | Valor informado para corte e costura, somado ao custo de matéria-prima. |
| `markup` | Multiplicador de margem | Valor informado, maior que 1 (ex.: 2,5). |
| `custo_materia_prima` (derivado) | Custo dos insumos | Não digitado. Soma calculada a partir da ficha técnica. |
| `preco_tabela` (derivado) | Preço de venda sugerido | Não digitado. Calculado por $(\text{custo} + \text{mão de obra}) \times \text{markup}$. |

### BOLSA (Especialização)

| Atributo | Descrição | Regra de negócio associada |
| --- | --- | --- |
| `tamanho` | Dimensão da bolsa | Obrigatório para bolsa. Herda atributos de PRODUTO. |
| `cor` | Cor | Obrigatória para bolsa. |
| `tipo_alca` | Tipo de alça | Obrigatório para bolsa. |
| `pecas_composicao` (multivalorado) | Peças que formam a bolsa | Mínimo de uma peça (Tampa, Frente, Costa, Fundo, Orla). |

### ACESSORIO (Especialização)

| Atributo | Descrição | Regra de negócio associada |
| --- | --- | --- |
| `tipo_peca` | Tipo do acessório | Obrigatório. Ex.: cinto, porta-cartões. |

### CLIENTE

| Atributo | Descrição | Regra de negócio associada |
| --- | --- | --- |
| `id_cliente` | Identificador | Chave primária, gerada pelo sistema. |
| `nome` | Razão social ou nome | Obrigatório (razão social se PJ, nome completo se PF). |
| `cpf_cnpj` | CPF ou CNPJ | Obrigatório e único. Usado na consulta de restrição de crédito (LGPD). |
| `inscricao_estadual` | Registo estadual | Obrigatório apenas se perfil for Atacado/Lojista. |
| `email` | E-mail | Usado para enviar a NF-e ao cliente. |
| `telefone` | Telefone/WhatsApp | Contacto principal do cliente. |
| `nome_comprador_responsavel` | Contacto de compras | Pessoa que faz os pedidos no atacado. |
| `perfil_cliente` | Tipo de cliente | Varejo Final ou Atacado/Lojista. |
| `limite credito` | Teto de compra a prazo | Valor em R$ (referência de R$ 10.000,00 para boletos). |
| `status_aprovacao_financeira` | Estatuto do crédito | Pendente, Aprovado ou Reprovado. |

### PEDIDO

| Atributo | Descrição | Regra de negócio associada |
| --- | --- | --- |
| `id_pedido` | Identificador | Chave primária, gerada pelo sistema. |
| `data_pedido` | Data do pedido | Obrigatória. Preenchida no registo. |
| `canal_venda` | Canal de venda | Loja Física, E-commerce, WhatsApp ou Representante. |
| `forma_pagamento` | Forma de pagamento | PIX, Cartão ou Boleto. |
| `condicao_parcelamento` | Parcelas ou prazo | Prazos em dias para boleto ou parcelas para cartão. |
| `status_pedido` | Estatuto do pedido | Obrigatório. Muda conforme o andamento. |
| `valor_total` (derivado) | Total do pedido | Não digitado. Soma calculada dos itens do pedido. |

### ITEM_PEDIDO (Associativa)

| Atributo | Descrição | Regra de negócio associada |
| --- | --- | --- |
| `quantidade` | Unidades vendidas | Número inteiro maior que zero. |
| `preco_unitario_praticado` | Preço na venda | Congelado no momento da venda (não muda com reajustes). |
| `desconto` | Desconto do item (R$) | Opcional. Limitado ao valor total do item. |

---

## 6. Modelação Conceitual e Relacionamentos

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

* O modelo de 9 entidades foca estritamente no núcleo de suprimentos, engenharia de produto, formação de preços e faturamento comercial, simplificando a complexidade operacional para atender perfeitamente aos requisitos centrais do escopo.

---

## 9. Uso de Inteligência Artificial

* Documentação do uso de ferramentas de IA (Claude) para a estruturação e formatação do dicionário de dados e diagramas conceituais alinhados ao recorte de 9 entidades.
