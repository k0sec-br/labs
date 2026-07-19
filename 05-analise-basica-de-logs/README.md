# 05 — Análise Básica de Logs

## Objetivo

Aprender a ler eventos simples em logs fictícios e identificar padrões básicos.

## Nível

Iniciante.

## Pré-Requisitos

- Noções de terminal.
- Noções básicas de data e hora em logs.

## Ambiente Autorizado

Arquivo fictício criado localmente para estudo.

## Ferramentas

- Editor de texto.
- Terminal.
- `grep` ou busca do editor.

## Exemplo de Log

Crie um arquivo `access.log` com:

```text
2026-07-19T10:00:00 local-app INFO usuario=aluno acao=login status=sucesso
2026-07-19T10:02:10 local-app WARN usuario=aluno acao=login status=falha
2026-07-19T10:02:30 local-app WARN usuario=aluno acao=login status=falha
2026-07-19T10:05:00 local-app INFO usuario=aluno acao=logout status=sucesso
```

## Passos

1. Crie uma pasta `lab-logs`.
2. Crie o arquivo `access.log` com o exemplo.
3. Conte quantas linhas existem.
4. Busque por `WARN`.
5. Busque por `falha`.
6. Identifique horários com eventos próximos.
7. Escreva uma hipótese defensiva.
8. Liste quais dados faltariam para uma análise real.

## Perguntas de Reflexão

- O que diferencia evento informativo de alerta?
- Duas falhas de login são sempre incidente?
- Quais informações ajudam a investigar sem expor dados pessoais?

## Resultado Esperado

Você deve conseguir filtrar logs simples e formular hipóteses sem concluir além das evidências.

## Encerramento ou Limpeza

Remova a pasta `lab-logs` se não quiser manter o exemplo.

## Limites Éticos e Legais

Não use logs reais contendo dados pessoais sem autorização. Remova ou anonimize informações sensíveis antes de compartilhar.
