# Modelagem de Banco de Dados — Contrasti Bolsas e Acessórios

## Metadados do Grupo

| Nome | RGM |
|---|---|
| Guilherme da Silva Ferreira Batista | 47302518 |
| Guilherme Petrucelli Domingos | 47270161 |
| Jaime Luiz de Oliveira Neto | 47336951 |
| Matheus Montagner | 47209470 |
| Vinicius Marques de Melo | 47213426 |



## 1. Caracterização da Organização

* **Nome e natureza:** Contrasti Bolsas e Acessórios Ltda (Fabricação de Bolsas). Indústria e comércio com fins lucrativos voltada para a fabricação própria e comercialização de bolsas e acessórios de couro.
* **Contexto e porte:** Operação de pequeno a médio porte que atua simultaneamente como indústria e comércio multicanal (loja física, e-commerce, WhatsApp e representantes externos). Utiliza facções terceirizadas e controle de aproveitamento de couro por corte. Volume médio de 50 a 70 pedidos mensais, com picos em datas comemorativas (Dia das Mães e Natal).
* **Problemas e necessidades:** Processos manuais ou descentralizados, ausência de rastreabilidade de lotes de insumos, falta de regras formais de crédito, cálculo de preços não padronizado e baixa integração entre vendas, estoque e financeiro.
* **Justificativa da escolha:** Organização de porte moderado com acesso facilitado para levantamento prático dos processos reais.
* **Evidências da organização:** 
  * **Endereço:** Rua Alpiste, 116 - Jd. Eliane - São Paulo - SP.
  * **Contato:** Osmar Lingiard | **Telefone:** (11) 97334-4846 | **E-mail:** osmar@specia.com.br.
  * *Status de mídias/fotos:* PENDENTE de anexo físico no repositório.

---

## 2. Processos de Negócio

Principais processos mapeados:
1. **Cadastro e gestão de clientes:** Múltiplos endereços, perfis (varejo/atacado) e controle de crédito.
2. **Cadastro e gestão de fornecedores:** Dados cadastrais, categorias de insumos e histórico de avaliações.
3. **Ficha técnica e produtos:** Gestão de modelos, SKUs e listas de materiais (BOM).
4. **Estoque e compras:** Entrada via XML de NF-e/manual, controle por unidades de medida, estoques mínimos e baixa automática.
5. **Ordem de produção (OP):** Fluxo Kanban por etapas (Modelagem, Corte, Costura, Acabamento, etc.) com controle de aproveitamento de couro.
6. **Vendas e faturamento:** Pedidos multicanal, formas de pagamento, comissões e emissão de notas fiscais.
7. **Expedição e pós-venda:** Separação, conferência por código de barras, embalagem e gestão de garantias (RMA).
8. **Gestão financeira:** Contas a pagar/receber automáticas e indicadores gerenciais (DRE, Curva ABC, Fluxo de Caixa).

---

## 3. Requisitos do Sistema

### 3.1 Requisitos Funcionais
* **RF01:** Cadastrar clientes com razão social/nome, CNPJ/CPF, inscrição estadual, e-mail para NF-e, telefone e comprador responsável.
* **RF02:** Registrar múltiplos endereços por cliente (matriz, entrega, cobrança).
* **RF03:** Diferenciar clientes por perfil (Varejo Final x Atacado/Lojista) com regras de preço distintas.
* **RF04:** Consultar restrições de CPF/CNPJ, aplicar limite de crédito para boletos e exigir aprovação financeira para novos lojistas.
* **RF05:** Cadastrar fornecedores detalhando categoria de insumo, vendedor e prazo médio de entrega.
* **RF06:** Registrar entrada de insumos por lote (rastreabilidade de cor, tonalidade e preço pago).
* **RF07:** Manter histórico de compras e avaliação de desempenho por fornecedor.
* **RF08:** Cadastrar produtos por modelo e variação (SKU), combinando cores, couros e ferragens.
* **RF09:** Manter a Ficha Técnica (BOM) de cada SKU com as quantidades necessárias de insumos.
* **RF10:** Calcular automaticamente custos e sugerir preços de venda com base na Ficha Técnica, mão de obra e markup.
* **RF11:** Registrar entradas de matéria-prima via XML de NF-e ou digitação manual.
* **RF12:** Controlar estoque de insumos nas unidades apropriadas com alertas automáticos de estoque mínimo.
* **RF13:** Realizar a baixa automática de insumos no estoque ao abrir uma Ordem de Produção.
* **RF14:** Abrir Ordens de Produção vinculadas a SKUs com acompanhamento via painel Kanban.
* **RF15:** Registrar artesãos/facções responsáveis por etapa e o percentual de aproveitamento/perda de couro no corte.
* **RF16:** Registrar pedidos de venda multicanal com formas de pagamento e parcelamento.
* **RF17:** Calcular comissões de vendedores/representantes com percentuais customizados por canal.
* **RF18:** Emitir NF-e e NFC-e nativamente ou via integração.
* **RF19:** Controlar separação, conferência por código de barras e embalagem (checklist e *dust bag*).
* **RF20:** Integrar com serviços de logística para emissão de etiquetas e rastreio.
* **RF21:** Registrar trocas, devoluções e garantias (RMA) vinculadas a itens específicos do pedido.
* **RF22:** Gerar lançamentos automáticos de Contas a Pagar e Contas a Receber.
* **RF23:** Gerar relatórios gerenciais (Curva ABC, margens de lucro, DRE, fluxo de caixa e estoque parado).
* **RF24:** Restringir acessos por perfil de usuário (Vendedor, Gerente de Produção, Financeiro, Administrador).

