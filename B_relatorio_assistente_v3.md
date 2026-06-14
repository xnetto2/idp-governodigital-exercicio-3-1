# Relatório do assistente v3

# B_relatorio_assistente_v3.md

## 1. Delimitação precisa do serviço analisado

O serviço analisado é a **consulta processual pública no e-Proc/JMU por cidadão interessado**, considerada estritamente sob a perspectiva do **usuário externo não autenticado**. Trata-se, portanto, do fluxo de acesso público, via internet, a informações processuais disponibilizadas pela Justiça Militar da União sem login funcional, sem certificado digital e sem credenciais profissionais de advogado, defensor, membro do Ministério Público ou servidor interno.

O escopo deste relatório **não** abrange peticionamento eletrônico, acesso restrito por perfil profissional, consulta autenticada, download integral de peças sob credencial, tramitação interna de gabinetes ou rotinas de secretaria que não se manifestem diretamente no serviço público de consulta. Também não se confunde com o processo judicial em si. O objeto é o **serviço digital de consulta**, isto é, a jornada pela qual um cidadão tenta localizar e compreender informações processuais públicas por meio da interface externa do e-Proc/JMU.

A delimitação institucional inclui as **Auditorias da Justiça Militar da União**, no primeiro grau, e o **Superior Tribunal Militar**, no segundo grau, porque a informação exibida ao cidadão depende da estrutura dual da JMU. Para fins de blueprint AS-IS, essa dualidade é relevante não como detalhe organizacional abstrato, mas como condicionante da navegação, da origem dos dados e da manutenção do acervo processual acessível.

## 2. Finalidade pública, valor e limites do serviço

A finalidade pública do serviço é permitir **acesso público qualificado a informações processuais** cuja divulgação seja juridicamente admissível, oferecendo transparência judicial mínima, rastreabilidade institucional e redução de dependência de canais humanos para dúvidas básicas de acompanhamento.

O valor do serviço está em permitir que o cidadão:

* verifique a existência de determinado processo;
* identifique órgão julgador ou unidade responsável;
* acompanhe movimentações e marcos processuais básicos;
* obtenha noções mínimas sobre o estado procedimental do feito.

Contudo, o valor do serviço é limitado por três ordens de restrição. A primeira é **jurídica**: nem toda informação processual pode ser exposta ao público geral. A segunda é **operacional**: a consulta depende de alimentação correta da base, indexação, disponibilidade e desempenho do sistema. A terceira é **cognitiva**: a informação pode estar tecnicamente visível, mas continuar pouco compreensível ao cidadão leigo se a interface utilizar linguagem excessivamente jurídica ou mensagens ambíguas.

Assim, o serviço é valioso não apenas quando “mostra dados”, mas quando mostra **dados publicáveis, corretos, acessíveis e interpretáveis**.

## 3. Jornada atual do usuário externo não autenticado

Para fins de blueprint AS-IS, a jornada atual pode ser descrita em sequência transacional:

### Etapa 1 — Identificação da necessidade

O cidadão percebe que precisa localizar um processo ou verificar seu andamento. Aqui ainda não há interação com o sistema, mas já existe uma pré-condição crítica: o usuário pode ou não possuir um identificador adequado, como número processual, nome de parte ou outra informação útil.

### Etapa 2 — Acesso ao portal

O usuário entra no portal institucional da Justiça Militar da União e tenta localizar a funcionalidade de consulta processual pública. O primeiro ponto de fricção é a **descoberta do serviço**.

### Etapa 3 — Seleção do ambiente de consulta

O usuário precisa acessar o ambiente correto de consulta. Em termos AS-IS, essa etapa é sensível porque a arquitetura institucional da JMU envolve primeiro e segundo graus. Se a interface exigir segmentação por instância, esse já é um ponto de decisão do usuário; se a busca for unificada, a complexidade é deslocada para o sistema.

### Etapa 4 — Preenchimento dos parâmetros

O usuário informa os dados de pesquisa disponíveis na interface pública. O serviço tende a ser mais eficiente quando o cidadão possui o número processual em formato adequado. Quando a busca depende de nome, dados incompletos ou filtros pouco compreendidos, cresce o risco de insucesso.

### Etapa 5 — Validação e submissão

O usuário envia a consulta. Havendo mecanismo antiabuso, essa etapa inclui validação prévia da requisição. Esse ponto é crítico para acessibilidade e sucesso da jornada.

### Etapa 6 — Processamento pelo sistema

Aqui se inicia o processamento síncrono invisível ao usuário: a interface envia a requisição ao servidor de aplicação, o sistema consulta a base de dados e aplica regras de negócio relativas a publicidade, restrição e retorno de resultados.

