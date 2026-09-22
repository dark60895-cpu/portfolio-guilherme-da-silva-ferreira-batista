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
* **Contexto e porte:** Empresa com fins lucrativos que atua na fabricação própria e comercialização de bolsas e acessórios de couro[cite: 6].
* **Evidências da organização:** 
  * **Endereço:** Rua Alpiste, 116 - Jd. Eliane - São Paulo - SP.
  * **Contato:** Osmar Lingiard | **Telefone:** (11) 97334-4846 | **E-mail:** osmar@specia.com.br.

---

## 2. Processos de Negócio
Mapeamento do fluxo central (da matéria-prima ao pedido faturado):
1. **Entrada de matéria-prima:** O fornecedor vende couro, zíperes, fivelas e forros, sendo que um mesmo insumo pode vir de vários fornecedores (N:N). O insumo armazena categoria, unidade de medida, estoque mínimo, estoque atual e custo unitário de referência[cite: 6].
2. **Engenharia de produto (Ficha Técnica):** Lista de materiais do produto com uma linha por insumo, determinando a quantidade necessária por peça e o percentual de perda técnica no corte[cite: 6].
3. **Formação de preço:** Cálculo derivado do custo de matéria-prima e mão de obra, aplicando o *markup* para sugerir o preço de tabela[cite: 6].
4. **Hierarquia de Produtos (Especialização TD):** Classificação dos produtos em Bolsas (tamanho, cor, tipo de alça e peças de composição) e Acessórios (tipo de peça)[cite: 6].
5. **Saída comercial:** Clientes realizam pedidos de venda multicanal compostos por itens com preços e descontos congelados no momento da transação[cite: 6].

---

## 3. Requisitos do Sistema
* **RF01:** Cadastrar clientes com dados fiscais, perfil (Varejo Final ou Atacado/Lojista), limite de crédito e status de aprovação financeira[cite: 6].
* **RF02:** Cadastrar fornecedores vinculados às categorias de insumos e prazos médios de entrega[cite: 6].
* **RF03:** Gerenciar insumos controlando estoque mínimo, estoque atual e custo unitário de referência[cite: 6].
* **RF04:** Manter a Ficha Técnica (BOM) associando produtos aos insumos necessários e ao percentual de perda técnica[cite: 6].
* **RF05:** Calcular automaticamente o custo de matéria-prima e o preço de tabela do produto[cite: 6].
* **RF06:** Registrar pedidos de venda multicanal associados a clientes, validando formas de pagamento, parcelamentos, limites de crédito e congelando o preço unitário praticado[cite: 6].

---

## 4. Regras de Negócio
* A inscrição estadual é obrigatória apenas quando o perfil do cliente é Atacado/Lojista[cite: 6].
* O custo da matéria-prima é calculado somando, para cada insumo da ficha, a quantidade multiplicada por $(1 + \text{perda})$ e pelo custo unitário[cite: 6].
* O preço de tabela é derivado da soma do custo de matéria-prima com o custo de mão de obra, multiplicado pelo *markup*[cite: 6].
* Para compras via boleto, o cliente precisa estar com o status Aprovado e o valor total do pedido deve respeitar o limite de crédito[cite: 6].
* O preço unitário praticado e os descontos no item do pedido ficam congelados no momento da venda, não sendo afetados por reajustes posteriores[cite: 6].

---

## 5. Dicionário de Dados Conceitual (9 Entidades)

### FORNECEDOR[cite: 6]
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| `id_fornecedor` | Identificador[cite: 6] | Chave primária, gerada pelo sistema. É única e nunca é reutilizada[cite: 6]. |
| `razao social` | Nome empresarial[cite: 6] | Obrigatório. É o nome que aparece nas compras e no histórico de avaliação[cite: 6]. |
| `cnpj` | CNPJ[cite: 6] | Obrigatório e único: não pode haver dois fornecedores com o mesmo CNPJ. Dado protegido pela LGPD[cite: 6]. |
| `inscricao estadual` | Registro estadual[cite: 6] | Opcional. Preenchida quando o fornecedor possui inscrição estadual[cite: 6]. |
| `email` | E-mail comercial[cite: 6] | Opcional. Canal de contato comercial com o fornecedor[cite: 6]. |
| `telefone` | Telefone[cite: 6] | Opcional. Contato direto com o fornecedor[cite: 6]. |
| `contato_vendedor` | Vendedor de referência[cite: 6] | Nome da pessoa que atende a empresa dentro do fornecedor[cite: 6]. |
| `categoria_insumo` | Tipo de insumo vendido[cite: 6] | Obrigatório. Aceita apenas: Curtume/Couro, Ferragens/Fivelas, Zíperes/Aviamentos ou Embalagens/Caixas[cite: 6]. |
| `prazo_medio_entrega_dias` | Prazo de entrega (dias)[cite: 6] | Número inteiro maior que zero. Impacta o planejamento da produção[cite: 6]. |