### 3.2 Requisitos Não Funcionais
* **RNF01 (Desempenho):** Consultas de estoque e ficha técnica devem responder agilmente para não atrasar a produção.
* **RNF02 (Segurança):** Controle rigoroso de acessos por perfil e proteção de dados em conformidade com a LGPD.
* **RNF03 (Disponibilidade):** O módulo de estoque/OP deve estar sempre disponível nos horários produtivos por conta das baixas bloqueantes.
* **RNF04 (Usabilidade):** O painel Kanban deve ser intuitivo para operação direta por artesãos e facções.
* **RNF05 (Integridade Transacional):** Baixas de estoque, títulos financeiros e emissão fiscal devem ocorrer de forma atômica.
* **RNF06 (Auditabilidade):** Preservação do histórico de preços de fornecedores e status de OPs para consultas futuras.

---

## 4. Regras de Negócio

* **Regras Operacionais:**
  * A Inscrição Estadual é obrigatória apenas para clientes do perfil Atacado/Lojista.
  * Cada variação de produto (SKU) pertence obrigatoriamente a um único modelo.
  * A baixa de insumos no estoque é imediata e automática na abertura da Ordem de Produção.
  * Todo insumo recebido exige amarração de lote para garantia de uniformidade de cor e textura nas coleções.
  * A comissão padrão de vendas é de 5%, variando conforme o canal comercial.
  * Chamados de garantia (RMA) vinculam-se estritamente ao item do pedido entregue, nunca ao pedido global.
  * A expedição de mercadorias só é liberada após conferência por código de barras e checklist de saída.

* **Restrições Organizacionais:**
  * Conformidade fiscal obrigatória para emissão de notas (NF-e/NFC-e).
  * Proteção obrigatória de dados de clientes e fornecedores conforme diretrizes da LGPD.
  * O *lead time* (prazo de entrega) de cada fornecedor dita o planejamento de compras antecipadas.
  * O pagamento de prestadores terceirizados exige registro fiel das etapas executadas.

---

## 5. Dicionário de Dados Conceitual (Preliminar)

> *Nota: O sistema completo abrange 21 entidades estruturadas para cobrir todo o ciclo operacional.*

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

## 6. Modelagem Conceitual

* **Total de Entidades:** 21 entidades estruturadas cobrindo blocos de Clientes, Suprimentos, Produtos, Produção, Vendas, Faturamento e Pós-venda.
* **Componentes Estruturais:** Contém 2 entidades fracas (`ENDERECO_CLIENTE`, `LOTE_INSUMO`) e 3 entidades associativas para resolução de relacionamentos N:N com atributos próprios (`ITEM_FICHA_TECNICA`, `EXECUCAO_ETAPA`, `ITEM_PEDIDO`).
* **Relacionamentos Principais:** O modelo contempla 22 relacionamentos principais mapeados de forma a garantir integridade referencial, controle de limites de crédito e rastreabilidade de lotes produtivos.

---

## 7. Diagrama Entidade-Relacionamento (DER)

* Os arquivos gráficos completos em notação de Chen (compatível com o BrModeloWeb) encontram-se salvos na pasta **`diagrams/`** deste repositório (incluindo `DER_Bolsas_Couro.png` / `.svg`).
* O desenho prioriza a escalabilidade, mantendo catálogos independentes para usuários, etapas de produção e separação clara entre o operacional e o financeiro.

---

## 8. Justificativa Técnica

* **Por que 21 entidades:** Permite isolar conceitos essenciais e ciclos de vida distintos (ex: separar lotes de insumos do cadastro genérico de insumos para garantir rastreabilidade rigorosa de cores e texturas do couro).
* **Tratamento de Entidades Fracas e Associativas:** Evita o uso de chaves artificiais desnecessárias e acomoda atributos nativos de junção (como percentuais de perda no corte e preços praticados congelados no momento da venda).

---

## 9. Anexo — Modelo Conceitual Simplificado (9 Entidades)

Recorte simplificado voltado ao fluxo central de custos, produção e faturamento. Os dicionários detalhados e diagramas específicos deste recorte encontram-se na pasta **`docs/`**.
* **Entidades do recorte:** `FORNECEDOR`, `INSUMO`, `FICHA_TECNICA`, `PRODUTO` (com especialização total e disjunta em `BOLSA` e `ACESSORIO`), `CLIENTE`, `PEDIDO` e `ITEM_PEDIDO`.

---

## 10. Uso de Inteligência Artificial

Documentação oficial do suporte tecnológico utilizado no desenvolvimento:

| Item | Descrição do Registro |
|---|---|
| **Ferramenta e Etapa** | Claude (Anthropic) — Utilizado para estruturação do levantamento de requisitos, tradução para o modelo conceitual de 21 entidades, conversão para notação de Chen e redação orientada da documentação. |
| **Motivação** | Otimizar a modelagem dos dados textuais em artefatos estruturados e aderentes às exigências metodológicas da disciplina. |
| **Prompts Principais** | Solicitações direcionadas à montagem de dicionários de dados e adaptação de diagramas para o modelo BrModeloWeb. |
| **Correções manuais** | Ajustes estruturais finos na remoção visual de chaves estrangeiras de diagramas conceituais puros e alinhamento estrito às regras da organização real analisada. |
