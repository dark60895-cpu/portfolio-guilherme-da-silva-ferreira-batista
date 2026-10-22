# 🏁 Sprint 1: O Desafio AgroTech (Kick-off)

## 🎯 Objetivo
Aplicar técnicas de levantamento de requisitos (entrevistas, escuta ativa e empatia) para definir o escopo inicial e mapear as dores reais dos usuários das Hortas Comunitárias do Sr. Carlos.

## 1. Matriz de Stakeholders
- **Sr. Carlos (Alta influência / Alto interesse):** Administrador das hortas. Patrocinador do projeto. Precisa de relatórios e controle.
- **Voluntários (Baixa influência / Alto interesse):** Usuários do dia a dia. Precisam de facilidade para registrar colheitas.
- **Comunidade (Baixa influência / Baixo interesse):** Beneficiários das doações. Querem saber onde doar.
- **Órgãos Reguladores (Alta influência / Baixo interesse):** Precisam de transparência nos dados.

## 2. Backlog do Produto (Requisitos)
### Requisitos Funcionais (RF)
- **RF01:** O sistema deve permitir o cadastro de voluntários.
- **RF02:** O sistema deve permitir o agendamento de colheitas.
- **RF03:** O sistema deve registrar a quantidade e o tipo de alimento colhido.
- **RF04:** O sistema deve gerar um relatório de doações realizadas.
- **RF05:** O sistema deve emitir alertas de estoque baixo.

### Requisitos Não Funcionais (RNF)
- **RNF01 (Usabilidade):** A interface deve ser responsiva e operável em dispositivos móveis (celulares) no campo.
- **RNF02 (Segurança):** Os dados dos voluntários devem ser armazenados em conformidade com a LGPD.
- **RNF03 (Desempenho):** O sistema deve carregar o dashboard em menos de 3 segundos.

## 3. História de Usuário (User Story)
**Como** voluntário da horta, **eu quero** registrar a colheita do dia no sistema **para que** o Sr. Carlos saiba exatamente o que temos em estoque.

### Critérios de Aceite (Padrão BDD)
- **Dado que** eu sou um voluntário autenticado,
- **Quando** eu preencher o formulário de colheita (tipo de alimento e quantidade) e clicar em "Salvar",
- **Então** o sistema deve registrar a colheita e atualizar o estoque em tempo real.

## 4. Mapa da Jornada do Usuário (AS-IS)
1. **Ação:** Voluntário colhe os alimentos.
2. **Ação:** Anota a quantidade em um caderno de papel.
3. **Ação:** O caderno fica na sede da horta.
4. **Ação:** Sr. Carlos vai até a horta para buscar os dados e digitar em uma planilha no computador.
- **Ponto de Dor (Gargalo):** Perda de dados, atraso no envio das informações e risco de erros de digitação.
