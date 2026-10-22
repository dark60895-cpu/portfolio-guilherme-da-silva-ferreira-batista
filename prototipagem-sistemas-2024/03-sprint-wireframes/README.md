# 📱 Sprint 3: Estrutura e Navegação (UX)

## 🎯 Objetivo
Demonstrar a arquitetura da informação e o fluxo de navegação através de wireframes (baixa fidelidade).

## 1. Estrutura de Telas (Wireframes Mentais)
- **Tela 1: Login**
  - Campos: E-mail, Senha.
  - Botão: "Entrar".
  - Link: "Esqueci minha senha".
- **Tela 2: Dashboard Principal (Home)**
  - Cabeçalho: Nome do usuário logado e foto.
  - Menu Inferior (Mobile): Início, Colheitas, Doações, Perfil.
  - Botões de Ação Rápida (Cards): "Registrar Colheita" e "Realizar Doação".
- **Tela 3: Cadastro de Voluntário**
  - Formulário simples: Nome, CPF, Telefone, E-mail.
  - Botão: "Cadastrar".
- **Tela 4: Registro de Colheita**
  - Dropdown: Selecionar a Horta.
  - Dropdown: Tipo de Alimento (Alface, Tomate, etc).
  - Campo Numérico: Quantidade (kg).
  - Botão Grande: "Salvar Registro".

## 2. Fluxo de Navegação do Usuário (Jornada)
1. Usuário abre o app e faz o **Login**.
2. Cai na tela **Dashboard**.
3. Clica no card **"Registrar Colheita"**.
4. Preenche o formulário e clica em **"Salvar"**.
5. O sistema exibe uma mensagem de sucesso ("Colheita registrada!") e retorna ao **Dashboard**.

## 3. Justificativa da Arquitetura de Informação
A disposição dos elementos prioriza a usabilidade no campo. O botão de "Salvar Registro" é grande e fixo na parte inferior da tela para facilitar o toque com uma mão só. O menu é simplificado para evitar sobrecarga cognitiva dos voluntários.
