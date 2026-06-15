# Grill transcript

# C_grill_transcript.md

## Objetivo deste documento

Registro estruturado da sessão de *grill-me* (perguntas críticas, uma a uma) conduzida para fixar as premissas do **Service Blueprint AS-IS** do serviço **consulta processual pública no e-Proc/JMU por cidadão interessado**.

Cada rodada registra: **a pergunta crítica feita**, **a resposta do responsável pela modelagem** e **a decisão extraída** que será insumo direto para a construção posterior do `C_blueprint_asis.md`.

> Observação metodológica: uma rodada preliminar anterior foi descartada por não seguir o protocolo de uma pergunta por vez. O transcript válido começa na Rodada 1 abaixo.

---

## Rodada 1 — Recorte e escopo do serviço

**Pergunta crítica.** O blueprint deve representar apenas a jornada do cidadão externo **não autenticado**, tratando qualquer transbordo para canais autenticados/humanos como saída do escopo?

**Resposta.** Sim. O recorte é exatamente a jornada do cidadão externo não autenticado na consulta processual pública do e-Proc/JMU, da percepção da necessidade até a obtenção da informação, a frustração da consulta ou o transbordo. Peticionamento eletrônico, consulta autenticada por perfil profissional e rotinas internas que não se manifestem no serviço público de consulta ficam fora do escopo principal. Transbordo para canal humano/ouvidoria pode aparecer apenas como **saída da jornada** ou consequência de fail point, nunca como continuação do serviço principal.

**Decisão extraída.** Escopo do blueprint = jornada do cidadão **não autenticado**. Fora do escopo: peticionamento, consulta autenticada, rotinas internas sem manifestação no serviço público. Transbordo = nó de saída, não continuação.

---

## Rodada 2 — Arquitetura dual da JMU e escolha de instância

**Pergunta crítica.** O cidadão escolhe a instância (1º grau / STM) na própria interface antes de buscar, ou o blueprint assume consulta unificada com o sistema decidindo a origem do dado?

**Resposta.** Assumir que o cidadão **precisa escolher o ambiente/instância de consulta antes da busca**, e que essa escolha aparece no **frontstage** como ponto de decisão no início da jornada. A arquitetura dual (1º grau / Auditorias e 2º grau / STM) não fica invisível: impacta a navegação pública e cria possível fail point de escolha incorreta do ambiente. Sequência: acessa portal → localiza consulta pública → escolhe ambiente → preenche parâmetros → envia.

**Decisão extraída.** Escolha de instância é **etapa frontstage** (ponto de decisão inicial), com fail point de escolha incorreta do ambiente. A dualidade da JMU é parcialmente visível ao usuário, não resolvida só internamente.

---

## Rodada 3 — Parâmetro principal de busca

**Pergunta crítica.** A consulta pública permite busca por nome de parte, ou o AS-IS assume que a interface exige essencialmente o número do processo (busca por nome restrita/indisponível ao não autenticado)?

**Resposta.** Assumir que a consulta pública não autenticada **exige essencialmente o número do processo** como parâmetro principal. Busca por nome de parte = restrita, indisponível ou não central, para evitar ambiguidade (homônimos, dado incompleto, exposição de dados pessoais). Campo principal = número do processo em formato adequado; dependência desse número = ponto crítico; ausência do número correto = fail point relevante; busca por nome não entra como fluxo principal.

**Decisão extraída.** Parâmetro principal = **número do processo**. Busca por nome fica fora do fluxo principal. Ausência/inadequação do número = fail point crítico da jornada.

---

## Rodada 4 — Mecanismo antiabuso / validação de segurança

**Pergunta crítica.** O mecanismo antiabuso/validação aparece como etapa frontstage explícita (com fail point de acessibilidade), ou como detalhe técnico de backstage sem passo próprio?

**Resposta.** Deve aparecer como **etapa frontstage explícita e própria**, entre o preenchimento dos parâmetros e o envio efetivo da consulta, porque o cidadão interage diretamente com a barreira antes do processamento. Carrega um **fail point específico de acessibilidade e usabilidade**: se a validação falhar, o usuário pode ficar impedido de concluir mesmo tendo o número correto. Barreira explícita, não mecanismo invisível.

**Decisão extraída.** Validação antiabuso = **etapa frontstage autônoma** entre preenchimento e envio, com fail point próprio de acessibilidade/usabilidade.

---

## Rodada 5 — Ramos de saída após a consulta

