# Conceitos de IA e OpenCode

Documento de orientação e mapeamento dos conceitos de IA Generativa e OpenCode usados no mini-curso.

## 🧠 IA Driven Development

Desenvolvimento guiado por IA — a IA escreve, revisa e planeja o código; o humano supervisiona.

## 📝 Engenharia de Prompts, Engenharia de Software com IA e Vibe Coding

**Engenharia de Prompts** é a prática de elaborar instruções para o LLM obter respostas precisas e úteis.

**Engenharia de Software com IA** é o uso de IA ao longo do ciclo de vida de desenvolvimento de software.

**Vibe Coding** é o estilo de pedir à IA que gere código com base na intuição/impressão ("vibe"), sem detalhes técnicos.

## 🔌 Harness

O Harness é o que transforma um modelo estático (que apenas responde textos) em um agente capaz de agir: ele fecha o loop entre "pensar" (LLM) e "fazer" (ferramentas).

Exemplo: o usuário pede "leia o arquivo X e some 10 às linhas que contêm Y". O Harness recebe a solicitação, chama a ferramenta de leitura, envia o conteúdo ao LLM, recebe a proposta de edição, chama a ferramenta de edição e informa o resultado de volta ao usuário.

## 🗺️ Mapeamento de Relacionamentos

O fluxo lógico resumido mostra como os conceitos se conectam:

Usuário solicita tarefa → OpenCode consulta o **ROSTER** → escolhe uma **SKILL** ou um **AGENTE** → se houver especificação, segue o **SDD** (especificar → planejar → executar) → o agente usa o **LLM** para raciocinar → se precisar de dados externos, usa **RAG** ou **MCP** → se houver múltiplas etapas, organiza como **DAG** → antes/depois da execução, os **HOOKS** disparam ações → procedimentos operacionais seguem os **RUNBOOKS** → o **HARNESS** orquestra todo o loop entre LLM e ferramentas → o resultado depende da qualidade da **ENGENHARIA DE PROMPTS** usada pelo usuário (de **VIBE CODING** a **ENGENHARIA DE SOFTWARE COM IA**).

## 🔄 Exemplo de Fluxo Ponta a Ponta

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

## 📖 Glossário Rápido

- **LLM**: modelo de linguagem grande que gera texto/código.
- **RAG**: busca externa injetada no prompt do LLM.
- **MCP**: protocolo padrão para conectar LLMs a ferramentas e dados.
- **Agentes**: LLM com ferramentas, memória e plano para executar tarefas autônomas.
- **DAG**: grafo direcionado sem ciclos que ordena tarefas por dependência.
- **Skills**: instruções executáveis que ensinam o OpenCode a fazer uma tarefa.
- **Hooks**: ganchos que disparam ações em eventos do ciclo de vida.
- **Runbooks**: manuais com procedimentos passo a passo para operações.
- **Roster**: catálogo de agentes e skills disponíveis para invocação.
- **SDD**: desenvolvimento dirigido por especificação — define o que será construído antes de implementar.
- **IA Driven Development**: desenvolvimento guiado por IA — a IA escreve, revisa e planeja o código; o humano supervisiona.
- **Engenharia de Prompts**: prática de elaborar instruções para o LLM obter respostas precisas e úteis.
- **Engenharia de Software com IA**: uso de IA ao longo do ciclo de vida de desenvolvimento de software.
- **Vibe Coding**: estilo de pedir à IA que gere código com base na intuição/impressão ("vibe"), sem detalhes técnicos.
- **Harness**: ambiente de execução que conecta o LLM a ferramentas, contexto e usuário, fechando o loop pensar-fazer.

## ☁️ Providers (Provedores de LLM)

**Definição.** Provider é o serviço que expõe um modelo de linguagem (LLM) por meio de uma API, cabendo ao cliente (ex.: OpenCode) enviar prompts e receber respostas. Pode ser um provedor de nuvem (NVIDIA NIM, OpenRouter, Hugging Face Inference API) ou local (Ollama rodando modelos no próprio hardware).

**Exemplos usados no curso:** NVIDIA, Ollama, OpenRouter, Hugging Face.

**Conexão com o OpenCode:** o OpenCode se conecta a um ou mais Providers via UI ou CLI, usando a URL da API e um API/Access Token de autenticação. O participante escolhe qual Provider responde a cada tarefa, podendo alternar entre nuvem e local conforme custo, privacidade e disponibilidade.

## 🔑 API/Access Token

**Definição.** API/Access Token é uma credencial de string longa que autentica o cliente junto ao provedor de serviços (Provider ou GitHub), autorizando chamadas à API sem exigir senha de usuário a cada requisição.

**No OpenCode:** o token do Provider (ex.: NVIDIA, OpenRouter) permite ao agente chamar LLMs; o Personal Access Token do GitHub (`$GITHUB_TOKEN`) permite ao agente ler/escrever repositórios e abrir pull requests.

**Boas práticas:** nunca commitar tokens em repositórios Git; armazenar em variáveis de ambiente ou no cofre de credenciais do OpenCode; rotacionar tokens periodicamente e revogar os que não forem mais usados.
