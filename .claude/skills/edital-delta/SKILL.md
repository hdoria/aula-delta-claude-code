---
name: edital-delta
description: >
  Analisa um edital de licitação pública, cruza com a documentação da empresa
  (cartão CNPJ, atestados de capacidade técnica, balanço patrimonial, certidões) e
  gera um parecer em HTML dizendo se dá para participar, o que falta, qual faixa
  de preço ofertar e qual o preço médio que o mercado pratica num contrato como aquele. Use SEMPRE que o usuário mencionar "edital", "licitação",
  "pregão", "dispensa eletrônica", "concorrência", "posso participar", "habilitação
  técnica", "qualificação técnica", "atestado de capacidade", "analisa esse edital",
  "preço de mercado desse contrato" ou pedir parecer sobre um certame. Também usar no modo "mapear", que lê os
  documentos da empresa e preenche o contexto canônico.
---

# edital-delta · análise de edital versus documentação da empresa

Dois modos. Se `empresa/00-CONTEXTO.md` ainda estiver marcado como TEMPLATE NÃO
PREENCHIDO, rode o **modo mapear** antes de analisar qualquer edital.

---

## Regra zero

`empresa/00-CONTEXTO.md` é a fonte de verdade. Leia antes de qualquer passo.

Depois que um documento da empresa foi transcrito para lá, **não se re-extrai o PDF**.
Isso não é otimização, é correção: a transcrição foi conferida uma vez, a re-extração
introduz variação. Só remapeie arquivo que ainda não conste no contexto.

**Regra anti-invenção (vale para os dois modos):** todo requisito citado no parecer
carrega o item de origem, no formato `item 10.6.1 do TR` ou `item 7.7 do Edital`.
Dado que você não localizou vira literalmente `não localizado no texto extraído`.
Nunca preencha lacuna com suposição, com "provavelmente" ou com conhecimento geral
sobre licitações. Um parecer com lacuna honesta é útil; um parecer com número
inventado faz perder certame.

---

# MODO MAPEAR

Dispara quando o usuário pede para mapear, cadastrar ou atualizar a documentação da
empresa, ou quando o contexto está vazio.

## M1. Inventariar

```bash
ls -la empresa/documentos/
```

Classifique cada arquivo: cartão CNPJ, contrato social, balanço patrimonial,
atestado de capacidade técnica, certidão (negativa de débito, FGTS, trabalhista),
declaração, outro.

## M2. Extrair um documento por vez

```bash
pdftotext -layout "empresa/documentos/ARQUIVO.pdf" /tmp/doc.txt
wc -c /tmp/doc.txt
```

Se o texto sair vazio ou quase vazio, o PDF é imagem escaneada. Use a ferramenta Read
direto no PDF, que enxerga a página. **Não marque "verificação manual necessária"
sem antes tentar a leitura visual.**

Transcreva para a seção correspondente do `00-CONTEXTO.md`, sempre com o nome do
arquivo de origem e a página.

## M3. Consolidar

Preencha a seção 4 (capacidade consolidada) somando os atestados. Some volumes só
quando as unidades forem compatíveis, e diga explicitamente qual conversão usou.

Marque as lacunas com franqueza: certificação que a empresa não tem, registro que
não possui, tecnologia fora do stack. **A lacuna registrada é o que impede a análise
seguinte de alucinar capacidade.**

## M4. Gate de mapeamento

Antes de encerrar, mostre ao usuário: quantos documentos foram lidos, o que entrou no
contexto e o que ficou de fora e por quê. Peça confirmação dos números do balanço,
que é onde erro passa despercebido.

---

# MODO ANALISAR

## 1. Localizar o edital

Os PDFs ficam em `editais/{identificador-do-certame}/`. Use somente os documentos
oficiais para extrair requisito:

1. `EDITAL*.pdf`
2. `Termo de Referencia*.pdf`, `TR*.pdf`, `SEI*TermodeReferencia*.pdf` (concentra a
   qualificação técnica)
3. `ETP*.pdf` (Estudo Técnico Preliminar): consulte quando o valor estimado for
   sigiloso, é lá que costuma estar a memória de cálculo da estimativa
4. Anexo específico de qualificação técnica

Pastas de edital acumulam planilha de lance e arquivo de trabalho. Ignore na extração
de requisito.

**Data da sessão sempre comparada com o bash, nunca de memória:**

```bash
date +%Y-%m-%d
```

Se a sessão já passou, diga isso de cara e pergunte se o usuário quer o parecer
mesmo assim.

## 2. Extrair requisitos

```bash
pdftotext -layout "editais/CERTAME/TR.pdf" /tmp/tr.txt
grep -n -i -A 60 "QUALIFICA..O T.CNICA\|HABILITA..O T.CNICA\|atestado" /tmp/tr.txt | head -200
grep -n -i "ponto de fun..o\|pontos de fun..o\|UST\|HST\|hora t.cnica\|anos de experi.ncia\|CFPS\|ISO\|CMMI\|MPS.BR\|INPI\|CREA\|CAU\|CRA\|SICAF\|vistoria\|inexequ" /tmp/tr.txt
grep -n -i "liquidez\|solv.ncia\|patrim.nio l.quido\|capital circulante\|.ndice" /tmp/tr.txt
```

