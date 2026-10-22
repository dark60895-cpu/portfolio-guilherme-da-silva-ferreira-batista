# 📐 Sprint 2: Diagramas UML e BPMN - AgroTech Monitor

**Disciplina:** Prototipagem de Sistemas Computacionais
**Professora:** Profa. Ma. Angela P. Barcellos
**Aluno:** Guilherme da Silva

## 1. Diagrama de Casos de Uso

> O Mermaid não tem tipo nativo de casos de uso, então usamos `flowchart`.

![Casos de uso](img/sprint2-casos-de-uso.png)

```mermaid
flowchart LR
    V(["👤 Voluntário"])
    G(["👤 Sr. Carlos (Gestor)"])
    O(["👤 ONG Beneficiária"])

    subgraph SIS["Sistema AgroTech Monitor"]
        UC1(("Registrar<br/>Colheita"))
        UC2(("Autenticar<br/>Usuário"))
        UC3(("Agendar<br/>Colheita"))
        UC4(("Gerenciar<br/>Estoque"))
        UC5(("Realizar<br/>Doação"))
        UC6(("Gerar Relatório<br/>Mensal"))
        UC7(("Emitir Alerta<br/>Estoque Baixo"))
        UC8(("Cadastrar<br/>Voluntário"))
        UC9(("Confirmar<br/>Recebimento"))
    end

    V --- UC1
    V --- UC3
    G --- UC4
    G --- UC5
    G --- UC6
    G --- UC8
    O --- UC9

    UC1 -.->|"«include»"| UC2
    UC4 -.->|"«include»"| UC2
    UC5 -.->|"«include»"| UC4
    UC7 -.->|"«extend»"| UC4
    UC9 -.->|"«extend»"| UC5
```

## 2. Diagrama de Classes

![Classes](img/sprint2-classes.png)

```mermaid
classDiagram
    class Voluntario {
        +int id
        +String nome
        +String email
        +String telefone
        +cadastrar()
        +autenticar()
    }
    class Horta {
        +int id
        +String nome
        +String localizacao
        +float areaM2
        +agendarColheita()
    }
    class Colheita {
        +int id
        +Date data
        +String alimento
        +float quantidadeKg
        +registrar()
    }
    class Doacao {
        +int id
        +Date data
        +String instituicaoDestino
        +float quantidadeKg
        +String status
        +realizar()
        +confirmarRecebimento()
    }
    Voluntario "1" --> "0..*" Colheita : realiza
    Horta "1" --> "0..*" Colheita : produz
    Voluntario "0..*" -- "0..*" Horta : atua em
    Colheita "1" --> "0..*" Doacao : origina
```

**Cardinalidades:** Voluntário→Colheita **1:N**; Horta→Colheita **1:N**; Voluntário↔Horta **N:N**; Colheita→Doação **1:N**.

## 3. Fluxo BPMN: Realizar Doação (TO-BE)

![BPMN](img/sprint2-bpmn.png)

```mermaid
flowchart LR
    subgraph L1["Raia: Sr. Carlos (Gestor)"]
        A((Início)) --> B["Consultar estoque<br/>no sistema"]
        B --> C{"Há estoque<br/>disponível?"}
        C -- Sim --> D["Selecionar ONG<br/>e alimentos"]
        D --> E["Registrar doação"]
    end
    subgraph L2["Raia: Sistema AgroTech"]
        E --> F["Validar quantidade<br/>e baixar estoque"]
        F --> G{"Estoque abaixo<br/>do mínimo?"}
        G -- Sim --> H["Emitir alerta"]
        G -- Não --> I["Notificar ONG"]
        H --> I
    end
    subgraph L3["Raia: ONG Beneficiária"]
        I --> J["Receber doação"]
        J --> K["Confirmar<br/>recebimento"]
    end
    K --> M["Atualizar status<br/>da doação"]
    M --> N(((Fim)))
    C -- Não --> O["Agendar nova<br/>colheita"]
    O --> P(((Fim)))
```

## 4. Especificação Textual: UC01 Registrar Colheita

| Campo | Descrição |
| :--- | :--- |
| **Ator principal** | Voluntário |
| **Objetivo** | Registrar tipo e quantidade de alimento colhido e atualizar o estoque. |
| **Pré-condições** | Voluntário autenticado; ao menos uma horta cadastrada. |
| **Pós-condições** | Colheita gravada e estoque atualizado. |
| **Requisitos** | RF03, RNF01, RNF03 |

**Fluxo Principal**
1. O voluntário acessa "Colheitas" e escolhe "Registrar Colheita".
2. O sistema exibe o formulário (horta, alimento, quantidade em kg).
3. O voluntário seleciona a horta.
4. O voluntário seleciona o alimento.
5. O voluntário informa a quantidade em kg.
6. O voluntário clica em "Salvar".
7. O sistema valida os dados.
8. O sistema grava a colheita e atualiza o estoque.
9. O sistema exibe "Colheita salva com sucesso" e encerra o caso de uso.

**Fluxos Alternativos**
- **A1: Registrar outra (passo 9).** O sistema limpa o formulário e volta ao passo 2.
- **A2: Cancelar (passos 2 a 6).** O sistema descarta os dados e volta à tela inicial.

**Fluxos de Exceção**
- **E1: Quantidade inválida (passo 7).** O sistema exibe "Informe uma quantidade maior que zero" e volta ao passo 5.
- **E2: Sessão expirada.** O sistema redireciona ao login e restaura o formulário após autenticar.
- **E3: Falha de conexão (passo 8).** O sistema guarda o registro localmente e sincroniza quando a conexão voltar.