**Pergunta crítica.** O blueprint representa os retornos como ramos distintos e explícitos após a consulta, ou consolida em menos saídas?

**Resposta.** Representar como **ramos distintos e explícitos**, com pelo menos quatro saídas: (1) resultado encontrado; (2) processo não encontrado; (3) acesso restrito / sigilo / publicidade mitigada; (4) erro operacional / indisponibilidade / timeout. O **erro de validação da requisição** fica associado à etapa anterior de validação/segurança, não às saídas finais. Justificativa: experiências materialmente diferentes, causas distintas no frontstage e backstage — preferível diagrama mais detalhado, porém semanticamente correto.

**Decisão extraída.** Quatro ramos de saída pós-consulta: **encontrado / não encontrado / restrito-sigilo / erro-indisponibilidade**. Erro de validação ancorado na etapa de segurança, não nas saídas finais.

---

## Rodada 6 — Etapa de interpretação do resultado (fail point cognitivo)

**Pergunta crítica.** A jornada bem-sucedida termina na exibição do dado, ou há etapa explícita de interpretação/compreensão com fail point cognitivo?

**Resposta.** Incluir uma **etapa explícita de interpretação/compreensão do resultado** após a exibição em tela. A jornada bem-sucedida só se encerra quando o cidadão compreende minimamente o que foi exibido. A **opacidade da linguagem jurídica** é fail point cognitivo próprio, mesmo no ramo "encontrado". Modelar: (1) exibição do resultado; (2) interpretação/compreensão; (3) desdobramento por compreensão insuficiente (abandono, nova tentativa de interpretação ou transbordo para suporte/ouvidoria).

**Decisão extraída.** Ramo "encontrado" tem etapa pós-exibição de **interpretação/compreensão**, com fail point cognitivo (opacidade jurídica) que pode gerar abandono, retentativa ou transbordo mesmo após consulta tecnicamente bem-sucedida.

---

## Rodada 7 — Camadas de backstage (síncrono e assíncrono)

**Pergunta crítica.** O blueprint representa as duas camadas de backstage — síncrona (processamento em tempo real) e assíncrona (alimentação anterior do dado) — como faixas distintas, com FP7 ligando a assíncrona ao frontstage? Ou mantém só o síncrono e trata a alimentação como pré-condição externa?

**Resposta.** Representar **as duas camadas como faixas distintas**: (1) backstage síncrono = recepção da requisição, validação, consulta à base, aplicação de regras de publicidade/restrição, montagem da resposta; (2) backstage assíncrono = autuação, classificação, lançamento de movimentações, metadados e flags de sigilo/publicidade pelas unidades judiciárias. A **dependência da qualidade do dado de origem (FP7)** é fail point **transversal**, conectando a camada assíncrona ao frontstage — não tratada como nota externa.

**Decisão extraída.** Duas faixas de backstage (**síncrono** e **assíncrono**) explícitas. FP7 = fail point **transversal** ligando produção do dado ao que o cidadão vê.

---

## Rodada 8 — Borda esquerda da jornada (etapa pré-sistema)

**Pergunta crítica.** O blueprint começa com etapa pré-sistema ("identificação da necessidade / posse do número") dentro do diagrama, ou só no momento em que o cidadão acessa o portal?

**Resposta.** Começar com uma **etapa pré-sistema explícita**, antes da interação com o portal, **dentro do diagrama**: "identificação da necessidade / posse do número do processo", com fail point de **não ter o número correto**. A etapa seguinte é a entrada no portal e a descoberta da funcionalidade de consulta, onde fica o fail point de **descoberta insuficiente do serviço (FP1)**. Razão: antes de acessar o portal, o cidadão já pode estar bloqueado pela falta do identificador.

**Decisão extraída.** Borda esquerda = **etapa pré-sistema** dentro do diagrama (necessidade + posse do número, com fail point de ausência do número), seguida de entrada no portal + descoberta (FP1).

---

## Rodada 9 — Gate jurídico de publicidade/sigilo como regra de negócio

**Pergunta crítica.** A decisão de publicidade/sigilo é um "gate" de regra de negócio explícito no backstage síncrono (com as normas anotadas), ou as normas ficam só como contexto regulatório à parte?

**Resposta.** Representar como **gate explícito de regra de negócio no backstage síncrono**: ponto de decisão automática do sistema que bifurca entre retorno público permitido e retorno restrito/sigilo/publicidade mitigada. As normas são anotadas ali como fundamento: art. 93, IX, CF; Resolução CNJ nº 121/2010; LAI; LGPD. Assim o ramo "acesso restrito/sigilo" decorre da aplicação de regras jurídicas operacionalizadas, não de aleatoriedade nem só de falha técnica.