### Etapa 7 — Retorno do frontstage

O usuário recebe uma resposta: resultado localizado, ausência de resultado, mensagem de restrição, erro de validação, timeout ou indisponibilidade.

### Etapa 8 — Interpretação do conteúdo

Se a consulta retornar resultado, o cidadão tenta interpretar dados básicos, movimentações e sinais de situação processual.

### Etapa 9 — Encerramento ou transbordo

O usuário encerra a jornada ao obter a informação desejada, ao desistir, ou ao migrar para atendimento humano, ouvidoria ou outro canal por insuficiência do serviço.

## 4. Etapas visíveis ao usuário (frontstage)

No frontstage, o serviço se materializa em elementos concretos:

1. **Página institucional de entrada**
2. **Tela de consulta processual pública**
3. **Campos de busca e filtros visíveis**
4. **Mecanismo de validação/segurança da consulta**
5. **Botão de envio ou execução da pesquisa**
6. **Tela de resultados ou ausência de resultados**
7. **Tela de detalhes processuais básicos**
8. **Espelho de movimentações ou andamentos**
9. **Mensagens de erro, restrição ou indisponibilidade**
10. **Indicação de suporte institucional quando aplicável**

Esses componentes frontstage não são neutros. Eles traduzem escolhas de design, regras jurídicas e limites técnicos. Em especial, a semântica das mensagens é decisiva: para o cidadão, é muito diferente receber “processo não encontrado”, “acesso restrito” ou “serviço temporariamente indisponível”. Quando a interface não distingue essas hipóteses, ela degrada a experiência e aumenta o transbordo para canais humanos.

Também é frontstage a inteligibilidade do vocabulário exibido. Expressões como “movimentação”, “conclusão”, “juntada”, “distribuição” ou “classe processual” podem ser juridicamente corretas, mas pouco úteis sem contextualização mínima.

## 5. Processos de bastidor, suporte e infraestrutura

Para o blueprint AS-IS, é essencial separar o que ocorre **sincronamente durante a consulta** do que ocorre **assíncronamente na formação do dado processual**.

### 5.1 Bastidor síncrono da consulta

No momento em que o usuário envia a pesquisa:

* a interface encaminha a requisição ao servidor de aplicação;
* o sistema valida parâmetros mínimos de entrada;
* a aplicação consulta a base de dados/indexadores;
* regras de negócio verificam restrições de publicidade e retorno admissível;
* o resultado é formatado para a interface pública.

Esse é o backstage diretamente relacionado à jornada da consulta.

### 5.2 Bastidor assíncrono de produção do dado

Em momento anterior à consulta:

* unidades judiciárias autuam, classificam e movimentam processos;
* servidores lançam metadados;
* atos processuais são registrados;
* flags de sigilo, categorias de publicidade e parâmetros institucionais influenciam o que poderá ser exibido depois ao público.

Esse fluxo não acontece “durante o clique” do cidadão, mas condiciona inteiramente a qualidade do resultado exibido.

### 5.3 Suporte e infraestrutura

Há ainda uma terceira camada:

* equipe de tecnologia da informação;
* administração de banco de dados;
* segurança da informação;
* governança de dados;
* atendimento institucional, suporte e ouvidoria.

Essas funções sustentam disponibilidade, desempenho, conformidade, tratamento de incidentes e absorção de falhas do canal público.

## 6. Atores envolvidos, funções e responsabilidades

### Atores humanos e institucionais

* **Cidadão não autenticado**: usuário do serviço público de consulta.
* **Servidor da secretaria da Auditoria Militar**: registra e mantém dados processuais do primeiro grau.
* **Servidor da secretaria judiciária do STM**: mantém dados processuais do segundo grau.
* **Magistrado/órgão julgador**: produz atos que influenciam o conteúdo registrável e visível.
* **Função de distribuição e gestão processual**: impacta autuação, classificação e consistência dos metadados.
* **Equipe de TI**: mantém portal, aplicação, base, desempenho e integrações.
* **Área de governança de dados e segurança**: define controles de publicidade, minimização e conformidade.
* **Ouvidoria/suporte institucional**: absorve falhas, dúvidas e transbordos do serviço.

### Sistemas e infraestrutura

* **e-Proc/JMU**: plataforma sociotécnica que integra interface pública, regras de exibição, base de dados e lógica de consulta.
* **Banco de dados/indexação**: camada de processamento e recuperação da informação.
* **Mecanismos de validação de segurança**: controle antiabuso e proteção da interface pública.

O sistema não é “ator” em sentido humano; ele é a infraestrutura técnica por meio da qual os atores institucionais viabilizam o serviço.

## 7. Evidências digitais e pontos de contato do serviço