Trabalhe com grep sobre o texto extraído em vez de carregar o PDF inteiro no contexto.

Capture, cada um com o item de origem:

| Campo | Onde costuma estar |
|---|---|
| Órgão, objeto, data e hora da sessão | capa do edital |
| Modalidade (PE, DE, CD) e se é SRP | capa |
| Valor estimado, ou "sigiloso" | edital, senão ETP |
| Vigência e prorrogação | minuta de contrato |
| Modelo de remuneração (UST, PF, hora, PPA, Fator-K) | TR |
| Anos mínimos de experiência | TR, qualificação técnica |
| Volume mínimo e a equivalência que o edital fixa | TR |
| Tecnologias exigidas | TR |
| Certificações exigidas | TR |
| Restrição sobre o emitente do atestado | TR |
| Índices econômico-financeiros mínimos | edital, qualificação econômica |
| Requisito complementar (INPI, conselho, vistoria) | edital |
| Regra de inexequibilidade | edital, seções 7.x, ou TR 3.3.x |
| Piso salarial de referência | TR, normalmente Portaria SGD/MGI 750/2023 |
| Tratamento favorecido ME/EPP e cotas | capa |

## 3. Cruzar requisito contra evidência

Uma linha por requisito. Nunca agrupe requisitos diferentes numa linha só.

| Requisito (com item) | Exigido | Comprovado | Documento que comprova | Situação |
|---|---|---|---|---|

Situação de cada linha: `ATENDE`, `NÃO ATENDE`, `PARCIAL` ou `NÃO LOCALIZADO`.

Converta volumes com a taxa correta. A do edital vence a padrão. Mostre a conta.

## 4. Decidir

Aplique **nesta ordem** e pare na primeira que bater:

1. **Sessão já passou** → fora da análise, salvo pedido explícito de histórico.
2. **Bloqueio estrutural** → ❌ NÃO PARTICIPAR. São bloqueios: registro em conselho
   profissional obrigatório que a empresa não tem, tecnologia core fora do stack,
   objeto fora do ramo de atividade, índice econômico abaixo do mínimo sem
   possibilidade de capital integralizado.
3. **Tudo coberto, sem certificação faltante** → ✅ PARTICIPAR.
4. **Falta um ponto crítico verificável** → ⚠️ VERIFICAR. Diga exatamente o que
   checar, onde checar e até quando.
5. **Volume ok, mas requisito técnico ou de domínio não consta em nenhum atestado**
   → 🔍 ANALISAR. Só participa se levantar atestado adicional.

Não invente sexta categoria. Não devolva "depende".

## 5. Sugerir preço

Só quando o modelo de custo da seção 5 do `00-CONTEXTO.md` estiver preenchido. Se não
estiver, escreva no parecer que a precificação não foi calculada por falta de
parâmetros de custo, e liste quais faltam.

Passos:

1. Identifique o modelo de remuneração do edital (UST/hora, PF, PPA por perfil,
   Fator-K sobre salário-base). Cada um precifica diferente.
2. Monte o custo direto pelos perfis exigidos no TR e pela quantidade estimada.
3. Aplique a fórmula do contexto:
   `preço = custo_direto × (1 + overhead) ÷ (1 − tributos − margem)`
4. **Ache o piso legal do edital antes de sugerir lance.** Muitos definem
   inexequibilidade por percentual do estimado, por Fator-K mínimo, ou por salário
   abaixo do piso da Portaria 750. Sugerir abaixo do piso é sugerir desclassificação.
5. Entregue uma **faixa**, não um número: preço de custo (break-even), preço alvo
   (com margem) e piso defensável (o menor lance que ainda sobrevive a diligência).
6. Mostre a memória de cálculo linha a linha. Planilha auditável vale mais que
   planilha bonita, e o mesmo vale para parecer.

Quando a empresa paga 13 honorários e fatura 12, o fator 1,0833 é obrigatório na
conta. Esquecer isso infla a margem aparente em cerca de 4 pontos.

## 6. Preço médio de mercado

Responde a "quanto o mercado cobraria por um contrato como este". É a pergunta que o
usuário faz com mais frequência e a que mais convida à invenção. **Todo número desta
seção precisa de fonte citável.** Se não houver fonte, a seção diz que não há base e
fica sem tabela. Estimativa de cabeça é invenção com aparência de análise.

Onde a fonte costuma estar, em ordem de qualidade:

1. **Anexo de pesquisa de preços do próprio edital.** Costuma se chamar Mapa
   Referencial de Preços, Cesta de Preços ou Estudo de Referenciamento. É a melhor
   fonte, porque é o número que a própria Administração apurou para aquele objeto.