### INSUMO[cite: 6]
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| `id_insumo` | Identificador[cite: 6] | Chave primária, gerada pelo sistema[cite: 6]. |
| `nome_insumo` | Nome do material[cite: 6] | Obrigatório. Ex.: Couro Bovino Caramelo, zíper, fivela, forro[cite: 6]. |
| `categoria_insumo` | Grupo do material[cite: 6] | Usa o mesmo conjunto de categorias do fornecedor[cite: 6]. |
| `unidade_medida` | Unidade de controle[cite: 6] | Obrigatória. Aceita: $\text{dm}^2$, $\text{m}^2$, unidade, metro, kg ou litro[cite: 6]. |
| `estoque_minimo` | Saldo mínimo[cite: 6] | Quando o estoque atual fica igual ou abaixo do mínimo, o sistema gera alerta de recompra[cite: 6]. |
| `estoque_atual` | Saldo em estoque[cite: 6] | Nunca pode ser negativo. Aumenta a cada compra recebida e diminui quando consumido na produção[cite: 6]. |
| `custo_unitario` | Custo por unidade de medida[cite: 6] | Maior que zero. Guarda o último custo de compra como referência de cálculo[cite: 6]. |

### FICHA_TECNICA (Associativa)[cite: 6]
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| `quantidade_necessaria` | Quantidade por peça[cite: 6] | Maior que zero e na unidade de medida do insumo. Cada insumo aparece uma única vez na ficha[cite: 6]. |
| `percentual_perda` | Perda técnica no corte[cite: 6] | Informada em fração, de 0 a 1 ($10\% = 0,10$). Entra no cálculo do custo[cite: 6]. |

### PRODUTO[cite: 6]
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| `id_produto` | Identificador[cite: 6] | Chave primária, gerada pelo sistema. Mesma chave usada em BOLSA e ACESSORIO[cite: 6]. |
| `nome_modelo` | Nome do modelo[cite: 6] | Obrigatório. Ex.: Bolsa Tote[cite: 6]. |
| `custo_mao_obra` | Mão de obra por peça[cite: 6] | Valor informado para corte e costura, somado ao custo de matéria-prima[cite: 6]. |
| `markup` | Multiplicador de margem[cite: 6] | Valor informado, maior que 1 (ex.: 2,5)[cite: 6]. |
| `custo_materia_prima` (derivado) | Custo dos insumos[cite: 6] | Não digitado. Soma calculada a partir da ficha técnica[cite: 6]. |
| `preco_tabela` (derivado) | Preço de venda sugerido[cite: 6] | Não digitado. Calculado por $(\text{custo} + \text{mão de obra}) \times \text{markup}$[cite: 6]. |

### BOLSA (Especialização)[cite: 6]
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| `tamanho` | Dimensão da bolsa[cite: 6] | Obrigatório para bolsa. Herda atributos de PRODUTO[cite: 6]. |
| `cor` | Cor[cite: 6] | Obrigatória para bolsa[cite: 6]. |
| `tipo_alca` | Tipo de alça[cite: 6] | Obrigatório para bolsa[cite: 6]. |
| `pecas_composicao` (multivalorado) | Peças que formam a bolsa[cite: 6] | Mínimo de uma peça (Tampa, Frente, Costa, Fundo, Orla)[cite: 6]. |

### ACESSORIO (Especialização)[cite: 6]
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| `tipo_peca` | Tipo do acessório[cite: 6] | Obrigatório. Ex.: cinto, porta-cartões[cite: 6]. |

