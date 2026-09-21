## ⚠️ Checklist — o que precisa ser alterado ou apagado antes de entregar

Antes de colar estas seções no README final e subir no repositório, revisem os pontos abaixo. Nenhum deles está resolvido automaticamente por este arquivo — são decisões que o grupo precisa tomar.

1. **Apagar a menção a "21 entidades" no restante do README.** O rascunho original (Caracterização/Introdução, se houver) ainda cita "o projeto contempla um total de 21 entidades". Com o novo DER de 9 entidades, essa frase fica contraditória e é o tipo de inconsistência que salta aos olhos do avaliador. Buscar e remover/atualizar em todo o documento, não só nas seções 5 e 6.
2. **Substituir por completo as antigas Seções 5 e 6** (dicionário de 21 entidades e tabela de 22 relacionamentos) pelas versões novas deste arquivo — não deixar as duas versões coexistindo no mesmo README.
3. **Atualizar a Seção 4 (Regras de Negócio)** para não listar regras de processos que saíram do escopo do DER atual (ex.: aprovação de crédito/limite de crédito, rastreabilidade de lote/tonalidade, RMA vinculado a item, baixa automática de estoque via Ordem de Produção). Ou apagar essas regras, ou marcá-las explicitamente como "regras observadas na organização, mapeadas para escopo futuro" — do jeito que está hoje, elas prometem algo que o modelo atual não cobre.
4. **Atualizar a Seção 9 (Uso de IA).** Ela ainda descreve apenas o modelo de 21 entidades e a reconstrução para notação de Chen. Precisa registrar também esta rodada: a redução para 9 entidades, os prompts usados para simplificar o diagrama, e o que foi mantido/descartado nessa simplificação — a seção é de documentação obrigatória e tem que refletir o processo real, não só a primeira versão.
5. **Decidir sobre `id_bolsa` em ACESSORIO.** Ficou registrado como ponto em aberto na Justificativa Técnica (Seção 8). Se o grupo optar por corrigir, renomear para `id_produto` no diagrama e neste dicionário antes de gerar a versão final do PNG/SVG no BrModeloWeb.
6. **Resolver os dois losangos redundantes** ("contém" + "pertence a") entre PRODUTO/ITEM_PEDIDO/PEDIDO no diagrama, se decidirem simplificar para um único relacionamento — nesse caso a tabela de relacionamentos da Seção 6 deste arquivo também precisa ser ajustada (ela já assume a estrutura atual do diagrama, com dois relacionamentos separados nas linhas 06 e 07).
7. **Apagar os campos "PENDENTE" da Caracterização da Organização** (fotos da visita, link do Google Maps/Meu Negócio) antes da entrega final — o enunciado exige evidência real de que a organização existe e foi visitada; sem isso, a seção fica incompleta mesmo com o resto do trabalho pronto.
8. **Conferir se o DER anexado (PNG/SVG do BrModeloWeb) é exatamente a versão de 9 entidades** descrita aqui, e não uma versão intermediária — é comum sobrar no repositório uma versão antiga do arquivo de imagem com nome parecido.

---

## 5. Dicionário de Dados Conceitual

> **Nota de escopo:** esta versão do modelo foi reduzida de 21 para **9 entidades**, cobrindo o núcleo essencial do fluxo de custo e venda (fornecimento de insumo → ficha técnica → produto → pedido → cliente). Controle de lote/rastreabilidade de couro, chão de fábrica (Kanban/etapas), financeiro (contas a pagar/receber, nota fiscal) e pós-venda (RMA) foram deliberadamente deixados fora deste modelo conceitual — ver justificativa na Seção 8.

### FORNECEDOR
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| id_fornecedor | Identificador do fornecedor | Chave primária |
| razao_social | Nome empresarial do fornecedor | — |
| cnpj | Documento fiscal do fornecedor | — |
| telefone | Contato principal | — |

### INSUMO
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| id_insumo | Identificador do insumo | Chave primária |
| nome_insumo | Nome do insumo (ex.: Couro Bovino Caramelo) | — |
| unidade_medida | Unidade de controle de estoque | Domínio: m², metros, unidade |
| estoque_atual | Saldo atual em estoque | Atualizado a cada entrada/baixa |
| custo_unitario | Custo de aquisição por unidade de medida | Base de cálculo do `custo_materia_prima` em PRODUTO |