2. **Acórdão do TCU citado no edital.** Vários editais de TIC ancoram a faixa aceitável
   num acórdão. Cite o número do acórdão.
3. **Portaria SGD/MGI de referenciamento salarial** (750/2023, 6.040/2025 e sucessoras),
   quando o modelo for salário-base mais multiplicador.
4. **ETP**, quando o edital omitir o valor estimado.
5. **Resultado de certames comparáveis**, se o usuário tiver a base. Registre qual.

Monte a faixa conforme o modelo de remuneração:

| Modelo | O que varrer |
|---|---|
| Fator K sobre salário-base | piso de inexequibilidade, piso e teto da faixa de referência, ponto médio, teto do edital |
| UST ou hora | preço unitário nos mesmos cinco pontos |
| Ponto de função | preço por PF nos mesmos cinco pontos |
| PPA por perfil | some os perfis em cada cenário |

Entregue sempre: a **faixa**, o **ponto médio** rotulado como média de mercado, o
valor do contrato inteiro nessa média e o **percentual do estimado** que ela
representa. O percentual é o que torna a comparação legível.

Abra a composição do multiplicador quando o edital publicar (encargos, benefícios,
tributos, margem). É isso que permite dizer o que está sendo comprimido quando alguém
oferta abaixo do piso técnico.

Compare o teto do edital com o topo da faixa de referência e comente a diferença. Teto
acima da referência significa folga real na disputa; teto abaixo significa certame
apertado, onde margem fina é o normal e não a exceção.

**Duas ressalvas obrigatórias no texto**, sempre:
- Se o regime de contratação da fonte (quase sempre CLT) for diferente do da empresa, a
  média não é o custo dela.
- Média de mercado diz onde a disputa costuma parar, não se a empresa aguenta chegar
  lá. Ela nunca substitui o cálculo do passo 5.

---

## 7. Gate antes de gerar o HTML

Confira e responda por escrito, para você mesmo, antes de renderizar:

1. Toda linha da tabela de cruzamento tem item de origem citado?
2. Tempo de experiência: coberto?
3. Volume: coberto, com a conversão correta e mostrada?
4. Tecnologias: cobertas?
5. Existe certificação exigida que a empresa não tem?
6. Emitente do atestado é compatível com a restrição do edital?
7. Índice econômico-financeiro: atende?
8. Existe requisito complementar bloqueante?
9. A faixa de preço respeita o piso de inexequibilidade do edital?
10. Cada linha do preço de mercado tem fonte citada, ou a seção declara que não há base?

Qualquer resposta sem evidência manda voltar ao passo 2.

## 8. Gerar o parecer

Renderize `pareceres/{AAAA-MM-DD}_{ORGAO}_{MODALIDADE}-{NUMERO}.html` seguindo
`template-parecer.html`, que já traz os tokens visuais, o tema claro e escuro e a
estrutura das seções. Arquivo único, sem dependência externa, abre com duplo clique.

Seções obrigatórias, nesta ordem:

1. **Veredito** no topo, com o selo colorido e uma frase de justificativa
2. **Identificação do certame** (órgão, objeto, modalidade, sessão, valor, vigência)
3. **Cruzamento requisito a requisito**, a tabela do passo 3, com item de origem
4. **Lacunas e pendências**, o que fazer e até quando
5. **Sugestão de preço**, faixa mais memória de cálculo
6. **Riscos e observações**, incluindo regra de inexequibilidade e prazo recursal
7. **Preço médio de mercado**, a faixa do passo 6 com as fontes citadas

Depois de gerar, abra para conferir:

```bash
open "pareceres/ARQUIVO.html"
```

## 9. Resumo no chat

```
Parecer: {ÓRGÃO} | {MODALIDADE} {NÚMERO} | sessão {data}

{SELO} {VEREDITO}
{uma frase de justificativa}

Atende: {n} de {N} requisitos
Pendências: {lista curta, ou "nenhuma"}
Faixa sugerida: {piso} a {alvo} por {unidade}
Média de mercado: {ponto médio} ({% do estimado} do estimado)

Parecer: pareceres/{arquivo}.html
```

---

## Checklist final

- [ ] Data da sessão comparada com `date`, não de memória
- [ ] Modalidade lida do edital, não deduzida do nome do arquivo
- [ ] Todo requisito com item de origem citado
- [ ] Lacuna escrita como "não localizado no texto extraído", nunca preenchida
- [ ] Conversão de volume com a taxa do edital quando ele fixar uma
- [ ] Faixa de preço acima do piso de inexequibilidade do edital
- [ ] Fator 13/12 aplicado quando cabe
- [ ] Preço médio de mercado com fonte citada em cada linha, ou seção declarada sem base
- [ ] Ressalva de regime (CLT versus outro) escrita quando a fonte for de regime diferente
- [ ] HTML abre sozinho, sem CDN, sem build
- [ ] `00-CONTEXTO.md` atualizado se apareceu documento novo
