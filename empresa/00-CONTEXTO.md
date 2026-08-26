# Contexto canônico da empresa

> Fonte de verdade do cruzamento. Toda análise de edital lê este arquivo primeiro.
> Mapeado em 2026-08-26 a partir de `documentos/`. Não re-extraia os documentos:
> a transcrição abaixo já foi conferida. Só remapeie arquivo novo.

**Aviso:** Norte Software é empresa fictícia de material didático. Os documentos em
`documentos/` declaram, eles mesmos, que não têm valor legal. Nenhum parecer gerado
a partir deste contexto serve para protocolo real.

## 1. Identificação

| Campo | Valor | Documento de origem |
|---|---|---|
| Razão social | NORTE SOFTWARE LTDA (fantasia: Norte) | 01-cnpj.md |
| CNPJ | 32.847.119/0001-64 (matriz) | 01-cnpj.md |
| Porte | EPP | 01-cnpj.md; 08-simplificada.md |
| CNAE principal | 62.01-5-01 desenvolvimento de programas sob encomenda | 01-cnpj.md |
| CNAE secundários | 62.02-3-00 licenciamento customizável; 62.09-1-00 suporte técnico e manutenção em TI | 01-cnpj.md |
| Natureza jurídica | 206-2 Sociedade Empresária Limitada | 01-cnpj.md |
| Sede | Rua das Pedreiras, 88, Sala 3, Centro, Aracaju/SE, 49010-120 | 01-cnpj.md |
| Data de abertura | 12/03/2018 (8 anos em 2026) | 01-cnpj.md |
| Situação cadastral | ATIVA | 01-cnpj.md |
| NIRE | 28200999999 | 08-simplificada.md |
| Sócia administradora | Helena Vasconcelos, 100% do capital | 08-simplificada.md |
| Corpo técnico | 28 pessoas | CLAUDE.md do kit da Norte |
| Escopo declarado | desenvolvimento, manutenção e sustentação. **Não faz helpdesk nem infraestrutura** | CLAUDE.md do kit da Norte |

## 2. Qualificação econômico-financeira

| Índice | Valor | Documento de origem |
|---|---|---|
| Capital social | **R$ 80.000,00**, integralizado | 08-simplificada.md |
| Patrimônio líquido | não localizado nos documentos fornecidos | — |
| Ativo circulante / Passivo circulante | não localizado | — |
| CCL / Capital de giro | não localizado | — |
| Liquidez corrente, liquidez geral, solvência geral | não calculáveis | — |
| Balanço patrimonial registrado | **AUSENTE** | — |

> **Lacuna crítica.** Não há balanço patrimonial em `documentos/`. Todo edital que
> exija LG, SG, LC, PL mínimo ou CCL fica sem comprovação até o balanço registrado
> na Junta ser anexado. O capital social de R$ 80.000,00 é o único lastro
> documentado, e é baixo para certame de vulto.

### Certidões (todas emitidas em 01/08/2026)

| Certidão | Situação | Validade | Arquivo |
|---|---|---|---|
| Federal RFB/PGFN | negativa | 28/01/2027 | 02-rfb.md |
| FGTS (CRF) | regular | **30/08/2026** | 03-fgts.md |
| Trabalhista (CNDT) | negativa | 28/01/2027 | 04-trabalhista.md |
| Municipal (Aracaju) | quite | 30/09/2026 | 05-municipal.md |
| Estadual (SEFAZ/SE) | isento, sem pendência | 28/01/2027 | 06-estadual.md |
| Cível (falência/recuperação, TJSE) | nada consta | 28/01/2027 | 07-civil.md |
| Simplificada (JUCESE) | emitida | sem validade declarada | 08-simplificada.md |

> A CRF do FGTS é a de janela mais curta: 30 dias. É a primeira a vencer em
> qualquer certame.

## 3. Atestados de capacidade técnica

### A1. Instituto Laranjeiras de Sistemas (ILS) | fábrica de software
- **Arquivo:** `documentos/atestado-01-fabrica.md`
- **Emitente:** ILS, CNPJ 00.000.000/0001-91, Laranjeiras/SE. Pessoa jurídica de direito privado.
- **Período:** março/2025 a fevereiro/2026 (**12 meses cheios**)
- **Volume declarado:** **1.400 pontos de função**
- **Perfis:** 2 desenvolvedores júnior, tempo integral
- **Escopo:** concepção, projeto, desenvolvimento, testes funcionais, implantação e documentação do sistema de protocolo digital
- **Metodologia:** Scrum; backlog do produto; sprints de 2 semanas; burndown semanal; review; retrospectiva
- **Tecnologias:** Java, PostgreSQL, API REST
- **Assinante:** Helena Vasconcelos, Diretora de Tecnologia do ILS, em 15/03/2026

> **Ponto de atenção:** a assinante do atestado tem o mesmo nome da sócia
> administradora da Norte (seção 1). Homonímia ou não, é o tipo de coincidência que
> a banca levanta em diligência. Verificar antes de usar o A1 como atestado
> principal.

