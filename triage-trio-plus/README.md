# Triage Trio Plus

Sistema web de gestão de fluxo de atendimento em pronto-socorro, cobrindo as três etapas do processo — **recepção**, **triagem de enfermagem** e **atendimento médico** — com um painel gerencial para acompanhar o andamento em tempo real.

## Sobre o projeto

O objetivo é simular e organizar a jornada de um paciente dentro de uma emergência hospitalar, desde a entrada até a alta, com cada equipe atuando na sua etapa:

1. **Recepção** — cadastra o paciente (nome, idade, sintomas, alergias, comorbidades) ao dar entrada.
2. **Enfermagem** — realiza a triagem: registra sinais vitais (temperatura, pressão arterial, saturação de O₂, frequência cardíaca e respiratória, glicemia) e classifica o risco do paciente pelo protocolo de cores, direcionando-o à especialidade adequada.
3. **Médico** — atende os pacientes chamados, define diagnóstico (com apoio de uma base de CID-10), medicamentos, procedimentos e observações da prescrição.
4. **Dashboard** — visão consolidada de todos os pacientes, com busca, filtros por cor de risco e por período (hoje/semana/mês), contagem por classificação e tempo médio de atendimento.

O acesso é controlado por papéis (recepção, enfermagem, médico), cada um enxergando apenas a tela correspondente à sua função.

## Classificação de risco

Baseada no Protocolo de Manchester:

| Cor | Critério (exemplo) | Tempo alvo |
| --- | --- | --- |
| 🔴 Vermelho | PA > 180/120 · Temp > 40°C | Imediato |
| 🟠 Laranja | PA > 160/100 · Temp > 39°C | 10 min |
| 🟡 Amarelo | Temp 38–38,9°C · PA moderada | 60 min |
| 🟢 Verde | Sinais vitais estáveis | 120 min |

## Tecnologias

- [React](https://react.dev/) + [TypeScript](https://www.typescriptlang.org/) + [Vite](https://vitejs.dev/)
- [shadcn/ui](https://ui.shadcn.com/) + [Tailwind CSS](https://tailwindcss.com/)
- [React Router](https://reactrouter.com/) · [React Hook Form](https://react-hook-form.com/) + [Zod](https://zod.dev/) · [TanStack Query](https://tanstack.com/query/latest) · [Recharts](https://recharts.org/)
- [Supabase](https://supabase.com/) (autenticação e papéis de usuário)
- [Vitest](https://vitest.dev/) para testes

## Como executar localmente

Pré-requisitos: [Node.js](https://nodejs.org/) (ou [Bun](https://bun.sh/)) instalado.

```bash
# 1. Clonar o repositório
git clone https://github.com/matheusmontagner8/triage-trio-plus.git
cd triage-trio-plus

# 2. Instalar as dependências
npm install

# 3. Configurar as variáveis de ambiente
# Crie um arquivo .env na raiz com:
# VITE_SUPABASE_URL=<url-do-seu-projeto-supabase>
# VITE_SUPABASE_PUBLISHABLE_KEY=<chave-publica-do-seu-projeto-supabase>

# 4. Rodar em modo desenvolvimento
npm run dev
```

Outros scripts disponíveis: `npm run build` (build de produção), `npm run lint` (lint) e `npm run test` (testes).

## Projeto

Desenvolvido com [Lovable](https://lovable.dev/).