### CLIENTE[cite: 6]
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| `id_cliente` | Identificador[cite: 6] | Chave primária, gerada pelo sistema[cite: 6]. |
| `nome` | Razão social ou nome[cite: 6] | Obrigatório (razão social se PJ, nome completo se PF)[cite: 6]. |
| `cpf_cnpj` | CPF ou CNPJ[cite: 6] | Obrigatório e único. Usado na consulta de restrição de crédito (LGPD)[cite: 6]. |
| `inscricao_estadual` | Registro estadual[cite: 6] | Obrigatório apenas se perfil for Atacado/Lojista[cite: 6]. |
| `email` | E-mail[cite: 6] | Usado para enviar a NF-e ao cliente[cite: 6]. |
| `telefone` | Telefone/WhatsApp[cite: 6] | Contato principal do cliente[cite: 6]. |
| `nome_comprador_responsavel` | Contato de compras[cite: 6] | Pessoa que faz os pedidos no atacado[cite: 6]. |
| `perfil_cliente` | Tipo de cliente[cite: 6] | Varejo Final ou Atacado/Lojista[cite: 6]. |
| `limite credito` | Teto de compra a prazo[cite: 6] | Valor em R\$ (referência de R\$ 10.000,00 para boletos)[cite: 6]. |
| `status_aprovacao_financeira` | Situação do crédito[cite: 6] | Pendente, Aprovado ou Reprovado[cite: 6]. |

### PEDIDO[cite: 6]
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| `id_pedido` | Identificador[cite: 6] | Chave primária, gerada pelo sistema[cite: 6]. |
| `data_pedido` | Data do pedido[cite: 6] | Obrigatória. Preenchida no registro[cite: 6]. |
| `canal_venda` | Canal de venda[cite: 6] | Loja Física, E-commerce, WhatsApp ou Representante[cite: 6]. |
| `forma_pagamento` | Forma de pagamento[cite: 6] | PIX, Cartão ou Boleto[cite: 6]. |
| `condicao_parcelamento` | Parcelas ou prazo[cite: 6] | Prazos em dias para boleto ou parcelas para cartão[cite: 6]. |
| `status_pedido` | Situação do pedido[cite: 6] | Obrigatório. Muda conforme o andamento[cite: 6]. |
| `valor_total` (derivado) | Total do pedido[cite: 6] | Não digitado. Soma calculada dos itens do pedido[cite: 6]. |

### ITEM_PEDIDO (Associativa)[cite: 6]
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| `quantidade` | Unidades vendidas[cite: 6] | Número inteiro maior que zero[cite: 6]. |
| `preco_unitario_praticado` | Preço na venda[cite: 6] | Congelado no momento da venda (não muda com reajustes)[cite: 6]. |
| `desconto` | Desconto do item (R\$)[cite: 6] | Opcional. Limitado ao valor total do item[cite: 6]. |

---

## 6. Modelagem Conceitual e Relacionamentos
* **Entidades:** 9 entidades centrais, incorporando a especialização Total e Disjunta (TD) de `PRODUTO` em `BOLSA` e `ACESSORIO`[cite: 6].
* **Relacionamentos Principais:**
  * `fornece` (N:N entre `FORNECEDOR` e `INSUMO`)[cite: 6]
  * `compõe` e `é detalhado em` (resolvem a lista de materiais em `FICHA_TECNICA`)[cite: 6]
  * `é vendido em` e `contém` (ligam `PRODUTO`, `ITEM_PEDIDO` e `PEDIDO`)[cite: 6]
  * `realiza` (liga `CLIENTE` a `PEDIDO`)[cite: 6]

---

## 7. Diagrama Entidade-Relacionamento (DER)
* O DER correspondente a este recorte de 9 entidades (com notação de Chen, cardinalidades em min/max, atributos derivados em elipse tracejada e especialização TD) encontra-se na pasta **`diagrams/`** como `DER_Diagrama_9_Entidades.pdf`[cite: 6].

---

## 8. Justificativa Técnica
* O modelo de 9 entidades foca estritamente no núcleo de suprimentos, engenharia de produto, formação de preços e faturamento comercial, simplificando a complexidade operacional para atender perfeitamente aos requisitos centrais do escopo[cite: 6].

---

## 9. Uso de Inteligência Artificial
* Documentação do uso de ferramentas de IA (Claude) para a estruturação e formatação do dicionário de dados e diagramas conceituais alinhados ao recorte de 9 entidades[cite: 6].
