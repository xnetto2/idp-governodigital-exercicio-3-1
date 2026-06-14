# Relatório do assistente v2
# B_relatorio_assistente_v2.md

## 1. Delimitação do serviço analisado

O serviço analisado é a **consulta processual pública no e-Proc/JMU por cidadão interessado**, considerando especificamente o **usuário externo não autenticado**, isto é, a pessoa que acessa o portal público da Justiça Militar da União sem login Gov.br, sem certificado digital e sem credenciais profissionais de advogado, membro do Ministério Público ou servidor interno. O foco, portanto, não está no peticionamento eletrônico, na consulta restrita por perfis profissionais nem em fluxos internos de gabinete ou secretaria, mas na experiência pública de busca e leitura de informações processuais disponibilizadas na internet.

O órgão responsável é a **Justiça Militar da União**, em estrutura que envolve as **Auditorias da Justiça Militar da União** em primeiro grau e o **Superior Tribunal Militar** em segundo grau. O canal principal de atendimento é o **portal eletrônico de consulta processual pública**, sustentado pela infraestrutura do e-Proc/JMU.

Essa delimitação é necessária porque um blueprint AS-IS exige um fluxo observável e coerente. Se o relatório misturar cidadão anônimo, advogado autenticado, servidor de secretaria e magistrado, a jornada deixa de representar um serviço específico e passa a retratar, de modo confuso, múltiplos serviços distintos.

## 2. Finalidade pública e valor do serviço

A consulta processual pública cumpre função de **transparência judicial, acesso à informação, acompanhamento processual e controle social mínimo sobre a atividade jurisdicional**. Seu valor público não se resume a “mostrar dados”: ele está em permitir que o cidadão acompanhe a existência, a tramitação e parte do histórico de processos cuja publicidade é juridicamente admissível, reduzindo dependência de atendimento humano para dúvidas básicas.

Esse valor, contudo, é condicionado por limites legais e operacionais. O serviço deve equilibrar, de um lado, o princípio da publicidade dos atos processuais e, de outro, a necessidade de resguardar sigilo legal, intimidade, dados pessoais e segurança institucional. Assim, sua utilidade pública depende não apenas da existência do portal, mas da capacidade institucional de apresentar dados corretos, compreensíveis, atualizados e juridicamente filtrados.

Além disso, a consulta pública funciona como mecanismo de diminuição de assimetria informacional. Para o cidadão leigo, saber se um processo existe, em que fase está, qual órgão o conduz e quais movimentações básicas ocorreram já representa ganho concreto de inteligibilidade institucional, ainda que ele não tenha acesso à integralidade dos autos.

## 3. Jornada atual do usuário externo

A jornada começa quando o cidadão percebe a necessidade de localizar um processo ou compreender seu andamento. Essa necessidade pode surgir por interesse pessoal, acompanhamento de caso conhecido, curiosidade legítima, pesquisa acadêmica, atividade jornalística ou necessidade de verificar a existência de processo envolvendo determinada pessoa ou tema.

O usuário então acessa o portal institucional da Justiça Militar da União e tenta localizar a funcionalidade correta de consulta processual pública. O primeiro ponto de fricção é a **descoberta do serviço**: a navegação pode exigir conhecimento prévio da arquitetura do portal ou familiaridade com a terminologia judiciária.

Após localizar a área de consulta, o cidadão precisa informar parâmetros de busca. Na prática, a consulta tende a ser mais eficiente quando o usuário possui o número do processo. Quando isso não ocorre, a experiência torna-se mais incerta, pois nomes de partes, filtros e dados incompletos podem não produzir resultado satisfatório ou podem gerar múltiplos resultados difíceis de interpretar.

Em seguida, o sistema processa a busca e devolve um dos seguintes cenários principais:
a) resultado encontrado com dados básicos e movimentações;
b) ausência de resultado;
c) retorno inconclusivo por erro de digitação, filtro inadequado ou limitação da busca;
d) restrição parcial ou total de acesso em razão de sigilo ou regra de publicidade mitigada.

