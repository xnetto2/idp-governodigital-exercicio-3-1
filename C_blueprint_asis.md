# C_blueprint_asis.md

## Service Blueprint AS-IS — Consulta processual pública no e-Proc/JMU por cidadão interessado

**Escopo modelado:** jornada do **cidadão externo não autenticado**, da percepção da necessidade até a obtenção da informação, a frustração da consulta ou o transbordo para canal humano. Fora do escopo: peticionamento eletrônico, consulta autenticada por perfil profissional e rotinas internas sem manifestação no serviço público de consulta.

**Legenda de evidências (EV):** EV1 página institucional de acesso · EV2 tela pública de consulta · EV3 campos e filtros de pesquisa · EV4 mecanismo visível de validação/segurança · EV5 feedback de submissão · EV6 lista de resultados · EV7 metadados processuais básicos · EV8 espelho de movimentações · EV9 mensagens de restrição/erro/ausência · EV10 página de indisponibilidade/timeout · EV11 indicação de canal de suporte/ouvidoria.

### Tabela do blueprint

| Raia / Linha | **E0 — Pré-sistema** (necessidade e posse do número) | **E1 — Acesso e descoberta** | **E2 — Escolha do ambiente/instância** | **E3 — Preenchimento dos parâmetros** | **E4 — Validação/segurança antiabuso** | **E5 — Submissão e processamento + Gate** | **E6 — Retorno da consulta (ramos)** | **E7 — Interpretação/compreensão** | **E8 — Encerramento ou transbordo** |
|---|---|---|---|---|---|---|---|---|---|
| **Evidências Digitais** | *(sem tela; contexto externo ao sistema)* | EV1, EV2 | EV3 *(seletor de ambiente)* | EV3, EV4 | EV4, EV5 *(componente e erro de validação)* | EV5 *(confirmação de submissão)* | **Encontrado:** EV6, EV7, EV8 · **Não encontrado/Restrito:** EV9 · **Erro/Indisponível:** EV9/EV10 | EV7, EV8 *(metadados e movimentações)* | EV11 *(quando há transbordo)* |
| **Ações do Cidadão** | Percebe a necessidade de consultar; verifica se possui o número do processo em formato adequado | Acessa o portal da JMU; procura a funcionalidade de consulta processual pública | Seleciona o ambiente de consulta: 1º grau (Auditorias) ou 2º grau (STM) | Informa o número do processo no campo principal de busca | Resolve o mecanismo antiabuso (ex.: CAPTCHA) | Envia a consulta e aguarda a resposta | Recebe a resposta do sistema em um dos quatro ramos | Lê e tenta interpretar metadados e movimentações | Obtém e compreende a informação **ou** recorre a suporte/ouvidoria |
| **══ LINHA DE INTERAÇÃO ══** | ─ | ─ | ─ | ─ | ─ | ─ | ─ | ─ | ─ |
| **Frontstage** | *(não há sistema ainda)* | Página institucional; link/menu de consulta pública | Seletor de ambiente/instância na interface | Formulário com campo de número do processo *(busca por nome restrita/indisponível ao não autenticado)* | Componente visível de validação; exibe **erro de validação da requisição** quando aplicável | Indicador de submissão/processamento | Tela de resultado · mensagem de não-encontrado · aviso de restrição/sigilo · página de erro/indisponibilidade | Tela de metadados básicos e espelho de movimentações, em linguagem jurídica | Indicação de canal de suporte/ouvidoria (apenas como saída) |
| **══ LINHA DE VISIBILIDADE ══** | ─ | ─ | ─ | ─ | ─ | ─ | ─ | ─ | ─ |
| **Backstage Síncrono** | — | Entrega de páginas pelo servidor web/aplicação | Roteamento para o ambiente de consulta correspondente (1º/2º grau) | Recepção dos parâmetros de entrada | Verificação antiabuso e validação de parâmetros mínimos da requisição | Consulta à base/indexadores; **GATE DE PUBLICIDADE/SIGILO** — aplica CF art. 93, IX; CNJ 121/2010; LAI; LGPD → decide entre **retorno público permitido** e **retorno restrito/sigilo/publicidade mitigada**; montagem da resposta | Formatação do retorno conforme o ramo; geração das mensagens de não-encontrado, restrição ou erro | Renderização dos metadados e movimentações publicáveis | Registro do encaminhamento ao suporte (quando há transbordo) |
| **══ LINHA DE INTERAÇÃO INTERNA ══** | ─ | ─ | ─ | ─ | ─ | ─ | ─ | ─ | ─ |
| **Backstage Assíncrono** | *(produção anterior do dado — não ocorre durante a consulta)* | — | — | — | — | Dado consultado resulta de autuação, classificação, lançamento de movimentações, metadados e **flags de sigilo/publicidade** feitos previamente pelas secretarias das Auditorias (1º grau) e do STM (2º grau) | Conteúdo/abrangência dos ramos depende diretamente da qualidade dessa alimentação prévia | Inteligibilidade depende de como as movimentações foram lançadas na origem | — |
| **Suporte / Infraestrutura / Governança** | — | Disponibilidade e desempenho do portal (TI/infra); segurança da informação | Idem | Idem | Idem; controles antiabuso mantidos pela segurança | Governança de dados/segurança define os controles de publicidade, minimização e conformidade que **alimentam o gate** | Monitoramento de erros e indisponibilidade (TI/infra) | — | **Ouvidoria/atendimento humano** recebe o transbordo *(nó de saída, não continuação do fluxo)* |
| **⚠ Fail points** | **FP-num** — cidadão não possui o número, ou em formato inadequado → bloqueio antes do sistema | **FP1** — descoberta insuficiente do serviço (não localiza a funcionalidade) | **FP-inst** — escolha incorreta do ambiente/instância | **FP2** — parâmetro inadequado (número incompleto/errado; busca por nome fora do fluxo principal) | **FP3** — barreira de validação/acessibilidade impede concluir mesmo com número correto; falha de validação da requisição ancorada aqui | **FP5** — timeout/degradação de desempenho; dependência do gate | **FP4** — ambiguidade semântica entre não-encontrado / restrito-sigilo / erro | **FP6** — opacidade cognitiva (linguagem jurídica) → compreensão insuficiente | Abandono ou transbordo por insuficiência do canal principal |

