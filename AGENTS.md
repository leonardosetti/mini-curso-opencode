# Seminários FATEC Taquaritinga

Repositório de trabalhos de seminário, um por tema. Não é projeto de software: sem Makefile, CI, lockfile ou venv. `python-docx` está disponível no Python do sistema.

## 📁 Organização

- **Modelo de referência de formato** (capa, rosto, resumo, sumário, referências ABNT): imitar estrutura, não conteúdo.
- **Tema atual — bancos de dados não convencionais**: gerador de documentos a partir de arquivos de conteúdo, com pasta de figuras e documento de saída pronto.
- **Configuração do OpenCode**: providers, agentes e servidores MCP locais. Agente principal: academicus-ti.
- **Validador de regras do agente**: executar antes de entregar.

## 🖥️ Inferência local (lenta — planeje)

- LLM local via llama.cpp (`http://127.0.0.1:8080/v1`), modelo `qwen30b-moe` (Q4_K_M), CPU-only, ~8 tok/s, janela 32768 / saída 4096.
- Seja conciso. Não regenere textos longos; reutilize trechos já gerados.

## ✍️ Criar ou editar um seminário

1. Crie a pasta do tema.
2. Use o modelo de referência de formato (estrutura, fontes, alinhamento justificado).
3. Gere o documento primeiro (via gerador), revise depois. Não entregue apenas md/txt.
4. ABNT: NBR 6023 (referências), NBR 10520 (citações), NBR 14724 (apresentação).

## 📜 Regras ACADEMICUS-TI (obrigatórias)

Execute antes de entregar: `python3 scripts/academicus-ti_validate.py` (deve dar 14/14).

- Toda afirmação factual: fonte com DOI/URL resolvido ou `[NÃO VERIFICADO]`. Zero tolerância a alucinação de citações/DOIs/dados.
- Sem Sci-Hub nem fontes piratas; apenas open-access.
- Não coletar dados pessoais em crawling (LGPD/GDPR).
- Anti-plágio: 0 sequências ≥ 8 palavras idênticas a fontes; citação direta ≤ 10%.
- Declaração de uso de IA ao final (ferramenta, data, escopo).
- Kill Switch: recusar dados pessoais, biometria, Sci-Hub; logar e notificar humano.
- Bypass Protocol: só com (1) justificativa, (2) plano de migração, (3) prazo. Sem um dos três: recusar.

## 🛠️ Comandos

- `python3 scripts/academicus-ti_validate.py` — auto-checar regras do agente.
- `python3 .../gerar_docx.py` — regenerar o documento do tema atual.
- `bash scripts/adesao_iniciativa.sh` — instala MCPs, sobe Qdrant/Graphify, grava configuração do OpenCode, valida (14/14). Reinicie o OpenCode após.

## ⚠️ Gotchas

- Arquivos de bloqueio do LibreOffice aparecem enquanto o documento está aberto; ignorar ou remover após fechar.
- Áudio (.m4a) e .pptx na pasta de referência são materiais grandes; não é necessário lê-los para gerar texto.
