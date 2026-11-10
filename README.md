# Ementa — Mini-Curso: Ferramentas de IA com SDD e Vibe Coding (OpenCode)

## 1. Proposta

Este mini-curso orienta e coloca em prática iniciativas de desenvolvimento de software com suporte intensivo de IA Generativa (LLM), aplicados à ferramenta **OpenCode** — um agente de programação de código aberto e acesso livre, instalável localmente em múltiplos ambientes, com suporte a provedores (providers) de LLM na nuvem e locais, configuração simples e gerenciamento isolado de múltiplos projetos. O OpenCode é customizável por meio de Skills, Agentes, Hooks, MCP e outros recursos.

O curso alia **Vibe Coding** (geração de código guiada por intenção, "mão na massa") a práticas de **Engenharia de Software com IA** (SDD — desenvolvimento dirigido por especificação, constitution, specs e validação), de modo que o participante saia apto a usar IA como parceira de desenvolvimento, e não como substituta do julgamento técnico.

## 2. Objetivos

Ao final do curso, o participante deverá ser capaz de:

1. Explicar os conceitos fundamentais de IA Generativa aplicada ao desenvolvimento (LLM, RAG, DAG, Roster, Skills, Hooks, Runbooks, Agentes, MCP, Providers e API/Access Token).
2. Configurar e replicar o ambiente de trabalho (WSL, Python, Node.js, Docker, Git, OpenCode).
3. Conectar provedores de LLM (Providers) via API/Access Token no OpenCode.
4. Configurar Skills, MCPs e um circuito simples de agentes com orquestrador (DAG e Roster).
5. Escrever e habilitar uma constitution de projeto e specs iniciais de um projeto greenfield.
6. Implementar um projeto simples em duas camadas (CLI e UI/Web) usando IA Generativa.
7. Fazer deploy simples do resultado no GitHub Pages.

## 3. Documentos vinculados a esta ementa

- [conceitos-ia-opencode.md](conceitos-ia-opencode.md) — definições detalhadas dos conceitos de IA e OpenCode abordados no curso (incluindo Providers e API/Access Token).
- [roteiro-ambiente-ferramentas.md](roteiro-ambiente-ferramentas.md) — roteiro passo a passo de instalação e configuração de ambiente e ferramentas exigidas no laboratório.

## 4. Pré-requisitos

### Mandatórios

- Usuário ativo no GitHub
- E-mail próprio

### Desejáveis

- Conhecimento básico de ambiente Unix-like (Linux)
- Conhecimentos básicos de redes (TCP/IP, HTTPS, portas, SSH)
- Conhecimento de JSON
- Boa escrita técnica
- Engenharia de software básica

## 5. Estrutura de laboratório esperada

- Máquinas com conexão à internet
- Acesso ao WSL (Windows Subsystem for Linux)
- Git instalado

## 6. Módulos e cronograma (19:00 às 22:00 — 180 min exatos)

| Bloco | Módulo | Horário | Duração |
|-------|--------|---------|---------|
| A | Conceitos fundamentais de IA e OpenCode | 19:00–19:30 | 30 min |
| B | Configuração de ambiente e ferramentas | 19:30–20:15 | 45 min |
| C | OpenCode: Providers, Skills, MCPs e agentes | 20:15–21:00 | 45 min |
| D | Projeto greenfield: SDD, constitution, specs, vibe coding e deploy | 21:00–22:00 | 60 min |
| **Total** | | **19:00–22:00** | **180 min** |

### Bloco A — Conceitos fundamentais de IA e OpenCode (30 min) — 19:00–19:30

Apresentação rápida dos conceitos que permeiam a iniciativa antes das configurações e da prática de vibe coding.

- LLM, RAG, DAG, Roster, Skills, Hooks, Runbooks, Agentes, MCP, Providers, API/Access Token
- Diferença entre Vibe Coding e Engenharia de Software com IA
- O papel do instrutor: orientação, não execução dos laboratórios pelos participantes

*Referência:* [conceitos-ia-opencode.md](conceitos-ia-opencode.md)

