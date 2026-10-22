# 📱 Sprint 3: Estrutura e Navegação (UX)

**Disciplina:** Prototipagem de Sistemas Computacionais
**Professora:** Profa. Ma. Angela P. Barcellos
**Aluno:** Guilherme da Silva
**Projeto:** AgroTech Monitor (gestão de hortas comunitárias do Sr. Carlos)

[⬅️ Voltar ao portfólio](../README.md)

## 🎯 Objetivo
Mostrar a estrutura e a navegação do aplicativo com um **wireframe de baixa fidelidade** (tons de cinza, bordas simples), pensado primeiro para o celular (*mobile-first*) usado no campo.

## 📁 Arquivos desta pasta
| Arquivo | O que é |
| :--- | :--- |
| `README.md` | Este documento |
| `wireframe.html` | Protótipo interativo da tela "Registrar Colheita" |
| `img/sprint3-wireframe.png` | Imagem da tela |

---

## 1. A tela: Registrar Colheita
![Wireframe](img/sprint3-wireframe.png)

| Elemento | Função |
| :--- | :--- |
| **Horta** (select) | Escolher onde a colheita foi feita |
| **Alimento** (select) | Escolher o tipo de alimento |
| **Quantidade (kg)** (campo numérico) | Informar o peso colhido |
| **SALVAR** (botão grande) | Grava o registro; botão largo para facilitar o toque no campo |
| **Menu inferior** | Navegação entre Início, Colheitas e Doações |

## 2. Fluxo de navegação
`Início` → `Colheitas` (esta tela) → preencher → **SALVAR** → mensagem "Colheita salva com sucesso" → `Doações`

## 3. Como testar o protótipo
- **Pelo site (mais fácil):** abra a página do portfólio no GitHub Pages; o wireframe funciona lá dentro.
- **No computador:** baixe `wireframe.html` e abra no navegador (o GitHub só mostra o código, não executa a página).

**Comportamento do botão SALVAR:** se faltar horta, alimento ou quantidade maior que zero, aparece um aviso; se estiver tudo certo, aparece "Colheita salva com sucesso!".

## 4. Decisões de design
- Botão grande e de largura total (uso com uma mão e luz forte).
- Rótulos claros, com a unidade (kg) no próprio texto.
- Menu inferior, no alcance do polegar.
