# 03 — Análise de Headers HTTP

## Objetivo

Observar headers HTTP em ambiente local ou sistema explicitamente autorizado.

## Nível

Iniciante.

## Pré-Requisitos

- Noções de HTTP.
- Navegador.
- Terminal.

## Ambiente Autorizado

Use uma página local, serviço próprio ou ambiente autorizado. Não use este laboratório para testar sites de terceiros.

## Ferramentas

- Navegador com ferramentas de desenvolvedor.
- `curl`.
- Servidor local simples, se disponível.

## Passos

1. Crie uma pasta `lab-headers`.
2. Crie um arquivo `index.html` com conteúdo simples.
3. Sirva a pasta localmente com uma ferramenta de sua escolha.
4. Acesse a página no navegador.
5. Abra as ferramentas de desenvolvedor.
6. Observe headers de resposta.
7. Execute `curl -I` contra o endereço local.
8. Registre headers encontrados.
9. Pesquise o significado de `Content-Type`, `Cache-Control` e `Server`, se aparecerem.
10. Escreva quais headers de segurança você gostaria de estudar depois.

## Perguntas de Reflexão

- O que um header revela?
- Por que headers de segurança são úteis?
- Qual a diferença entre ambiente local e um site de terceiro?

## Resultado Esperado

Você deve conseguir observar headers sem tocar em sistemas não autorizados.

## Encerramento ou Limpeza

Pare o servidor local e remova arquivos temporários se desejar.

## Limites Éticos e Legais

Não faça enumeração, varredura ou testes automatizados contra sites públicos. Use somente serviços próprios ou autorizados.