### FICHA_TECNICA *(entidade associativa)*
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| id_bolsa | Produto ao qual a ficha pertence | Chave primária composta + FK → PRODUTO |
| id_insumo | Insumo utilizado na peça | Chave primária composta + FK → INSUMO |
| quantidade_necessaria | Quantidade teórica do insumo necessária para fabricar a peça | Base de cálculo do custo de matéria-prima |
| percentual_perda | Percentual de perda técnica do insumo no processo de corte | Ex.: 10% de perda de couro — soma-se ao custo real |

### PRODUTO *(superclasse)*
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| id_bolsa | Identificador do produto | Chave primária |
| nome_modelo | Nome do modelo da peça | — |
| preco_tabela | Preço de venda calculado | Preço Tabela = (Custo MP + Custo MO) × Markup |
| custo_materia_prima | Custo total de insumos, já considerando perdas | Somatório via FICHA_TECNICA |
| custo_mao_obra | Custo de tempo de corte e costura | — |
| markup | Multiplicador aplicado sobre o custo total | Define a margem de lucro do produto |

### BOLSA *(subclasse — especialização Total e Disjunta)*
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| id_bolsa | Identificador do produto | Chave primária e estrangeira → PRODUTO |
| tamanho | Dimensão da bolsa | Domínio: P, M, G |
| cor | Cor predominante da peça | — |
| tipo_alca | Modelo da alça | Ex.: Transversal, Ombro, Mão |
| pecas_composicao | Peças que compõem a bolsa | Ex.: Tampa, Frente, Costa, Fundo, Orla |

### ACESSORIO *(subclasse — especialização Total e Disjunta)*
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| id_bolsa | Identificador do produto | Chave primária e estrangeira → PRODUTO |
| tipo_peca | Classificação do acessório | Ex.: Cinto, Porta-cartões |

### ITEM_PEDIDO *(entidade associativa)*
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| id_pedido | Pedido ao qual o item pertence | Chave primária composta + FK → PEDIDO |
| id_bolsa | Produto vendido | Chave primária composta + FK → PRODUTO |
| quantidade | Quantidade vendida daquele produto no pedido | — |

### PEDIDO
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| id_pedido | Identificador do pedido | Chave primária |
| data_pedido | Data em que o pedido foi realizado | — |
| valor_total | Valor total da transação | Somatório dos itens do pedido (via ITEM_PEDIDO) |

### CLIENTE
| Atributo | Descrição | Regra de negócio associada |
|---|---|---|
| id_cliente | Identificador do cliente | Chave primária |
| nome | Nome do cliente (PF ou PJ) | — |
| cpf_cnpj | Documento fiscal do cliente | — |
| telefone | Contato principal | — |
| email | E-mail de contato | — |

---

## 6. Modelagem Conceitual (Entidades, Atributos, Relacionamentos)

- **Entidades reconhecidas (9 no total):** FORNECEDOR, INSUMO, FICHA_TECNICA, PRODUTO, BOLSA, ACESSORIO, ITEM_PEDIDO, PEDIDO e CLIENTE, cobrindo o fluxo ponta a ponta: entrada de matéria-prima → engenharia de custo do produto → venda ao cliente. Duas são **entidades associativas** que reificam relacionamentos N:N com atributos próprios: FICHA_TECNICA (quantidade necessária e perda por insumo) e ITEM_PEDIDO (quantidade vendida por produto).
- **Atributos e classificações:** detalhados por entidade na Seção 5, com chave primária, chaves estrangeiras e domínios de valor explícitos (ex.: `unidade_medida`, `tipo_alca`, `tipo_peca`).
- **Relacionamentos pertinentes (8 no total):**

| # | Entidade A | Card. A | Verbo | Card. B | Entidade B |
|---|---|---|---|---|---|
| 01 | FORNECEDOR | (1,1) | fornece | (0,N) | INSUMO |
| 02 | INSUMO | (1,1) | compõe | (0,N) | FICHA_TECNICA |
| 03 | PRODUTO | (1,1) | é detalhado em | (0,N) | FICHA_TECNICA |
| 04 | PRODUTO | (1,1) | especializa-se em (TD) | (0,1) | BOLSA |
| 05 | PRODUTO | (1,1) | especializa-se em (TD) | (0,1) | ACESSORIO |
| 06 | PRODUTO | (1,1) | é vendido em | (0,N) | ITEM_PEDIDO |
| 07 | PEDIDO | (1,1) | contém | (1,N) | ITEM_PEDIDO |
| 08 | CLIENTE | (1,1) | realiza | (0,N) | PEDIDO |