### A2. Companhia Ribeira de Pagamentos S/A | web e mobile
- **Arquivo:** `documentos/atestado-02-mobile.md`
- **Emitente:** Cia. Ribeira de Pagamentos S/A, CNPJ 00.000.000/0002-72, Aracaju/SE. Direito privado.
- **Período:** janeiro/2024 a dezembro/2025 (**24 meses**)
- **Volume declarado:** **14.400 horas** executadas, entregues e aceitas
  - 11.500 h (80%) web · 2.900 h (20%) mobile
  - Média de **7.200 h por 12 meses**
  - O atestado declara 1 UST = 1 hora, mas o número principal está **em horas**
- **Escopo:** desenvolvimento, sustentação, evolução, testes, implantação e documentação de soluções web e mobile
- **Metodologia:** Scrum, Kanban, TDD. Artefatos: backlog, release plan, planning, daily, review, retrospectiva, burndown
- **Tecnologias:** React, Java, Kotlin, PostgreSQL, REST, Git
- **Assinante:** Marcos Peixoto, Gerente de Tecnologia, em 10/01/2026

### A3. Central de Relacionamento Costa Azul | service desk
- **Arquivo:** `documentos/atestado-03-helpdesk.md`
- **Período:** julho/2024 a dezembro/2025 (18 meses)
- **Volume:** 22.400 chamados; 15 analistas de suporte
- **Escopo:** service desk, helpdesk, reset de senha, suporte de microinformática
- **Situação:** **não serve como atestado de fábrica de software.** Editais de
  desenvolvimento costumam vedar expressamente helpdesk e suporte de infra. Usar
  este atestado para comprovar fábrica é erro de habilitação, não é agressividade.

## 4. Capacidade consolidada

| Dimensão | Valor comprovado | Atestados |
|---|---|---|
| Experiência contínua | desde jan/2024 nos atestados; empresa aberta em 03/2018 | A1, A2 |
| Volume em pontos de função (nativo) | **1.400 PF em 12 meses** | A1 |
| Volume em horas (nativo) | **14.400 h em 24 meses** = 7.200 h por 12 meses | A2 |
| Volume em perfis alocados | 2 (A1). A2 não declara quantidade de perfis | A1 |
| Desenvolvimento, testes, implantação, documentação | sim | A1, A2 |
| Sustentação e evolução | sim | A2 |
| Métodos ágeis (Scrum, Kanban, TDD) | sim, com artefatos nomeados | A1, A2 |
| Versionamento e gestão de backlog | sim (Git, backlog, release plan) | A2 |
| Tecnologias | Java, Kotlin, React, PostgreSQL, REST, Git | A1, A2 |
| Mobile | sim, 2.900 h | A2 |
| **Modelo de maturidade (CMMI, MPS.BR)** | **não consta em nenhum atestado** | lacuna |
| **Segurança (DevSecOps, ISO 27001, Security by Design)** | **não consta** | lacuna |
| **Testes/QA quantificados em separado** | **não consta**; testes aparecem como atividade, sem horas nem perfis próprios | lacuna |
| Helpdesk, service desk, infraestrutura | sim (A3), mas é **contraindicado** em edital de fábrica | A3 |
| Certificações ISO, CMMI, MPS.BR, CFPS | não possui | lacuna |
| Registros INPI, CREA, CAU, CRA | não constam | lacuna |

**Equivalências padrão:** 1 UST = 1 HST = 1 hora. 1 PF = 10 horas (Portaria SGD/MGI
750/2023). **Se o edital fixar outra taxa, ou vedar conversão, o edital prevalece.**

> Atenção estrutural: os dois atestados válidos têm métrica **nativa** diferente
> (A1 em PF, A2 em horas). Isso é uma vantagem quando o edital veda conversão, e
> vira armadilha quando alguém soma os dois. **Nunca some PF com horas.**

## 5. Modelo de custo (base da sugestão de preço)

| Parâmetro | Valor | Observação |
|---|---|---|
| Tributos sobre receita (%) | **não informado** | depende do regime; não há documento fiscal na pasta |
| Overhead / custos indiretos (%) | **não informado** | |
| Margem de lucro alvo (%) | **não informado** | |
| Regime de contratação da equipe (CLT / PJ) | **não informado** | muda encargo e Fator-K de forma decisiva |
| Custo direto por perfil | **não informado** | |

> **Precificação bloqueada.** Nenhum parâmetro de custo da Norte foi fornecido. Todo
> parecer gerado a partir deste contexto deve dizer que o preço não foi calculado e
> listar exatamente o que falta, em vez de arbitrar número.

## 6. Documentos inválidos ou proibidos

- **A3 (helpdesk)** não pode ser apresentado como comprovação de fábrica de software.
- Todos os documentos da pasta são **material didático sem valor legal** e não podem
  ser protocolados em certame real.
