# Conceitos de IA e OpenCode — Orientação e Mapeamento

Este documento explica cada conceito no contexto de inteligência artificial e do OpenCode, e mapeia como eles se relacionam entre si.

---

## Índice

1. [LLM](#llm)
2. [RAG](#rag)
3. [MCP](#mcp)
4. [AGENTS](#agents)
5. [DAG](#dag)
6. [SKILLS](#skills)
7. [HOOKS](#hooks)
8. [RUNBOOKS](#runbooks)
9. [ROSTER](#roster)
10. [SDD](#sdd)
11. [AI Driven Development](#ai-driven-development)
12. [Engenharia de Prompts, Engenharia de Software com IA e Vibe Coding](#engenharia-de-prompts-engenharia-de-software-com-ia-e-vibe-coding)
13. [Harness](#harness)
14. [Mapeamento de relacionamentos](#mapeamento-de-relacionamentos)
15. [Exemplo de fluxo ponta a ponta](#exemplo-de-fluxo-ponta-a-ponta)

---

## LLM — Large Language Model

**O que é.** Um LLM (Modelo de Linguagem Grande) é um modelo de inteligência artificial treinado em grandes volumes de texto que aprende a prever o próximo token (palavra, parte de palavra ou caractere) dado um contexto de entrada. É o "cérebro" por trás da maioria dos sistemas de IA generativa atuais.

**No contexto da IA.** O LLM é o componente central de quase toda aplicação de IA moderna: chatbots, assistentes de código, geradores de conteúdo, resumidores, tradutores, entre outros. Ele não acessa a internet por si só, não executa comandos e não lembra de conversas passadas a menos que o contexto seja reenviado a cada interação.

**No contexto do OpenCode.** No OpenCode, o LLM é o modelo que gera as respostas, planeja etapas, escreve código e decide quais ferramentas chamar. O OpenCode não é um modelo próprio; ele orquestra chamadas a modelos (via provedores configurados) e gerencia o contexto enviado a cada requisição.

**Exemplo.** O usuário pede "escreva uma função que soma dois números". O OpenCode monta um prompt com essa instrução e envia ao LLM configurado. O LLM retorna o código da função.

---

## RAG — Retrieval-Augmented Generation

**O que é.** RAG (Geração Aumentada por Recuperação) é uma técnica que combina busca em fontes externas com geração de texto por LLM. Em vez de confiar apenas no que o modelo aprendeu no treinamento, o sistema busca documentos relevantes (código, documentação, tickets, memória) e injeta esse conteúdo no prompt antes de pedir a resposta ao LLM.

**No contexto da IA.** Resolve problemas comuns de LLMs: alucinações (inventar fatos), desconhecimento de dados recentes e falta de acesso a informações privadas da empresa. O fluxo é: 1) o usuário faz uma pergunta; 2) o sistema busca trechos relevantes em uma base de conhecimento; 3) esses trechos são adicionados ao contexto do LLM; 4) o LLM gera a resposta com base nos trechos fornecidos.

**No contexto do OpenCode.** O OpenCode pode usar RAG quando indexa o repositório local, documentação do projeto ou sessões anteriores para que o agente responda com base no que realmente existe no código, em vez de generalizar a partir do treinamento do modelo. Isso melhora a precisão em tarefas de refatoração, debug e geração de código alinhado ao estilo do projeto.

**Exemplo.** O usuário pergunta "qual é a função de validação de CPF neste projeto?". O sistema busca no repositório trechos que contenham "validarCPF" ou "CPF", injeta esses trechos no prompt e o LLM responde com o caminho exato da função.

---

## MCP — Model Context Protocol

**O que é.** MCP (Protocolo de Contexto para Modelos) é um padrão aberto que define como LLMs se conectam a fontes externas de dados e ferramentas. Em vez de cada aplicação criar integrações proprietárias, o MCP estabelece uma interface comum entre clientes (LLM/OpenCode) e servidores MCP que expõem recursos como bancos de dados, sistemas de arquivos, GitHub, Slack, entre outros.

**No contexto da IA.** Padroniza a forma como modelos de linguagem acessam contexto externo e executam ações. Um servidor MCP expõe recursos (arquivos, consultas, APIs) e o cliente MCP (o LLM ou o agente que o rodeia) consome esses recursos de forma uniforme.

**No contexto do OpenCode.** O OpenCode pode se conectar a servidores MCP para estender suas capacidades sem escrever código personalizado para cada ferramenta. Por exemplo, um servidor MCP do GitHub permite que o agente leia issues, crie pull requests e faça commits por meio da interface padronizada do MCP.

**Exemplo.** Um servidor MCP de sistema de arquivos expõe a leitura de arquivos locais. O OpenCode, como cliente MCP, solicita a leitura de `src/app.py` e recebe o conteúdo, que então é enviado ao LLM como contexto.

---

## AGENTS — Agentes de IA

**O que é.** Um agente de IA é um sistema que usa um LLM como motor de raciocínio e adiciona a ele ferramentas, memória e capacidade de planejamento para executar tarefas de forma autônoma ou semiautônoma. Diferente de um chat simples, que responde e encerra, um agente pode dividir um objetivo em etapas, decidir qual ferramenta usar em cada etapa, executar, observar o resultado e ajustar o plano.

**No contexto da IA.** Agentes são usados para automação de fluxos complexos: pesquisa na web, escrita e execução de código, interação com APIs, operação de sistemas, entre outros. Eles combinam o poder de linguagem do LLM com a capacidade de agir no mundo real.

**No contexto do OpenCode.** O próprio OpenCode funciona como um agente: ele recebe uma tarefa do usuário, planeja os passos, escolhe ferramentas disponíveis (ler arquivos, editar, executar comandos, gerar imagens, consultar GitHub, etc.), executa e itera até o objetivo ser atingido. Skills e Hooks são mecanismos que ampliam o que esse agente pode fazer.

**Exemplo.** O usuário pede "corrija o bug na página de login e abra um pull request". O agente OpenCode: 1) lê o arquivo relevante; 2) identifica o problema; 3) edita o código; 4) executa testes; 5) usa uma ferramenta MCP do GitHub para criar o pull request.

---

## DAG — Directed Acyclic Graph

**O que é.** DAG (Grafo Direcionado Acíclico) é uma estrutura de dados composta por nós (vértices) e arestas direcionadas, sem ciclos. Cada aresta aponta de um nó para outro, e não é possível voltar a um nó já visitado seguindo as arestas. É amplamente usado para representar dependências e ordem de execução.

**No contexto da IA.** DAGs modelam pipelines de machine learning e fluxos de agentes multi-etapa, onde cada nó é uma tarefa (extrair dados, treinar modelo, avaliar, implantar) e as arestas definem qual tarefa depende de qual. A ausência de ciclos garante que o fluxo tenha um início e um fim bem definidos.

**No contexto do OpenCode.** Quando um plano de fase envolve múltiplas tarefas com dependências (ex: "criar o banco de dados antes de criar a API"), o OpenCode pode organizar essas tarefas como um DAG e executá-las na ordem correta, paralelizando as que são independentes. Isso evita bloqueios e garante que nenhuma etapa comece antes de suas dependências estarem prontas.

**Exemplo.** Tarefa A (escrever código) → Tarefa B (executar testes) → Tarefa C (abrir PR). A depende de nada, B depende de A, C depende de B. O grafo é: A → B → C.

---

## SKILLS — Habilidades do OpenCode

**O que é.** Skills são instruções que ensinam o agente OpenCode a executar um tipo específico de tarefa. Elas definem quando uma skill deve ser invocada, o que ela faz e quais passos seguir. São como "manuais de procedimento executáveis" para o agente.

**No contexto da IA.** Em sistemas de agentes, skills correspondem a capacidades especializadas que o modelo pode acionar quando o contexto exige. Elam transformam um LLM de propósito geral em um assistente capaz de realizar tarefas específicas de domínio.

**No contexto do OpenCode.** O OpenCode vem com skills embutidas (por exemplo, para criar projetos, revisar código, gerar documentação, executar testes, criar PRs) e permite que usuários adicionem skills personalizadas. Quando o usuário pede algo que corresponde a uma skill conhecida, o agente pode delegar a execução para ela.

**Exemplo.** O usuário pede "revisar este pull request". O OpenCode identifica que isso corresponde à skill de code review e executa os passos definidos nela: ler o diff, identificar problemas, sugerir correções.

---

## HOOKS — Ganchos do OpenCode

**O que é.** Hooks são pontos de extensão que disparam ações automaticamente em eventos específicos do ciclo de vida do OpenCode (antes ou depois de um comando, antes de editar um arquivo, após concluir uma tarefa, etc.). Eles permitem customizar o comportamento do agente sem modificar seu núcleo.

**No contexto da IA.** Hooks são semelhantes a middlewares ou callbacks em sistemas de agentes: permitem injetar lógica personalizada em momentos chave da execução, como validação, logging, notificação ou limpeza.

**No contexto do OpenCode.** O usuário ou o sistema pode registrar hooks para executar ações antes ou depois de eventos do OpenCode. Por exemplo, um hook pode rodar linters automaticamente antes de salvar um arquivo, ou enviar uma notificação quando uma fase de projeto é concluída.

**Exemplo.** Um hook configurado para o evento `preEdit` executa um linter no arquivo antes que o agente escreva nele, garantindo que o código siga o padrão do projeto.

---

## RUNBOOKS — Manuais de Procedimento

**O que é.** Runbooks são documentos que descrevem passo a passo como executar procedimentos operacionais: desplegar uma aplicação, responder a incidentes, restaurar backups, configurar ambientes, entre outros. Eles padronizam operações para que qualquer pessoa (ou agente) siga o mesmo procedimento.

**No contexto da IA.** Runbooks servem como referência para agentes e equipes humanas sobre como lidar com situações recorrentes, reduzindo erros e tempo de resolução.

**No contexto do OpenCode.** Runbooks podem ser indexados via RAG para que o agente consulte procedimentos antes de executar tarefas operacionais. Eles também orientam o próprio uso do OpenCode: como iniciar um projeto, como criar uma fase, como revisar código, como lidar com falhas.

**Exemplo.** Um runbook de "deploy de emergência" lista: 1) criar branch de hotfix; 2) aplicar correção; 3) rodar testes; 4) abrir PR; 5) fazer merge após aprovação. O agente OpenCode segue esses passos quando acionado para um deploy urgente.

---

## ROSTER — Catálogo de Agentes e Skills

**O que é.** O roster (lista/escala) é o catálogo de agentes e skills disponíveis no ambiente OpenCode. Ele registra quem ou o que pode ser invocado para cada tipo de tarefa, servindo de guia para o agente escolher o executante adequado.

**No contexto da IA.** Em sistemas multi-agente, o roster é o registro de agentes especializados e suas capacidades, permitindo roteamento inteligente de tarefas.

**No contexto do OpenCode.** O roster lista as skills instaladas, os agentes configurados e seus escopos de atuação. Quando o usuário faz uma solicitação, o OpenCode consulta o roster para decidir qual skill ou qual agente é mais adequado para atender a demanda.

**Exemplo.** O roster contém: skill `code-review` (revisa diffs), skill `gsd-plan-phase` (planeja fases), agent `explore` (explora código). O usuário pede "planeje a próxima fase" → o OpenCode consulta o roster e escolhe a skill `gsd-plan-phase`.

---

## SDD — Specification-Driven Development

**O que é.** SDD (Desenvolvimento Dirigido por Especificação) é uma abordagem onde a especificação detalhada do que deve ser construído é definida antes da implementação. No contexto do ecossistema GSD/OpenCode, isso se manifesta em artefatos como SPEC.md, PLAN.md, ROADMAP.md e PLANNING que guiam o agente na execução de fases de projeto.

**No contexto da IA.** Em vez de pedir ao LLM para "construir X" de forma aberta, o SDD fornece uma especificação estruturada que restringe o espaço de soluções, reduz ambiguidades e torna o resultado mais previsível e verificável. A especificação atua como contrato entre a intenção humana e a geração da IA.

**No contexto do OpenCode.** O OpenCode/GSD utiliza SDD em fases: antes de executar, o agente discute, especifica (SPEC.md), planeja (PLAN.md) e só então executa. Isso evita que o agente comece a escrever código sem um alvo claro e permite revisão e convergência do plano antes de qualquer alteração no repositório.

**Exemplo.** O usuário quer uma nova funcionalidade. O OpenCode não escreve código imediatamente: primeiro gera a especificação do que será entregue, valida com o usuário, cria o plano de execução com dependências e só então executa cada etapa.

---

## AI Driven Development

**O que é.** AI Driven Development (Desenvolvimento Guiado por IA) é um paradigma de desenvolvimento de software onde a inteligência artificial assume um papel central na escrita, revisão, refatoração e até no planejamento do código. O desenvolvedor deixa de digitar cada linha para passar a orientar, revisar e curar o que a IA produz.

**No contexto da IA.** O LLM deixa de ser uma ferramenta de autocomplete para se tornar um parceiro de desenvolvimento que entende o domínio, gera testes, propõe arquitetura e encontra bugs. O foco muda de "digitar sintaxe" para "definir requisitos e validar comportamento".

**No contexto do OpenCode.** O OpenCode é, por essência, uma ferramenta de AI Driven Development: o usuário descreve o objetivo em linguagem natural e o agente escreve o código, executa comandos, lê arquivos, cria PRs e itera até a tarefa estar concluída. O desenvolvedor supervisiona e intervém quando necessário, em vez de escrever tudo manualmente.

**Exemplo.** Em vez de criar um componente React à mão, o usuário pede "crie um formulário de cadastro com validação de e-mail". O OpenCode gera o componente, os testes e ajusta conforme feedback.

---

## Engenharia de Prompts, Engenharia de Software com IA e Vibe Coding

**Engenharia de Prompts.** É a prática de elaborar, testar e refinar as instruções (prompts) enviadas a um LLM para obter respostas mais precisas, úteis e seguras. Inclui técnicas como dar exemplos (few-shot), definir papéis ao modelo, estruturar a saída e encadear raciocínio (chain-of-thought).

**Engenharia de Software com IA.** É o uso de IA como parte integrante do ciclo de vida de desenvolvimento de software: geração de código, revisão automática, escrita de testes, documentação, refatoração assistida e manutenção. Diferente de usar IA apenas como chat, aqui ela está embutida nos processos de engenharia.

**Vibe Coding.** É um estilo de desenvolvimento onde o programador confia na intuição e na descrição de "como deve parecer/funcionar" (a "vibe") para a IA gerar o código, sem especificar detalhes técnicos ou revisar profundamente o resultado. É rápido para protótipos, mas arriscado para código de produção, pois pode gerar soluções que funcionam superficialmente sem robustez, segurança ou manutenibilidade.

**No contexto do OpenCode.** O OpenCode permite ambos os extremos: o usuário pode fazer um pedido vaga (estilo vibe coding) e o agente tentará atender, ou pode seguir um caminho mais estruturado com SDD, specs e planos (engenharia de software com IA). A engenharia de prompts entra em como o usuário formula a solicitação para que o agente entenda corretamente o que quer.

**Exemplo.** Vibe coding: "faça um app de tarefas bonito" → o OpenCode gera algo visual, mas pode faltar persistência ou testes. Engenharia de software com IA: especificar requisitos, criar planos, revisar o diff, rodar testes e só então entregar.

---

## Harness

**O que é.** Harness (arreio/coleira, em tradução livre) é o ambiente de execução que conecta o modelo de linguagem às ferramentas, ao contexto e ao usuário. No contexto do OpenCode, o harness é a camada que orquestra o ciclo do agente: recebe a solicitação, monta o contexto, chama o LLM, executa as ferramentas escolhidas (ler arquivo, editar, executar comando, consultar GitHub via MCP, etc.), captura resultados e repassa ao modelo para a próxima iteração.

**No contexto da IA.** O harness é o que transforma um modelo estático (que apenas responde textos) em um agente capaz de agir: ele fecha o loop entre "pensar" (LLM) e "fazer" (ferramentas).

**No contexto do OpenCode.** O OpenCode roda dentro de um harness que gerencia sessões, worktrees, skills, hooks, chamadas de ferramentas e o histórico de conversa. Sem o harness, o LLM seria apenas um modelo de previsão de token; com ele, o OpenCode se torna um agente de software autônomo.

**Exemplo.** O usuário pede "leia o arquivo X e some 10 às linhas que contêm Y". O harness recebe a solicitação, chama a ferramenta de leitura, envia o conteúdo ao LLM, recebe a proposta de edição, chama a ferramenta de edição e informa o resultado de volta ao usuário.

---

## Mapeamento de Relacionamentos

A tabela abaixo mostra como os conceitos se conectam entre si:

| Conceito | Relaciona-se com | Como |
|----------|------------------|------|
| **LLM** | RAG | O LLM recebe o contexto recuperado pelo RAG e gera a resposta. |
| **LLM** | AGENTS | O LLM é o motor de raciocínio dentro de um agente. |
| **LLM** | MCP | O LLM consome recursos expostos por servidores MCP. |
| **RAG** | SKILLS | Skills podem disparar buscas RAG para obter contexto antes de agir. |
| **RAG** | RUNBOOKS | Runbooks indexados via RAG são consultados pelo agente durante operações. |
| **AGENTS** | DAG | Agentes organizam tarefas multi-etapa como DAGs para respeitar dependências. |
| **AGENTS** | SKILLS | Agentes delegam tarefas específicas a skills do seu catálogo. |
| **AGENTS** | HOOKS | Hooks disparam ações antes ou depois da execução de um agente. |
| **SKILLS** | HOOKS | Uma skill pode registrar hooks para customizar seu próprio ciclo de execução. |
| **SKILLS** | ROSTER | Skills são entradas do roster, que o agente consulta para escolher quem executar. |
| **HOOKS** | RUNBOOKS | Um hook pode iniciar a execução de um runbook em resposta a um evento. |
| **MCP** | AGENTS | Agentes usam MCP para acessar ferramentas e dados externos. |
| **ROSTER** | AGENTS | O roster lista os agentes disponíveis para invocação. |
| **SDD** | SKILLS | Skills de planejamento e execução de fases seguem o fluxo SDD (especificar → planejar → executar). |
| **SDD** | AI DRIVEN DEVELOPMENT | O SDD é o modelo de trabalho que torna o desenvolvimento guiado por IA previsível e verificável. |
| **AI DRIVEN DEVELOPMENT** | ENGENHARIA DE PROMPTS | A forma como o usuário direciona a IA (prompts) determina a qualidade do desenvolvimento guiado por IA. |
| **ENGENHARIA DE PROMPTS** | VIBE CODING | Vibe coding é o extremo de prompts vagos e intuição; engenharia de prompts é o extremo de instruções estruturadas. |
| **HARNESS** | LLM | O harness fecha o loop entre o LLM (pensar) e as ferramentas (fazer). |
| **HARNESS** | HOOKS | O harness dispara hooks nos eventos do ciclo de vida do agente. |
| **HARNESS** | SKILLS | O harness invoca skills quando o contexto da tarefa corresponde a uma delas. |
| **HARNESS** | AGENTS | O harness é o ambiente que executa o agente OpenCode. |
| **ENGENHARIA DE SOFTWARE COM IA** | SDD | A especificação antecipada (SDD) é uma prática de engenharia de software com IA para evitar código gerado sem alvo. |

**Fluxo lógico resumido:**

Usuário solicita tarefa → OpenCode consulta **ROSTER** → escolhe **SKILL** ou **AGENTE** → se houver especificação, segue **SDD** (especificar → planejar → executar) → o agente usa o **LLM** para raciocinar → se precisar de dados externos, usa **RAG** ou **MCP** → se houver múltiplas etapas, organiza como **DAG** → antes/depois da execução, **HOOKS** disparam ações → procedimentos operacionais seguem **RUNBOOKS** → o **HARNESS** orquestra todo o loop entre LLM e ferramentas → o resultado depende da qualidade da **ENGENHARIA DE PROMPTS** usada pelo usuário (de **VIBE CODING** a **ENGENHARIA DE SOFTWARE COM IA**).

---

## Exemplo de Fluxo Ponta a Ponta

Cenário: o usuário pede "corrija o bug na página de login e prepare o release".

1. **ROSTER**: OpenCode consulta o catálogo e encontra a skill `gsd-execute-phase` e o agent `explore`.
2. **SKILLS**: A skill `gsd-execute-phase` assume a tarefa e planeja as etapas.
3. **DAG**: As etapas são ordenadas: A (ler código do login) → B (corrigir bug) → C (executar testes) → D (abrir PR). B depende de A, C depende de B, D depende de C.
4. **AGENTE**: O agent `explore` lê os arquivos relevantes da página de login.
5. **LLM**: O agente envia o trecho do código ao LLM com a instrução de corrigir o bug. O LLM propõe a correção.
6. **MCP**: Se o repositório estiver em um servidor remoto, o agente usa um servidor MCP do GitHub para ler e escrever arquivos.
7. **HOOKS**: Antes de salvar a correção, um hook `preEdit` executa o linter. Após salvar, um hook `postEdit` roda os testes.
8. **RUNBOOKS**: O agente consulta o runbook "release de correção" para seguir os passos de abertura de PR e deploy.
9. **RAG**: Durante o processo, o agente busca no repositório documentação sobre o padrão de commits e mensagens de erro, injetando via RAG no contexto do LLM.

---

## Glossário Rápido

- **LLM**: modelo de linguagem grande que gera texto/código.
- **RAG**: busca externa injetada no prompt do LLM.
- **MCP**: protocolo padrão para conectar LLMs a ferramentas e dados.
- **AGENTES**: LLM com ferramentas, memória e plano para executar tarefas autônomas.
- **DAG**: grafo direcionado sem ciclos que ordena tarefas por dependência.
- **SKILLS**: instruções executáveis que ensinam o OpenCode a fazer uma tarefa.
- **HOOKS**: ganchos que disparam ações em eventos do ciclo de vida.
- **RUNBOOKS**: manuais com procedimentos passo a passo para operações.
- **ROSTER**: catálogo de agentes e skills disponíveis para invocação.
- **SDD**: desenvolvimento dirigido por especificação — define o que será construído antes de implementar.
- **AI DRIVEN DEVELOPMENT**: desenvolvimento guiado por IA — a IA escreve, revisa e planeja o código; o humano supervisiona.
- **ENGENHARIA DE PROMPTS**: prática de elaborar instruções para o LLM obter respostas precisas e úteis.
- **ENGENHARIA DE SOFTWARE COM IA**: uso de IA ao longo do ciclo de vida de desenvolvimento de software.
- **VIBE CODING**: estilo de pedir à IA que gere código com base na intuição/impressão ("vibe"), sem detalhes técnicos.
- **HARNESS**: ambiente de execução que conecta o LLM a ferramentas, contexto e usuário, fechando o loop pensar-fazer.

---

## Providers (Provedores de LLM)

**Definição.** Provider é o serviço que expõe um modelo de linguagem (LLM) por meio de uma API, cabendo ao cliente (ex.: OpenCode) enviar prompts e receber respostas. Pode ser um provedor de nuvem (NVIDIA NIM, OpenRouter, Hugging Face Inference API) ou local (Ollama rodando modelos no próprio hardware).

**Exemplos usados no curso:** NVIDIA, Ollama, OpenRouter, Hugging Face.

**Conexão com o OpenCode:** o OpenCode se conecta a um ou mais Providers via UI ou CLI, usando a URL da API e um API/Access Token de autenticação. O participante escolhe qual Provider responde a cada tarefa, podendo alternar entre nuvem e local conforme custo, privacidade e disponibilidade.

## API/Access Token

**Definição.** API/Access Token é uma credencial de string longa que autentica o cliente junto ao provedor de serviços (Provider ou GitHub), autorizando chamadas à API sem exigir senha de usuário a cada requisição.

**No OpenCode:** o token do Provider (ex.: NVIDIA, OpenRouter) permite ao agente chamar LLMs; o Personal Access Token do GitHub (`$GITHUB_TOKEN`) permite ao agente ler/escrever repositórios e abrir pull requests.

**Boas práticas:** nunca commitar tokens em repositórios Git; armazenar em variáveis de ambiente ou no cofre de credenciais do OpenCode; rotacionar tokens periodicamente e revogar os que não forem mais usados.

---
*Documento de orientação e mapeamento de conceitos — IA e OpenCode.*
Versão: 1.0.0 | Data: 10/11/2026 | Vinculado a: mini-curso-ementa.md