### Bloco B — Configuração de ambiente e ferramentas (45 min) — 19:30–20:15

Instalação e configuração do ambiente de laboratório, seguindo o roteiro vinculado.

- WSL, Python, pip, Uvicorn, Nvm, Node.js, Docker, Git
- Instalação do OpenCode (opencode.json básico, funcional e puro)
- Validação de cada ferramenta instalada

*Referência:* [roteiro-ambiente-ferramentas.md](roteiro-ambiente-ferramentas.md)

### Bloco C — OpenCode: Providers, Skills, MCPs e agentes (45 min) — 20:15–21:00

Configuração da ferramenta OpenCode e integração de recursos.

- Conectar Providers (UI/CLI): NVIDIA, Ollama, OpenRouter, Hugging Face
- Configurar API/Access Token (incluindo `$GITHUB_TOKEN` / Personal Access Token)
- Setup do GitHub como provedor de tokens
- Instalação e ativação de Skills, MCPs e circuito simples de agentes
- Conceitos de economia de tokens, ganho de performance, memória persistente (ex.: Graphify) e conectividade com plataformas externas (GitHub)

### Bloco D — Projeto greenfield: SDD, constitution, specs, vibe coding e deploy (60 min) — 21:00–22:00

Desenvolvimento prático de um projeto do zero (greenfield), em duas seções: CLI e UI/Web.

- Definição estrutural do projeto e sugestão de stacks simples (a, b, c) formatadas para GitHub Pages
- Definição de constitution de projeto
- Especs iniciais e execução da implementação com auxílio de IA
- Duas seções práticas: uma via CLI e outra via UI/Web
- Vibe coding 100% usando IA Generativa, supervisionado pelo instrutor
- Deploy simples no GitHub Pages

## 7. Workflow de cumprimento dos objetivos

1. **Apresentação e conceituação (Bloco A):** exposição dialogada com exemplos reais; uso do [conceitos-ia-opencode.md](conceitos-ia-opencode.md) como material de apoio.
2. **Pré-configuração (Bloco B):** cada participante segue o [roteiro-ambiente-ferramentas.md](roteiro-ambiente-ferramentas.md); o instrutor valida a instalação de cada ferramenta antes de avançar.
3. **Configuração da ferramenta (Bloco C):** cadastro de Providers e API/Access Token; ativação de Skills, MCPs e agentes; teste de conexão com cada provedor.
4. **Implementação orientada (Bloco D):** definição de constitution e specs (SDD); codificação assistida por IA nas duas camadas (CLI e UI/Web); revisão crítica do código gerado; deploy no GitHub Pages.
5. **Encerramento:** verificação dos critérios de sucesso e assinatura do termo de aceite.

## 8. Critérios de sucesso

- Ambiente 100% configurado e replicável pelo participante a partir do roteiro
- Compreensão dos conceitos de Engenharia de Software com IA Generativa
- OpenCode configurado com Providers e API/Access Token funcionais
- Integração de recursos (Skills, MCPs, agentes) no projeto
- Desenvolvimento de agentes simples
- Circuito de agentes com orquestrador (L0..LN) usando DAG e Roster
- Constitution de projeto escrita, validada e habilitada
- Specs iniciais inteligentes e utilizadas na implementação
- Vibe coding supervisionado em 100% da implementação prática
- Deploy simples de um site no GitHub Pages

## 9. Controle de versão

| Versão | Data | Autor | Mudança |
|--------|------|-------|---------|
| 0.0.1 | — | Rascunho | Estrutura inicial de brainstorm |
| 1.0.0 | 10/11/2026 | Leonardo Setti | Ementa consolidada com cronograma 19:00–22:00, blocos nomeados, workflow, documentos vinculados e critérios de sucesso |

## 10. Aceite da coordenação e diretoria

| Responsável | Papel | Data | Assinatura/Aceite |
|-------------|-------|------|-------------------|
|             |       |      |                   |
| Leonardo Setti | Instrutor | 10/11/2026 | Leonardo Setti |
