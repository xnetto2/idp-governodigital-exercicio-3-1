# Relatório de auditoria v2
# B_relatorio_auditoria_v2.md

## 1. Avaliação geral da evolução entre v1 e v2

A versão v2 apresenta um avanço real em relação à v1, corrigindo o erro fatal de escopo ao definir o usuário como "cidadão externo não autenticado". A distinção entre atores humanos e infraestrutura tecnológica (e-Proc não é mais um "ator") também foi sanada, e as falhas operacionais descritas (como o falso negativo por segredo de justiça) deixaram de ser meros "erros de sistema" genéricos.

Contudo, o texto ainda não está apto a ser transposto diretamente para um *Service Blueprint AS-IS*. A v2 falha ao manter uma abordagem discursiva e ensaística sobre o serviço, em vez de tratá-lo como um fluxo temporal estrito de engenharia de serviços. O relatório descreve bem as "raias" (linhas horizontais do blueprint: frontstage, backstage, etc.), mas continua ignorando a "cronologia" (colunas verticais: passo a passo da interação). Além disso, as normas foram citadas, mas continuam operando como pano de fundo teórico, sem conexão determinística com os artefatos da interface.

## 2. Problemas remanescentes identificados

* **Conexão Normativa Genérica:** O item 8 cita a Resolução CNJ nº 121/2010 e a LGPD, mas de forma abstrata ("disciplina a divulgação", "impõe limites"). Num blueprint, a norma é uma regra de negócio parametrizada em código. A v2 omite **como** o CNJ 121/2010 opera na prática: bloqueando o download de peças processuais em PDF (íntegra dos autos) e permitindo **apenas** a visualização de metadados (classe, assunto, partes, advogados) e do andamento (eventos). A LGPD, por sua vez, reflete-se na oclusão ou abreviação de nomes em processos sensíveis, o que não foi mapeado nas evidências.
* **Omissão Operacional da Separação de Instâncias:** O relatório fala em Auditorias (1º grau) e STM (2º grau), mas não esclarece um ponto de fricção gravíssimo do e-Proc: a busca pública exige que o usuário selecione previamente a instância desejada em portais/abas diferentes, ou trata-se de um barramento de busca unificada? Essa omissão afeta diretamente o passo 1 da jornada (EV1/EV2).
* **Evidências Digitais Insuficientemente Especificadas:** A lista de evidências (EV1 a EV10) melhorou, mas "EV4 — Mecanismo de validação de segurança" é insuficiente. É um reCAPTCHA v2 (seleção de imagens)? Um hCaptcha? Um CAPTCHA sonoro? A especificação exata é crucial porque dita o nível real de barreira de acessibilidade (eMAG). Além disso, a "EV3 - Campos de busca" não lista quais são os parâmetros efetivos (ex: Número CNJ, Nome, CPF/CNPJ, OAB), mascarando pontos de erro de digitação.
* **Gargalos de *Timeout* e Sincronização Ignorados:** O relatório foca na "busca mal sucedida por sintaxe", mas ignora o gargalo de infraestrutura de banco de dados: consultas abertas (como buscar por um nome comum sem filtros adicionais de data ou classe) frequentemente geram estouramento de tempo de resposta (*timeout*) no SGBD do e-Proc, resultando na "EV9", não por indisponibilidade total, mas por sobrecarga de processamento.

## 3. Fragilidades metodológicas ainda presentes

* **Confusão de Linha do Tempo no Backstage:** Há um erro metodológico grave no item 5 ("Processos de bastidor"). O relatório mistura a **alimentação processual** (que é um processo humano assíncrono e pretérito) com o **processo de suporte tecnológico em tempo real**. No momento em que o cidadão clica em "Consultar" (frontstage), o servidor da Auditoria não está "registrando metadados" (backstage). O que ocorre no backstage *naquele milissegundo* é a requisição HTTP da interface para o servidor de aplicação, e a consulta (Query SQL) ao banco de dados relacional. O blueprint AS-IS mapeia a jornada *síncrona* do usuário, não o histórico de formação do processo judiciário.
* **Ausência de Granularidade das Colunas (Passos):** O item 4 ("Etapas visíveis") lista 6 etapas lógicas, mas elas não oferecem o nível de detalhe transacional necessário. O passo 3 ("Validação da requisição") agrupa a ação do usuário (resolver o captcha), o frontstage (enviar formulário) e o suporte (validar token via API) no mesmo balaio conceptual, quebrando a *Linha de Visibilidade* e a *Linha de Interação Interna*.

## 4. Recomendações específicas para a versão v3

Para a elaboração do `B_relatorio_assistente_v3.md`, execute as seguintes correções táticas:

1. **Estruture a Jornada em Colunas Cronológicas Transacionais:** Substitua os textos corridos de etapas por um fluxo mapeado passo a passo, separando exatamente: Ação do Cidadão -> Reação da Interface (Frontstage) -> Processamento no Sistema (Processo de Suporte Automático). Exclua a "alimentação processual pelos servidores" do fluxo em tempo real da consulta.
2. **Especifique os Filtros e as Máscaras (EV3 e EV4):** Liste expressamente quais campos de texto estão disponíveis na tela de busca da JMU (ex: Máscara obrigatória para NPU 20 dígitos) e defina qual é a tecnologia exata de CAPTCHA empregada.
3. **Vincule Normas a Componentes da Interface:** No item de normativos, declare taxativamente: *"Por força do Art. X da Res. CNJ 121, a EV7 (Espelho de movimentação) exibe apenas a descrição sumária do andamento processual, suprimindo os hiperlinks para download da íntegra das petições e sentenças, que estão condicionados à autenticação."*
4. **Mapeie o Ponto de Decisão Algorítmica (Segredo de Justiça):** Descreva explicitamente a etapa de backstage onde o sistema operacionaliza a falha 3: "SGBD localiza o processo -> Sistema identifica a flag `Segredo de Justiça = TRUE` -> Regra de negócio bloqueia o retorno de metadados -> Frontstage exibe EV8 (Processo não encontrado / Acesso Restrito)."
5. **Esclareça a Arquitetura de Instâncias:** Defina logo na delimitação se a busca mapeada ocorre de forma unificada ou segmentada entre 1º Grau (Auditorias) e 2º Grau (STM), e como isso afeta a navegação do cidadão.
