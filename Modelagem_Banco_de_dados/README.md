# Projeto de Sistema de Gestão — Indústria de Bolsas de Couro
**Contrasti Bolsas e Acessórios Ltda**

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
* **Natureza e Porte:** Empresa com fins lucrativos que atua simultaneamente como indústria e comércio (venda em loja física, e-commerce, WhatsApp e representantes externos). Operação de pequeno a médio porte, com produção sob encomenda/lote e volume médio de 50 a 70 pedidos mensais (picos em datas comemorativas).
* **Problemas Identificados:** Gestão descentralizada/manual de estoque sem rastreabilidade de lotes, ausência de regras formais de crédito para vendas a prazo, precificação não padronizada, falta de visibilidade no chão de fábrica e sistemas desintegrados.
* **Justificativa:** Escolha estratégica baseada na facilidade de acesso operacional e na complexidade equilibrada para o escopo do projeto.
* **Evidências:** 
  * *Endereço:* Rua Alpiste, 116 - Jd. Eliane - São Paulo - SP.
  * *Contato:* Osmar Lingiard (Tel: 11 97334-4846 | E-mail: osmar@specia.com.br).

---

## 2. Processos de Negócio
1. **Cadastro e Gestão de Clientes:** Perfis diferenciados (Varejo x Atacado) e controle de crédito.
2. **Gestão de Fornecedores:** Homologação por categoria de insumo e histórico de entregas.
3. **Engenharia de Produto (Ficha Técnica):** Estrutura de Modelos, SKUs e Listas de Materiais (BOM).
4. **Controle de Estoque e Compras:** Entradas via XML/Manual com alertas de estoque mínimo e baixa automática.
5. **Chão de Fábrica (Ordens de Produção):** Acompanhamento via Kanban e controle de aproveitamento de couro.
6. **Vendas e Faturamento:** Emissão de notas fiscais multicanal (NF-e/NFC-e) e cálculo de comissões.
7. **Expedição e Pós-Venda:** Processos de *picking & packing*, rastreio logístico e gestão de garantias (RMA).
8. **Gestão Financeira:** Automação de Contas a Pagar/Receber e indicadores gerenciais (DRE, Curva ABC).

---

## 3. Requisitos do Sistema

### 3.1 Requisitos Funcionais (Resumo)
* **RF01 a RF04:** Cadastro, múltiplos endereços, segmentação de perfis e gestão/limite de crédito de clientes.
* **RF05 a RF07:** Homologação, rastreabilidade de lotes de insumos e histórico de fornecedores.
* **RF08 a RF12:** Cadastro de SKUs, Ficha Técnica (BOM), cálculo automatizado de custos/preços e controle de estoque com unidades específicas.
* **RF13 a RF15:** Baixa automática de insumos, abertura de Ordens de Produção com painel Kanban e apontamento de produção por etapa/artesão.
* **RF16 a RF18:** Registro de pedidos multicanal, cálculo de comissões e emissão nativa/integrada de notas fiscais.
* **RF19 a RF21:** Expedição com código de barras, integração logística e controle de trocas/RMA por item.
* **RF22 a RF24:** Automação financeira (Pagar/Receber), geração de relatórios gerenciais e controle de acesso baseado em perfis.

### 3.2 Requisitos Não Funcionais
* **RNF01 (Desempenho):** Resposta ágil em consultas de estoque e fichas técnicas para não gargalar a produção.
* **RNF02 & RNF03 (Segurança e Disponibilidade):** Proteção de dados conforme a LGPD e alta disponibilidade do módulo de estoque/OP por ser bloqueante.
* **RNF04 a RNF06 (Usabilidade e Integridade):** Interface intuitiva para o Kanban de chão de fábrica, transações atômicas (tudo ou nada) e auditabilidade completa de preços e status.

---

## 4. Regras de Negócio e Restrições
* **Inscrição Estadual:** Obrigatória exclusivamente para clientes do perfil Atacado/Lojista.
* **Rastreabilidade:** Todo insumo recebido gera um lote vinculado ao fornecedor para manter a uniformidade de cor e textura na coleção.
* **Baixa de Estoque:** Ocorre de forma automática e imediata no momento da abertura da Ordem de Produção.
* **Comissão Padrão:** Definida em 5% sobre pedidos faturados, variando conforme o canal de venda.
* **Garantia (RMA):** Vinculada estritamente ao item específico do pedido entregue, e nunca ao pedido de forma global.

---

## 5. Dicionário de Dados Conceitual (Modelo Principal — 21 Entidades)
*(O modelo completo abrange as seguintes entidades organizadas por blocos operacionais)*
* **Clientes e Endereços:** `CLIENTE`, `ENDERECO_CLIENTE`
* **Suprimentos e Compras:** `FORNECEDOR`, `INSUMO`, `LOTE_INSUMO`, `COMPRA`, `CONTAS_PAGAR`
* **Produtos e Produção:** `MODELO_PRODUTO`, `VARIACAO_PRODUTO`, `ITEM_FICHA_TECNICA`, `ARTESAO_FACCAO`, `ETAPA_PRODUCAO`, `ORDEM_PRODUCAO`, `EXECUCAO_ETAPA`
* **Comercial e Financeiro:** `USUARIO`, `PEDIDO_VENDA`, `ITEM_PEDIDO`, `NOTA_FISCAL`, `CONTAS_RECEBER`, `EXPEDICAO`, `RMA_GARANTIA`

---

## 6. Modelagem e Justificativa Técnica
* **Arquitetura de 21 Entidades:** Garante o isolamento correto de ciclos de vida distintos (ex: separar `LOTE_INSUMO` de `INSUMO` para rastreabilidade fiscal e física).
* **Entidades Associativas:** Utilizadas (`ITEM_FICHA_TECNICA`, `EXECUCAO_ETAPA`, `ITEM_PEDIDO`) para acomodar atributos próprios de relacionamentos N:N, como percentuais de perda no corte e preços congelados na venda.
* **Escalabilidade:** O modelo desacopla blocos operacionais dos gerenciais, facilitando futuras evoluções para Business Intelligence (BI).

---

## 7. Anexo: Modelo Conceitual Simplificado (9 Entidades)
* **Escopo do Recorte:** Foco exclusivo no fluxo de engenharia, custos, formação de preço e vendas (Entidades: `FORNECEDOR`, `INSUMO`, `FICHA_TECNICA`, `PRODUTO`, `BOLSA`, `ACESSORIO`, `CLIENTE`, `PEDIDO`, `ITEM_PEDIDO`)[cite: 1, 2].
* **Dicionário Detalhado (Exemplo de Entidade Principal - PRODUTO):**
  * `id_produto`: Identificador / Chave primária do sistema[cite: 1, 2].
  * `nome_modelo`: Nome comercial descritivo (Ex: Bolsa Tote)[cite: 1, 2].
  * `custo_mao_obra`: Valor fixo de corte e costura por peça[cite: 1, 2].
  * `markup`: Multiplicador comercial de margem (> 1)[cite: 1, 2].
  * `custo_materia_prima (derivado)`: Somatório calculado dos insumos da ficha técnica[cite: 1, 2].
  * `preco_tabela (derivado)`: Preço sugerido de venda com base nos custos e markup[cite: 1, 2].

---

## 8. Uso de Inteligência Artificial
* **Ferramenta:** Claude (Anthropic).
* **Aplicações:** Apoio na estruturação lógica do levantamento de requisitos, conversão e refinamento do dicionário de dados e diagramação conceitual seguindo estritamente a Notação de Chen (BrModeloWeb) solicitada pelo docente.