> **FP7 — fail point transversal (qualidade do dado de origem):** atravessa E5, E6 e E7. A produção assíncrona do dado pelas secretarias (autuação, classificação, metadados, flags de sigilo) condiciona inteiramente o que o sistema consegue exibir no frontstage; erros de cadastro ou classificação repercutem diretamente no resultado entregue ao cidadão.

### Ramos de saída da etapa E6 (distintos e explícitos)

1. **Resultado encontrado** → segue para E7 (interpretação): exibe EV6 (lista), EV7 (metadados) e EV8 (movimentações).
2. **Processo não encontrado** → EV9 com mensagem de ausência de resultado.
3. **Acesso restrito / sigilo / publicidade mitigada** → EV9; decorre do **gate de publicidade/sigilo** no backstage síncrono, não de falha técnica.
4. **Erro operacional / indisponibilidade / timeout** → EV9/EV10; associado a FP5.

*(O erro de validação da requisição **não** é um ramo de saída: fica ancorado na etapa E4 — validação/segurança.)*

## Observações analíticas

- **Borda esquerda pré-sistema (E0):** a jornada começa antes do portal porque a posse do número do processo é pré-condição crítica do sucesso; modelar esse marco torna visível um bloqueio que ocorre fora da interface.
- **Dualidade da JMU como decisão do usuário (E2):** a arquitetura 1º grau/2º grau não foi escondida no backstage — aparece como ponto de decisão frontstage, com fail point de escolha incorreta do ambiente.
- **Validação como etapa autônoma (E4):** o mecanismo antiabuso recebe passo próprio na linha de interação, com fail point de acessibilidade; o erro de validação fica aqui, separado das saídas finais da consulta, para não contaminar a semântica dos ramos de E6.
- **Gate jurídico no backstage síncrono (E5):** a publicidade/sigilo é modelada como regra de negócio explícita (CF 93,IX; CNJ 121/2010; LAI; LGPD), tornando o ramo "restrito" uma consequência normativa e não aleatória nem meramente técnica.
- **Quatro ramos distintos (E6):** preserva-se a diferença material de experiência entre não-encontrado, restrito, erro e sucesso, evitando a ambiguidade de uma saída genérica (FP4).
- **Etapa de compreensão (E7):** a consulta tecnicamente bem-sucedida não encerra a jornada; a opacidade da linguagem jurídica é fail point cognitivo próprio (FP6) que pode levar a abandono ou transbordo.
- **Duas camadas de backstage + FP7 transversal:** separar o processamento em tempo real (síncrono) da produção anterior do dado (assíncrono) explicita que a confiabilidade do frontstage depende de rotinas institucionais anteriores ao clique.
- **Sustentação e ouvidoria:** TI/infra, governança/segurança e suporte formam uma raia única de sustentação; a ouvidoria figura exclusivamente como nó de saída/transbordo, coerente com o recorte AS-IS do serviço público de consulta.