Na fase final da jornada, o usuário tenta interpretar o que vê. É aqui que a utilidade do serviço se define materialmente. Se a tela apresentar apenas jargão técnico e movimentações pouco inteligíveis, o serviço permanece formalmente público, mas funcionalmente opaco para o cidadão comum.

## 4. Etapas visíveis ao usuário (frontstage)

No frontstage, o usuário interage com elementos perceptíveis do serviço: portal institucional, página de consulta, campos de busca, filtros, botão de submissão, eventuais mecanismos de segurança, tela de resultados, metadados processuais, movimentações, mensagens de erro, avisos de restrição e páginas de indisponibilidade.

Essas etapas visíveis podem ser organizadas assim:

1. **Descoberta do serviço**: localização do menu ou página adequada no portal.
2. **Preenchimento da busca**: inserção de número processual ou outro identificador.
3. **Validação da requisição**: submissão da consulta e eventual verificação de segurança.
4. **Exibição do resultado**: lista de resultados ou abertura direta do processo.
5. **Leitura do processo**: interpretação de classe, órgão, datas e movimentações.
6. **Encerramento da sessão**: obtenção da informação, abandono ou redirecionamento a atendimento humano.

No plano do frontstage, três elementos são especialmente críticos:

* **clareza da linguagem**, porque o cidadão não domina necessariamente a gramática processual;
* **qualidade da busca**, porque erros pequenos podem inviabilizar a localização;
* **mensagens do sistema**, porque a diferença entre “não encontrado”, “acesso restrito” e “erro de consulta” precisa ser semanticamente clara.

## 5. Processos de bastidor (backstage)

O backstage do serviço não se limita à existência do sistema. Ele depende de uma cadeia organizacional, jurídica e tecnológica.

Em primeiro lugar, há a **alimentação processual** realizada pelas unidades judiciárias. Servidores das Auditorias e do STM classificam processos, lançam movimentações, registram metadados, juntam documentos e aplicam corretamente as marcações que impactam a publicidade.

Em segundo lugar, há a **parametrização das regras de exibição pública**. O portal público não expõe tudo o que está nos autos. A visualização depende de regras de negócio associadas a sigilo, publicidade mitigada, perfis de acesso e restrições normativas.

Em terceiro lugar, há a **infraestrutura tecnológica**, composta por portal web, banco de dados, indexação de buscas, disponibilidade de servidores, segurança da informação, gestão de desempenho e manutenção corretiva. O serviço visível ao cidadão só funciona se essa camada estiver estável.

Em quarto lugar, há a **governança institucional**: decisões administrativas sobre transparência, proteção de dados, padrões de publicação, políticas de acessibilidade, tratamento de incidentes e canais de suporte.

Por fim, existe um backstage humano indireto: ouvidoria, atendimento institucional e suporte técnico, que absorvem demandas quando a interface pública não resolve a necessidade do usuário.

## 6. Atores envolvidos e suas funções

Os principais atores humanos e institucionais envolvidos são:

* **Cidadão não autenticado**: usuário externo da consulta pública.
* **Parte processual**: pode recorrer à consulta pública, embora nem sempre obtenha nela tudo o que necessita.
* **Servidor da secretaria da Auditoria Militar**: registra e atualiza dados que alimentam o sistema.
* **Servidor da secretaria judiciária do STM**: atua sobre processos em segundo grau e contribui para consistência das informações públicas.
* **Magistrado ou órgão julgador**: produz decisões e atos cujo registro influencia o conteúdo visível da tramitação.
* **Distribuição/gestão processual**: função administrativa que impacta autuação, classificação e consistência dos metadados.
* **Equipe de tecnologia da informação**: mantém portal, banco de dados, desempenho, segurança e integrações.
* **Área de governança de dados e segurança**: define critérios de publicidade, minimização de dados e conformidade.
* **Ouvidoria ou suporte**: recebe dúvidas e reclamações quando a consulta pública falha ou é insuficiente.

O **e-Proc/JMU** não deve ser tratado como ator humano, mas como **infraestrutura sociotécnica central** do serviço: portal web, regras de exibição, mecanismos de busca, base de dados e lógica de restrição.

