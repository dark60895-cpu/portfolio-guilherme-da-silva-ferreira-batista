# 📐 Sprint 2: A Planta Baixa do Sistema

## 🎯 Objetivo
Utilizar notações UML e BPMN para estruturar a lógica do sistema antes de qualquer design de tela.

## 1. Diagrama de Casos de Uso (Descrição)
**Atores:** Voluntário, Administrador (Sr. Carlos), Sistema de Notificação.
**Casos de Uso:**
- Registrar Colheita (Ator: Voluntário) - *<<include>> Autenticar Usuário*
- Agendar Colheita (Ator: Voluntário)
- Gerenciar Estoque (Ator: Administrador)
- Gerar Relatório de Doações (Ator: Administrador) - *<<extend>> Enviar por Email*
- Cadastrar Voluntário (Ator: Administrador)

## 2. Diagrama de Classes (Estrutura Lógica)
- **Voluntário:** Atributos (ID, Nome, CPF, Telefone). Métodos (cadastrar, agendar).
- **Horta:** Atributos (ID, Nome, Localização). Métodos (adicionarColheita).
- **Colheita:** Atributos (ID, Data, Quantidade, TipoAlimento). Métodos (calcularTotal).
- **Doação:** Atributos (ID, Data, Destino). 
**Relações:**
- Um **Voluntário** pode realizar várias **Colheitas** (1 para N).
- Uma **Horta** possui várias **Colheitas** (1 para N).
- Uma **Colheita** pode gerar uma ou várias **Doações** (1 para N).

## 3. BPMN do Fluxo "Realizar Doação" (TO-BE)
1. **Início:** Voluntário acessa o sistema.
2. **Tarefa:** Verifica estoque de alimentos.
3. **Gateway (Decisão):** Há estoque suficiente?
   - *Se Não:* O sistema exibe alerta e o fluxo termina.
   - *Se Sim:* Voluntário seleciona a instituição de caridade.
4. **Tarefa:** O sistema registra a doação e abate do estoque.
5. **Tarefa:** O sistema gera um comprovante.
6. **Fim:** Doação concluída.

## 4. Especificação Textual de Caso de Uso: Realizar Doação
- **Ator Principal:** Voluntário
- **Pré-condições:** Estar autenticado e haver estoque disponível.
- **Fluxo Principal:**
  1. O voluntário seleciona a opção "Realizar Doação".
  2. O sistema exibe os alimentos em estoque.
  3. O voluntário seleciona o alimento e a quantidade.
  4. O voluntário seleciona a instituição de destino.
  5. O sistema registra a doação e atualiza o estoque.
- **Fluxo Alternativo:** O voluntário pode buscar a instituição por nome ou CEP.
- **Fluxo de Exceção:** Se o sistema perder a conexão com o banco de dados, exibe a mensagem "Erro ao registrar doação. Tente novamente mais tarde".