**Decisão extraída.** **Gate de publicidade/sigilo** no backstage síncrono, com normas anotadas (CF 93,IX; CNJ 121/2010; LAI; LGPD). É a origem do ramo "restrito".

---

## Rodada 10 — Faixa de evidências digitais

**Pergunta crítica.** O blueprint inclui faixa de evidências digitais mapeada ponto a ponto (EV1–EV11) contra cada etapa, ou tratamento mais enxuto só nos momentos-chave?

**Resposta.** Incluir **faixa explícita de evidências digitais no topo**, em solução intermediária: cada coluna da jornada registra as evidências relevantes daquele momento (com IDs), consolidadas por etapa, sem coluna separada por EV. Mapeamento de referência:
- entrada/descoberta → EV1, EV2;
- escolha do ambiente e preenchimento → EV3, EV4;
- validação/segurança → EV5;
- resultado encontrado → EV6, EV7, EV8;
- restrição/erro/indisponibilidade → EV9 ou EV10;
- transbordo → EV11.

**Decisão extraída.** **Faixa de evidências no topo**, consolidada por etapa com IDs EV1–EV11 (sem fragmentar). Mapeamento de referência registrado acima.

---

## Rodada 11 — Raias de sustentação e posição da ouvidoria

**Pergunta crítica.** Os atores de sustentação (TI/infra, governança/segurança, suporte) entram como raia adicional explícita? Faixa única ou fora das raias, com suporte/ouvidoria só como nó de saída?

**Resposta.** Criar **uma raia única adicional de suporte/infraestrutura/governança** (TI/infra + governança de dados/segurança + suporte institucional), representando a sustentação estrutural — sem substituir o backstage síncrono nem o assíncrono. A **ouvidoria/suporte humano** não aparece como continuação normal da jornada: surge **apenas como nó de saída/transbordo** quando houver falha, compreensão insuficiente ou impossibilidade de resolução no canal principal.

**Decisão extraída.** **Raia única de suporte/infraestrutura/governança** como camada de sustentação. Ouvidoria/suporte humano = **nó de saída/transbordo**, não etapa ordinária.

---

## Síntese das decisões para o `C_blueprint_asis.md`

Estrutura consolidada a ser representada no blueprint AS-IS:

**Raias (de cima para baixo):**
1. Evidências físicas/digitais (faixa de topo, consolidada por etapa, EV1–EV11);
2. Ação do cidadão (linha de interação);
3. Frontstage do sistema (acima da linha de visibilidade);
4. Backstage síncrono (processamento em tempo real, inclui o gate de publicidade/sigilo);
5. Backstage assíncrono (produção/alimentação anterior do dado);
6. Suporte / infraestrutura / governança (raia única de sustentação).

**Sequência da jornada (linha de interação):**
1. Pré-sistema: identificação da necessidade + posse do número do processo *(fail point: não ter o número)*;
2. Acesso ao portal + descoberta da consulta *(FP1: descoberta insuficiente)*;
3. Escolha do ambiente/instância — 1º grau / STM *(fail point: escolha incorreta)*;
4. Preenchimento de parâmetros — número do processo como campo principal;
5. Validação/segurança antiabuso *(fail point de acessibilidade/usabilidade; erro de validação aqui)*;
6. Submissão da consulta;
7. Processamento (backstage síncrono) + **gate de publicidade/sigilo**;
8. Quatro ramos de saída: **(a) encontrado**, (b) não encontrado, (c) restrito/sigilo, (d) erro/indisponibilidade/timeout;
9. No ramo encontrado: exibição → interpretação/compreensão *(fail point cognitivo)*;
10. Encerramento (sucesso compreendido) **ou** transbordo para ouvidoria/suporte (nó de saída).

**Fail points-chave:** ausência do número; FP1 descoberta; escolha de instância; validação/acessibilidade; ambiguidade semântica das saídas; timeout/desempenho (FP5); opacidade cognitiva (FP6); **FP7 transversal** (qualidade do dado de origem, ligando backstage assíncrono ao frontstage).

**Fundamento normativo anotado no gate:** art. 93, IX, CF; Resolução CNJ nº 121/2010; LAI (Lei 12.527/2011); LGPD (Lei 13.709/2018); referência transversal de acessibilidade (eMAG).