## 7. Evidências digitais e pontos de contato

Para fins de blueprint AS-IS, é importante nomear as evidências de forma rastreável:

* **EV1 — Página institucional de acesso ao serviço**
* **EV2 — Tela de consulta processual pública**
* **EV3 — Campos de busca e filtros**
* **EV4 — Mecanismo de validação de segurança**
* **EV5 — Lista de resultados**
* **EV6 — Tela de detalhes básicos do processo**
* **EV7 — Espelho de movimentação processual**
* **EV8 — Mensagens de erro, ausência de resultado ou restrição**
* **EV9 — Página de indisponibilidade ou time-out**
* **EV10 — Canal institucional de suporte/contato**

Essas evidências não são meros detalhes visuais: elas estruturam a linha de interação do usuário e permitem desenhar as swim lanes do blueprint.

## 8. Normativos e condicionantes do serviço

O funcionamento do serviço é condicionado por marcos normativos concretos. A **Constituição Federal**, no art. 93, IX, consagra a publicidade dos julgamentos, mas admite restrições legais quando cabíveis. A **Resolução CNJ nº 121/2010** disciplina a divulgação de dados processuais eletrônicos na internet, sendo central para distinguir o que pode ser publicizado amplamente e o que exige contenção. A **Lei nº 12.527/2011 (LAI)** reforça o dever de transparência, enquanto a **Lei nº 13.709/2018 (LGPD)** impõe limites e critérios ao tratamento e à exposição de dados pessoais em meios digitais.

Além disso, a acessibilidade do canal público deve ser interpretada à luz do **eMAG**, porque a consulta processual pública, sendo serviço digital governamental, não pode pressupor navegação plenamente visual, CAPTCHA indecifrável ou interação incompatível com leitores de tela.

Esses normativos não ficam “ao lado” do serviço: eles condicionam diretamente o que é exibido, como é exibido e quem consegue efetivamente usar o serviço.

## 9. Gargalos, fail points e limitações atuais

Três grupos de falhas merecem destaque.

**Falha 1 — Acessibilidade e barreira de segurança.** Se o mecanismo de validação de segurança for pouco legível ou mal implementado para tecnologias assistivas, o cidadão com deficiência visual pode ter a entrada no serviço inviabilizada. Esse é um fail point crítico porque impede o início da jornada.

**Falha 2 — Busca mal sucedida por sintaxe ou indexação.** A consulta depende de padrões corretos de numeração, grafia e indexação. Erros pequenos no preenchimento, homônimos ou inconsistências na base podem levar a falso negativo, resultado excessivo ou mensagem que não explica a causa do insucesso.

**Falha 3 — Respostas ambíguas diante de restrição ou indisponibilidade.** Quando o sistema não distingue claramente “processo inexistente”, “acesso restrito” e “erro operacional”, gera-se frustração e transbordo para atendimento humano.

Além disso, persistem limitações estruturais: linguagem técnica, dependência de correta alimentação interna, possível lentidão em horários de pico e baixa capacidade de orientação contextual ao usuário leigo.

## 10. Síntese analítica para o blueprint AS-IS

Como base para um Service Blueprint AS-IS, o serviço deve ser representado com pelo menos cinco camadas:

1. **ações do usuário externo não autenticado**;
2. **etapas visíveis do portal público**;
3. **regras e decisões de exibição pública**;
4. **processos humanos e tecnológicos de bastidor**;
5. **fail points, evidências e canais de suporte**.

O ponto central desta versão v2 é que a consulta processual pública no e-Proc/JMU não pode ser tratada apenas como “uma página de busca”. Ela é um serviço público digital condicionado por dados processuais, regras de publicidade mitigada, infraestrutura tecnológica, governança institucional e requisitos de acessibilidade. Seu blueprint AS-IS deverá, portanto, mostrar não apenas o clique do cidadão, mas também a linha de visibilidade entre o que ele enxerga e o que ocorre nas secretarias, na TI e na governança de dados para que aquela informação apareça — ou deixe de aparecer — na interface pública.

