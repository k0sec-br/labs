# 01 — Linux Permissões

## Objetivo

Entender permissões básicas de arquivos em Linux usando arquivos locais.

## Nível

Iniciante.

## Pré-Requisitos

- Terminal Linux, WSL, VM ou ambiente educacional.
- Comandos básicos: `pwd`, `ls`, `cd`, `mkdir`, `touch`.

## Ambiente Autorizado

Somente uma pasta local criada para o laboratório.

## Ferramentas

- Terminal.
- Comandos `ls`, `chmod`, `cat`, `echo`.

## Passos

1. Crie uma pasta `lab-permissoes`.
2. Entre na pasta.
3. Crie um arquivo `nota.txt`.
4. Escreva uma frase sem informação sensível no arquivo.
5. Execute `ls -l`.
6. Observe permissões de leitura, escrita e execução.
7. Remova permissão de escrita do arquivo para o usuário.
8. Tente editar o arquivo e observe o resultado.
9. Restaure a permissão de escrita.
10. Registre o que cada mudança fez.

## Perguntas de Reflexão

- Qual a diferença entre leitura, escrita e execução?
- Por que permissões incorretas podem gerar risco?
- Quando permissões restritivas ajudam na defesa?

## Resultado Esperado

Você deve conseguir interpretar permissões básicas e entender impacto de mudanças simples.

## Encerramento ou Limpeza

Remova a pasta `lab-permissoes` se não quiser manter os arquivos de teste.

## Limites Éticos e Legais

Não altere permissões de arquivos de sistema ou arquivos de outras pessoas. Use apenas arquivos criados para o laboratório.
