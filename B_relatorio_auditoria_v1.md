# Relatório de auditoria v1
## 1. Avaliação geral
O relatório v1 apresenta uma descrição essencialmente conceitual e abstrata do serviço de Consulta Processual Pública no e-Proc/JMU. Embora demonstre uma compreensão macro sobre os objetivos de transparência e os desafios de usabilidade da ferramenta, o documento falha gravemente em fornecer a densidade técnica e operacional necessária para subsidiar a construção de um Service Blueprint AS-IS.

O principal defeito do relatório está na falta de delimitação precisa do escopo. Ao tentar abraçar uma massa homogênea de usuários (misturando o cidadão leigo, o advogado e as partes), o texto dilui as especificidades do canal digital público sem autenticação. Um Blueprint exige o mapeamento de um fluxo determinístico; ao não definir se o usuário opera de forma anônima ou logada, todo o detalhamento de frontstage e backstage torna-se impreciso e inutilizável para fins de modelagem de processos ou governança digital.

## 2. Problemas substantivos identificados
Ausência Absoluta de Ancoragem Normativa: O item 8 ("Normativos e limites institucionais") opera como um mero indicador de intenções, sem citar uma única norma concreta. Para que este relatório tenha validade na disciplina de Governo Digital, ele deve obrigatoriamente integrar os impactos da Resolução CNJ nº 121/2010 (que dita quais dados são de exibição pública geral e quais exigem senha) e da Lei Geral de Proteção de Dados - LGPD (Lei nº 13.709/2018), que impõe restrições severas à raspagem de dados e à visualização de nomes de réus ou testemunhas em crimes militares sensíveis.

Tratamento de Tecnologia como Ator Humano: No item 6, o "Sistema e-Proc/JMU" é listado erroneamente como um ator ao lado de servidores e magistrados. Sistemas não são atores em um ecossistema de serviços; eles são a infraestrutura sociotécnica e os canais que viabilizam a interação. O relatório falha ao não discriminar as equipes humanas por trás do sistema, como a Diretoria de Tecnologia da Informação (DTI) do STM e os Técnicos/Analistas Judiciários das Secretarias de Auditoria Militar.

Abstração Genérica de Pontos de Falha (Fail Points): Dizer que há "risco de indisponibilidade" ou "resultados pouco explicativos" (item 9) é um diagnóstico superficial. O relatório omite gargalos técnicos e operacionais crônicos do e-Proc/JMU, tais como:

Falso Negativo por Segredo de Justiça: Quando o cidadão busca um processo criminal militar sob sigilo, o sistema retorna a mensagem padrão "Processo não encontrado" em vez de uma mensagem de restrição de acesso. Isso gera um transbordo de demanda (gargalo) para os canais de atendimento humano (Ouvidoria ou Balcão Virtual).

Erros de Sintaxe Documental: A ausência de uma máscara rígida automatizada para o padrão de numeração unificada do CNJ (NNNNNNN-DD.AAAA.J.TR.OOOO), permitindo que o usuário digite pontos e traços incorretos, quebrando a consulta no banco de dados SQL.

Insuficiência das Evidências Digitais: O item 7 repete termos amplos como "página institucional" e "lista de resultados". Falta especificar os artefatos concretos gerados pelo serviço atual: a chave CAPTCHA de segurança (fator de fricção de acessibilidade), o espelho de movimentação processual em HTML e a certidão de andamento exportável em PDF assinada digitalmente pelo e-Proc.

## 3. Fragilidades metodológicas
Confusão entre Interface de Usuário (UI) e Processo de Serviço: O texto confunde a jornada de navegação em telas com a prestação do serviço público descentralizado da JMU. O relatório descreve o ato de clicar e ler telas, mas ignora o processo de sincronização e indexação de dados que ocorre entre as Auditorias Militares de Primeira Instância e os servidores centrais do Superior Tribunal Militar localizados em Brasília.

Quebra da Lógica de Linha de Visibilidade: No item 5 ("Processos de bastidor"), o relatório não separa o que é suporte automatizado em tempo real (consultas indexadas a bancos de dados através de APIs) do que é rotina humana assíncrona (a autuação de uma petição inicial ou a juntada de uma decisão por um escrevente militar). Sem essa separação, o documento inviabiliza a divisão correta das raias (swimlanes) do Blueprint.

Falta de Diferenciação de Instâncias: A Justiça Militar da União opera em uma estrutura dual clara (Auditorias de 1ª Instância espalhadas pelas Circunscrições Judiciárias Militares e o STM como corte de 2ª Instância). O relatório trata o ecossistema como um bloco monolítico, omitindo que o banco de dados do e-Proc lida com tabelas processuais distintas para cada grau de jurisdição.

## 4. Recomendações específicas para a versão v2
Para a construção do B_relatorio_assistente_v2.md, execute rigorosamente as seguintes alterações:

Restrição de Escopo: Fixe o usuário estritamente como "Cidadão comum, não autenticado (sem login Gov.br e sem certificado digital de advogado)". Remova qualquer menção a advogados ou membros do Ministério Público Militar, cujas jornadas pertencem a outro tipo de serviço (peticionamento e consulta restrita).

Mapeamento de Regras de Negócio Jurídicas: Insira no texto como a regra da publicidade mitigada (Art. 93, IX da CF/88) se traduz em código no e-Proc: o sistema impede a visualização de documentos internos (despachos, decisões, sentenças na íntegra) para usuários anônimos, exibindo apenas as linhas de texto descritivo dos andamentos.

Refinamento da Taxonomia de Atores e Sistemas: Separe explicitamente os atores humanos do backstage por suas funções institucionais na JMU (ex: Distribuidor da Auditoria Militar, Servidor da Secretaria Judiciária do STM) e isole o e-Proc como a plataforma tecnológica dividida em banco de dados e portal web.

Detalhamento Técnico de Três Falhas Operacionais: Inclua um tópico detalhado para cada uma destas três falhas reais na v2:

Falha 1: Erro de validação de segurança (CAPTCHA ilegível inviabilizando o acesso de leitores de tela para cidadãos com deficiência visual - descumprimento do eMAG/WCAG).

Falha 2: Time-out de requisição ao banco de dados em horários de pico de julgamento no STM.

Falha 3: Erro na indexação de termos de busca fonética por nome de parte quando há homônimos ou grafias errôneas inseridas na distribuição original do processo.

Criação de IDs de Evidência: Nomeie os artefatos tangíveis de forma sistemática para facilitar a transposição para o modelo visual