- **Restrições e políticas organizacionais aplicadas ao modelo:** a especialização de PRODUTO em BOLSA/ACESSORIO é **Total e Disjunta (T,D)** — todo produto do catálogo pertence a exatamente uma das duas subclasses, nunca a nenhuma ou às duas ao mesmo tempo, refletindo a forma como a fábrica organiza seu catálogo. O `percentual_perda` em FICHA_TECNICA existe porque a organização trata a perda de couro no corte como custo real do produto, não como refugo desconsiderado — isso é o que sustenta o cálculo de `custo_materia_prima` em PRODUTO. `PEDIDO–ITEM_PEDIDO` é (1,N) do lado do item porque, operacionalmente, um pedido sem nenhum item não existe.

---

## 8. Justificativa Técnica

- **Por que reduzir de 21 para 9 entidades:** a primeira versão do modelo (Entrega 1, rascunho inicial) cobria as 8 seções do levantamento de requisitos na íntegra, incluindo chão de fábrica, financeiro e pós-venda. Para esta entrega, o grupo optou por um **núcleo conceitual enxuto**, focado no eixo que mais evidencia a lógica de negócio da organização — a formação de custo e preço de venda (fornecedor → insumo → ficha técnica → produto → pedido → cliente) — priorizando clareza do diagrama sobre cobertura total. Controle de lote/tonalidade de couro (rastreabilidade), acompanhamento de Ordem de Produção por etapa (Kanban, artesão responsável), emissão fiscal, contas a pagar/receber e RMA são processos reais e documentados no levantamento de requisitos, mas ficam mapeados como **extensões previstas para as próximas entregas**, e não foram descartados por não existirem na organização.
- **Por que FICHA_TECNICA e ITEM_PEDIDO como entidades associativas:** ambas carregam atributos que não pertencem a nenhuma das duas entidades que conectam — `quantidade_necessaria` e `percentual_perda` não são propriedade nem de PRODUTO nem de INSUMO isoladamente, mas sim da combinação dos dois; o mesmo vale para `quantidade` em ITEM_PEDIDO, que só existe no contexto de um produto específico dentro de um pedido específico. A notação de Chen exige reificar esse tipo de relacionamento N:N como entidade para acomodar tais atributos.
- **Por que a Especialização Total e Disjunta em PRODUTO:** a fábrica trabalha com dois tipos de artigo com atributos estruturalmente diferentes (BOLSA tem tamanho/alça/composição de peças; ACESSORIO tem apenas tipo de peça). Modelar como uma tabela única geraria muitos atributos nulos (ex.: `tipo_alca` sempre vazio para acessórios). A especialização é **Total** porque todo produto cadastrado precisa pertencer a uma das duas categorias (não existe produto "genérico" sem tipo definido), e **Disjunta** porque uma peça não pode ser bolsa e acessório ao mesmo tempo.
- **Por que essas cardinalidades e não outras:** `FORNECEDOR–INSUMO` é (1,1)-(0,N) porque um insumo tem origem em um único fornecedor cadastrado, mas um fornecedor normalmente supre vários insumos. `PEDIDO–ITEM_PEDIDO` é (1,N) do lado do item porque um pedido sem item não existe operacionalmente, enquanto `CLIENTE–PEDIDO` é (0,N) porque um cliente pode estar cadastrado sem nunca ter feito um pedido.
- **Alternativas descartadas:** cogitou-se manter `id_bolsa` como identificador único de PRODUTO mesmo para instâncias de ACESSORIO (como está representado hoje, por herança direta da PK da superclasse) — o grupo optou por manter essa nomenclatura por ora para não alterar o diagrama já validado, mas registra como ponto de ajuste futuro renomear para `id_produto`, evitando a leitura equivocada de que um acessório seria "um tipo de bolsa". Cogitou-se também representar `custo_materia_prima` apenas como campo calculado em tempo de consulta (sem persistir em PRODUTO) — descartado nesta etapa conceitual porque o documento técnico do DER trata esse valor como métrica de primeira classe do produto, a ser detalhada no dicionário de dados; a decisão de persistir ou calcular dinamicamente fica para o projeto físico (Entrega 2).
