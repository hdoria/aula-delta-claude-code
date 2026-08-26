# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## O que é

Ferramenta de análise de edital de licitação pública. Recebe um edital, cruza com a
documentação da empresa (cartão CNPJ, atestados de capacidade técnica, balanço
patrimonial, certidões) e entrega um parecer em HTML: participa ou não, o que falta
para habilitar, e qual faixa de preço ofertar.

Projeto de exemplo, construído do zero para aula de Claude Code. Não puxe dado nem
contexto de outro projeto de licitações da máquina: este aqui é autocontido.

A ferramenta **é uma skill**, não um app. Não há build, não há servidor, não há
dependência de runtime além do `pdftotext`. O pipeline inteiro vive em
`.claude/skills/edital-delta/SKILL.md`.

## Como rodar

```bash
pdftotext -v   # única dependência externa (poppler); brew install poppler
```

Fluxo, na ordem:

```
/edital-delta mapear                    # 1x: lê empresa/documentos/, preenche o contexto
/edital-delta editais/{certame}         # a cada edital: gera o parecer
open pareceres/{arquivo}.html           # confere o resultado
```

O modo `mapear` é pré-requisito. Enquanto `empresa/00-CONTEXTO.md` estiver marcado
como TEMPLATE NÃO PREENCHIDO, a análise não tem base de comparação e não deve rodar.

## Estrutura

```
empresa/
  00-CONTEXTO.md      ← fonte de verdade: identificação, índices, atestados,
                        capacidade consolidada, modelo de custo
  documentos/         ← PDFs originais (cartão CNPJ, atestados, balanço, certidões)
editais/{certame}/    ← PDFs do certame (EDITAL, TR, ETP, anexos)
pareceres/            ← saída: um HTML por certame
.claude/skills/edital-delta/
  SKILL.md            ← o pipeline inteiro
  template-parecer.html
```

## Como as peças se ligam

O ponto que não é óbvio olhando os arquivos: **`00-CONTEXTO.md` é uma cache
conferida, não um índice.** Os PDFs em `empresa/documentos/` são extraídos uma única
vez, no modo `mapear`, e transcritos para lá. Da segunda análise em diante ninguém
volta no PDF: lê-se o contexto.

Isso não é economia de token, é correção. Extração de PDF varia entre execuções,
principalmente em documento escaneado, e um número de balanço que muda entre uma
análise e outra derruba a credibilidade do parecer inteiro. A transcrição foi
conferida com humano uma vez; é ela que vale.

Consequência prática: documento novo em `empresa/documentos/` **não entra sozinho na
análise**. Ou passa pelo `mapear`, ou não existe.

O mesmo vale ao contrário para o edital: o edital nunca é cacheado. É lido a cada
análise, direto do PDF, porque cada certame é um documento diferente.

## Regras que não se negocia

**Todo requisito citado carrega o item de origem.** Formato `item 10.6.1 do TR`,
`item 7.7 do Edital`. Requisito sem origem citada não entra no parecer.

**Lacuna é escrita, não preenchida.** Dado que não apareceu no texto extraído vira
literalmente `não localizado no texto extraído`. Nunca complete com suposição, com
conhecimento geral sobre licitação, nem com o que costuma constar em edital
parecido. Parecer com lacuna honesta é útil. Parecer com número inventado faz perder
certame e pode virar problema em diligência.

**Data vem do bash.** `date +%Y-%m-%d` antes de classificar sessão como futura ou
vencida. Nunca de memória.

**Modalidade vem do edital**, não do nome da pasta. PE (pregão eletrônico), DE
(dispensa eletrônica) e CD (concorrência) precificam e habilitam diferente.

**Conversão de volume mostra a conta.** Padrão: 1 UST = 1 HST = 1 hora, 1 PF = 10
horas (Portaria SGD/MGI 750/2023). Se o edital fixar outra taxa, a do edital
prevalece, sempre.

**Preço é faixa, com memória de cálculo.** Nunca um número solto. Break-even, piso
defensável e alvo, com as linhas da conta visíveis. E o piso de inexequibilidade do
edital é checado antes: lance abaixo dele é desclassificação, não agressividade.

**Nada de documento retroativo.** Se falta lastro de relação antiga, o caminho é
termo assinado hoje com cláusula de reconhecimento mais prova contemporânea. Nunca
data retroativa nem assinatura simulada.

## Decisão

Escada de cinco níveis, aplicada em ordem, parando na primeira que bater. Está
detalhada no passo 4 da skill. Os vereditos são exatamente estes quatro, sem
categoria intermediária e sem "depende":

| Selo | Quando |
|---|---|
| ✅ PARTICIPAR | tudo coberto, nenhuma certificação faltante |
| ⚠️ VERIFICAR | falta um ponto crítico verificável, com o que checar dito |
| 🔍 ANALISAR | requisito não consta em nenhum atestado, precisa levantar outro |
| ❌ NÃO PARTICIPAR | bloqueio estrutural |

## Saída

HTML de arquivo único em `pareceres/`, sem CDN e sem build, abre com duplo clique e
imprime em PDF limpo. Tokens visuais, tema claro e escuro e estrutura das seções
estão em `.claude/skills/edital-delta/template-parecer.html`. Ao mexer no visual,
mexa no template, não no parecer gerado.

## Dado sensível

O kit desta aula é fictício. Norte Software não existe, e cada documento em
`empresa/documentos/` declara no próprio corpo que não tem valor legal. Os PDFs em
`editais/` são de licitação pública. Por isso o repositório é público e versiona
tudo: sem `empresa/documentos/` e sem o parecer de exemplo, o exercício não roda.

A regra muda quando o dado é real. Ao apontar esta ferramenta para a documentação de
uma empresa de verdade, `empresa/documentos/` e `pareceres/` entram no `.gitignore`
antes do primeiro commit: ali tem CNPJ, balanço e atestado com nome de pessoa.