Para viabilizar transposição ao blueprint, as evidências podem ser nomeadas assim:

* **EV1** — Página institucional de acesso
* **EV2** — Tela pública de consulta
* **EV3** — Campos e filtros de pesquisa
* **EV4** — Mecanismo visível de validação/segurança
* **EV5** — Feedback de submissão da consulta
* **EV6** — Lista de resultados
* **EV7** — Tela de metadados processuais básicos
* **EV8** — Espelho de movimentações processuais
* **EV9** — Mensagens de restrição, erro ou ausência de resultado
* **EV10** — Página de indisponibilidade ou timeout
* **EV11** — Indicação de canal de suporte ou atendimento institucional

Essas evidências delimitam os pontos de contato do cidadão com o serviço e ajudam a definir a linha de interação, a linha de visibilidade e os pontos de falha do AS-IS.

## 8. Normativos, regras de negócio e condicionantes institucionais

O serviço é condicionado por normas que operam como **regras de negócio institucionais e tecnológicas**.

O **art. 93, IX, da Constituição Federal** consagra a publicidade dos julgamentos, mas admite restrições legais. Isso se traduz, no serviço, na necessidade de não tratar a publicidade como absoluta.

A **Resolução CNJ nº 121/2010** é central porque disciplina a divulgação de dados processuais eletrônicos na internet. Em termos operacionais, ela sustenta a distinção entre o que pode ser exibido ao público geral como metadado processual e o que não deve ser disponibilizado sem controle adicional. Para o AS-IS, isso significa que a interface pública tende a privilegiar dados básicos e andamentos sumários, e não necessariamente a integralidade das peças.

A **Lei nº 12.527/2011 (LAI)** reforça a transparência, mas a **Lei nº 13.709/2018 (LGPD)** impõe limites ao tratamento e à exposição de dados pessoais. No serviço, essa tensão aparece na necessidade de exibir informação suficiente para a função pública da consulta sem transformar a plataforma em vetor de exposição indevida de dados.

Já o **eMAG** e os referenciais de acessibilidade digital são relevantes porque a barreira de entrada do serviço não pode excluir usuários por deficiência visual, cognitiva ou limitações de navegação assistiva.

## 9. Gargalos, fail points e riscos operacionais do AS-IS

### FP1 — Descoberta insuficiente do serviço

O cidadão pode não localizar com facilidade a funcionalidade correta no portal.

### FP2 — Insucesso por parâmetros inadequados

A busca depende de informação suficientemente precisa. Erro de sintaxe, dado incompleto, homônimo ou filtro mal compreendido podem gerar falso negativo ou resultado pouco útil.

### FP3 — Barreira de validação de segurança

Se o mecanismo antiabuso for pouco acessível ou pouco compreensível, o usuário pode ser bloqueado antes mesmo de consultar.

### FP4 — Ambiguidade semântica da resposta

Mensagens que não distinguem claramente inexistência do processo, restrição de acesso e erro operacional comprometem a utilidade do serviço.

### FP5 — Timeout ou degradação de desempenho

Consultas mais pesadas, picos de uso ou sobrecarga de infraestrutura podem levar a lentidão e falhas de resposta.

### FP6 — Opacidade cognitiva

Mesmo quando a busca funciona, a linguagem técnica pode impedir a compreensão material do resultado.

### FP7 — Dependência da qualidade do dado de origem

O serviço público de consulta reflete a qualidade da alimentação processual anterior. Erros de cadastro ou classificação repercutem diretamente no frontstage.

## 10. Síntese final para construção do Service Blueprint AS-IS

Esta versão v3 consolida o serviço como uma jornada de consulta pública **cronologicamente observável**, com separação mais nítida entre:

* ação do usuário;
* resposta visível da interface;
* processamento automático em tempo real;
* processos institucionais anteriores que condicionam o dado exibido;
* suporte e infraestrutura.

Para o blueprint AS-IS, a linha central não deve ser “o processo judicial em geral”, mas a sequência pela qual o cidadão não autenticado:

1. percebe a necessidade;
2. acessa o portal;
3. localiza a consulta;
4. insere parâmetros;
5. submete a requisição;
6. recebe resposta;
7. interpreta ou não a informação;
8. resolve a demanda ou transborda para atendimento humano.

O valor analítico desta versão está em mostrar que a consulta processual pública no e-Proc/JMU é um serviço digital juridicamente condicionado, tecnologicamente dependente e cognitivamente exigente. O blueprint AS-IS deverá representar, com linhas de visibilidade claras, como a publicidade processual, a infraestrutura técnica, a alimentação dos dados, a acessibilidade e as regras de negócio se combinam para produzir a experiência efetiva — ou a frustração — do cidadão que busca informação pública judicial.

