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
6. [Modelagem Conceitual e Relacionamentos](#6-modelagem-conceitual-e-relacionamentos)
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
Mapeamento do fluxo central (da matéria-prima ao pedido faturado)[cite: 5]:
1. **Entrada de matéria-prima:** Gestão de fornecedores e insumos (couro, zíperes, fivelas e forros) com controle de estoque e custo unitário de referência[cite: 5].
2. **Engenharia de produto (Ficha Técnica):** Lista de materiais por produto, determinando a quantidade necessária e o percentual de perda técnica no corte[cite: 5].
3. **Formação de preço:** Cálculo derivado do custo de matéria-prima e mão de obra, aplicando o *markup* para sugerir o preço de tabela[cite: 5].
4. **Hierarquia de Produtos (Especialização TD):** Classificação dos produtos em Bolsas (com atributos como tamanho, cor, tipo de alça e peças de composição) e Acessórios[cite: 5].
5. **Saída comercial:** Cadastro de clientes com controle de limite de crédito e aprovação financeira, além do registro de pedidos de venda com preços congelados no momento da transação[cite: 5].

---

## 3. Requisitos do Sistema
* **RF01:** Cadastrar clientes com dados fiscais, perfil (Varejo Final ou Atacado/Lojista), limite de crédito e status de aprovação financeira[cite: 5].
* **RF02:** Cadastrar fornecedores vinculados às categorias de insumos e prazos médios de entrega[cite: 5].
* **RF03:** Gerenciar insumos controlando estoque mínimo, estoque atual e custo unitário de referência[cite: 5].
* **RF04:** Manter a Ficha Técnica (BOM) associando produtos aos insumos necessários e ao percentual de perda técnica[cite: 5].
* **RF05:** Calcular automaticamente o custo de matéria-prima e o preço de tabela do produto[cite: 5].
* **RF06:** Registrar pedidos de venda multicanal associados a clientes, validando formas de pagamento, parcelamentos, limites de crédito e congelando o preço unitário praticado[cite: 5].

---

## 4. Regras de Negócio
* A inscrição estadual é obrigatória apenas quando o perfil do cliente é Atacado/Lojista[cite: 5].
* O custo da matéria-prima é calculado somando, para cada insumo da ficha, a quantidade multiplicada por $(1 + \text{perda})$ e pelo custo unitário[cite: 5].
* O preço de tabela é derivado da soma do custo de matéria-prima com o custo de mão de obra, multiplicado pelo *markup*[cite: 5].
* Para compras via boleto, o cliente precisa estar com o status Aprovado e o valor total do pedido deve respeitar o limite de crédito[cite: 5].
* O preço unitário praticado e os descontos no item do pedido ficam congelados no momento da venda, não sendo afetados por reajustes posteriores[cite: 5].

---

## 5. Dicionário de Dados Conceitual (9 Entidades)

### FORNECEDOR[cite: 5]
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| `id_fornecedor` | Identificador[cite: 5] | Chave primária, gerada pelo sistema. É única e nunca é reutilizada[cite: 5]. |
| `razao social` | Nome empresarial[cite: 5] | Obrigatório. É o nome que aparece nas compras e no histórico de avaliação[cite: 5]. |
| `cnpj` | CNPJ[cite: 5] | Obrigatório e único (protegido pela LGPD)[cite: 5]. |
| `inscricao estadual` | Registro estadual[cite: 5] | Opcional. Preenchida quando o fornecedor possui inscrição[cite: 5]. |
| `email` | E-mail comercial[cite: 5] | Opcional. Canal de contato comercial[cite: 5]. |
| `telefone` | Telefone[cite: 5] | Opcional. Contato direto[cite: 5]. |
| `contato_vendedor` | Vendedor de referência[cite: 5] | Nome da pessoa que atende a empresa[cite: 5]. |
| `categoria_insumo` | Tipo de insumo vendido[cite: 5] | Obrigatório. Aceita apenas: Curtume/Couro, Ferragens/Fivelas, Zíperes/Aviamentos ou Embalagens/Caixas[cite: 5]. |
| `prazo_medio_entrega_dias` | Prazo de entrega (dias)[cite: 5] | Número inteiro maior que zero. Impacta o planejamento da produção[cite: 5]. |

### INSUMO[cite: 5]
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| `id_insumo` | Identificador[cite: 5] | Chave primária, gerada pelo sistema[cite: 5]. |
| `nome_insumo` | Nome do material[cite: 5] | Obrigatório. Ex.: Couro Bovino Caramelo, zíper, fivela[cite: 5]. |
| `categoria_insumo` | Grupo do material[cite: 5] | Usa o mesmo conjunto de categorias do fornecedor[cite: 5]. |
| `unidade_medida` | Unidade de controle[cite: 5] | Obrigatória. Aceita: $\text{dm}^2$, $\text{m}^2$, unidade, metro, kg ou litro[cite: 5]. |
| `estoque_minimo` | Saldo mínimo[cite: 5] | Quando o estoque atual fica abaixo do mínimo, gera alerta de recompra[cite: 5]. |
| `estoque_atual` | Saldo em estoque[cite: 5] | Nunca pode ser negativo. Aumenta com compras e diminui na produção[cite: 5]. |
| `custo_unitario` | Custo por unidade de medida[cite: 5] | Maior que zero. Guarda o último custo de compra como referência[cite: 5]. |

### FICHA_TECNICA (Associativa)[cite: 5]
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| `quantidade_necessaria` | Quantidade por peça[cite: 5] | Maior que zero e na unidade de medida do insumo[cite: 5]. |
| `percentual_perda` | Perda técnica no corte[cite: 5] | Informada em fração, de 0 a 1 ($10\% = 0,10$)[cite: 5]. |

### PRODUTO[cite: 5]
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| `id_produto` | Identificador[cite: 5] | Chave primária, gerada pelo sistema[cite: 5]. |
| `nome_modelo` | Nome do modelo[cite: 5] | Obrigatório. Ex.: Bolsa Tote[cite: 5]. |
| `custo_mao_obra` | Mão de obra por peça[cite: 5] | Valor informado para corte e costura[cite: 5]. |
| `markup` | Multiplicador de margem[cite: 5] | Valor informado, maior que 1[cite: 5]. |
| `custo_materia_prima` (derivado) | Custo dos insumos[cite: 5] | Não digitado. Soma calculada a partir da ficha técnica[cite: 5]. |
| `preco_tabela` (derivado) | Preço de venda sugerido[cite: 5] | Não digitado. Calculado por $(\text{custo} + \text{mão de obra}) \times \text{markup}$[cite: 5]. |

### BOLSA (Especialização)[cite: 5]
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| `tamanho` | Dimensão da bolsa[cite: 5] | Obrigatório para bolsa. Herda atributos de PRODUTO[cite: 5]. |
| `cor` | Cor[cite: 5] | Obrigatória para bolsa[cite: 5]. |
| `tipo_alca` | Tipo de alça[cite: 5] | Obrigatório para bolsa[cite: 5]. |
| `pecas_composicao` (multivalorado) | Peças que formam a bolsa[cite: 5] | Mínimo de uma peça (Tampa, Frente, Costa, Fundo, Orla)[cite: 5]. |

### ACESSORIO (Especialização)[cite: 5]
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| `tipo_peca` | Tipo do acessório[cite: 5] | Obrigatório. Ex.: cinto, porta-cartões[cite: 5]. |

### CLIENTE[cite: 5]
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| `id_cliente` | Identificador[cite: 5] | Chave primária, gerada pelo sistema[cite: 5]. |
| `nome` | Razão social ou nome[cite: 5] | Obrigatório (razão social se PJ, nome completo se PF)[cite: 5]. |
| `cpf_cnpj` | CPF ou CNPJ[cite: 5] | Obrigatório e único. Protegido pela LGPD[cite: 5]. |
| `inscricao_estadual` | Registro estadual[cite: 5] | Obrigatório apenas se perfil for Atacado/Lojista[cite: 5]. |
| `email` | E-mail[cite: 5] | Usado para envio da NF-e ao cliente[cite: 5]. |
| `telefone` | Telefone/WhatsApp[cite: 5] | Contato principal do cliente[cite: 5]. |
| `nome_comprador_responsavel` | Contato de compras[cite: 5] | Pessoa que faz os pedidos no atacado[cite: 5]. |
| `perfil_cliente` | Tipo de cliente[cite: 5] | Varejo Final ou Atacado/Lojista[cite: 5]. |
| `limite credito` | Teto de compra a prazo[cite: 5] | Valor em R\$ (ex.: R\$ 10.000,00 para boletos)[cite: 5]. |
| `status_aprovacao_financeira` | Situação do crédito[cite: 5] | Pendente, Aprovado ou Reprovado[cite: 5]. |

### PEDIDO[cite: 5]
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| `id_pedido` | Identificador[cite: 5] | Chave primária, gerada pelo sistema[cite: 5]. |
| `data_pedido` | Data do pedido[cite: 5] | Obrigatória. Preenchida no registro[cite: 5]. |
| `canal_venda` | Canal de venda[cite: 5] | Loja Física, E-commerce, WhatsApp ou Representante[cite: 5]. |
| `forma_pagamento` | Forma de pagamento[cite: 5] | PIX, Cartão ou Boleto[cite: 5]. |
| `condicao_parcelamento` | Parcelas ou prazo[cite: 5] | Prazos em dias para boleto ou parcelas para cartão[cite: 5]. |
| `status_pedido` | Situação do pedido[cite: 5] | Obrigatório. Muda conforme o andamento[cite: 5]. |
| `valor_total` (derivado) | Total do pedido[cite: 5] | Não digitado. Soma calculada dos itens do pedido[cite: 5]. |

### ITEM_PEDIDO (Associativa)[cite: 5]
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| `quantidade` | Unidades vendidas[cite: 5] | Número inteiro maior que zero[cite: 5]. |
| `preco_unitario_praticado` | Preço na venda[cite: 5] | Congelado no momento da venda (não muda com reajustes)[cite: 5]. |
| `desconto` | Desconto do item (R\$)[cite: 5] | Opcional. Limitado ao valor total do item[cite: 5]. |

---

## 6. Modelagem Conceitual e Relacionamentos
* **Entidades:** 9 entidades centrais, incorporando a especialização Total e Disjunta (TD) de `PRODUTO` em `BOLSA` e `ACESSORIO`[cite: 5].
* **Relacionamentos Principais:**
  * `fornece` (N:N entre `FORNECEDOR` e `INSUMO`)[cite: 5]
  * `compõe` e `é detalhado em` (resolvem a lista de materiais em `FICHA_TECNICA`)[cite: 5]
  * `é vendido em` e `contém` (ligam `PRODUTO`, `ITEM_PEDIDO` e `PEDIDO`)[cite: 5]
  * `realiza` (liga `CLIENTE` a `PEDIDO`)[cite: 5]

---

## 7. Diagrama Entidade-Relacionamento (DER)
* O DER correspondente a este recorte de 9 entidades (com notação de Chen, cardinalidades em min/max, atributos derivados em elipse tracejada e especialização TD) encontra-se na pasta **`diagrams/`** como `DER_Diagrama_9_Entidades.pdf`[cite: 5].

---

## 8. Justificativa Técnica
* O modelo de 9 entidades foca estritamente no núcleo de suprimentos, engenharia de produto, formação de preços e faturamento comercial, simplificando a complexidade operacional para atender perfeitamente aos requisitos centrais do escopo[cite: 5].

---

## 9. Uso de Inteligência Artificial
* Documentação do uso de ferramentas de IA (Claude) para a estruturação e formatação do dicionário de dados e diagramas conceituais alinhados ao recorte de 9 entidades[cite: 5].
