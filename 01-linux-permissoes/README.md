# 01 — Linux Permissões

## Objetivo

Entender permissões básicas de arquivos e diretórios em Linux usando somente itens criados para este laboratório.

## Nível

Iniciante.

## Pré-Requisitos

- Terminal Linux, WSL, VM ou ambiente educacional.
- Comandos básicos: `pwd`, `ls`, `cd`, `mkdir`, `touch`, `cat` e `printf`.
- Leitura de [SAFETY.md](../SAFETY.md).
- Usuário comum, sem privilégios de administrador.

## Ambiente Autorizado

Execute os comandos somente em uma máquina própria e no diretório exclusivo:

```text
/tmp/k0sec-lab-permissoes-linux
```

> **Atenção:** não adapte os comandos para sua pasta pessoal, diretórios com arquivos importantes ou diretórios do sistema. Não use `sudo`. Se o caminho acima já existir, pare: não apague nem reutilize seu conteúdo.

## Ferramentas

- Terminal.
- Comandos `pwd`, `ls`, `mkdir`, `touch`, `printf`, `cat`, `chmod`, `test` e `rm`.

## Conceitos Básicos

Ao executar `ls -l`, o início de cada linha mostra o tipo do item e suas permissões. Por exemplo:

```text
-rw-r-----  ... nota.txt
drwxr-x---  ... arquivos
```

O primeiro caractere indica o tipo: `-` representa um arquivo comum e `d` representa um diretório. Os nove caracteres seguintes formam três grupos:

| Grupo | Exemplo | A quem se aplica |
| --- | --- | --- |
| Usuário (`u`) | `rw-` | Dono do item |
| Grupo (`g`) | `r--` | Integrantes do grupo do item |
| Outros (`o`) | `---` | Demais usuários |

Cada permissão tem um significado e um valor numérico:

| Permissão | Letra | Valor | Em um arquivo | Em um diretório |
| --- | --- | ---: | --- | --- |
| Leitura | `r` | 4 | Ler o conteúdo | Listar os nomes dos itens |
| Escrita | `w` | 2 | Alterar o conteúdo | Criar, renomear ou remover itens |
| Execução | `x` | 1 | Executar um programa ou script | Entrar e acessar itens pelo nome |

Em diretórios, as permissões funcionam em conjunto. Por exemplo, listar nomes com `r` não garante acesso aos itens sem `x`.

## 1. Criar o Diretório de Trabalho

Primeiro, confirme que o caminho reservado não existe:

```bash
test ! -e /tmp/k0sec-lab-permissoes-linux && echo "Caminho livre"
```

Resultado esperado:

```text
Caminho livre
```

Se nada for exibido, pare. O diretório já existe e pode conter dados de outra execução.

Crie o ambiente e entre nele:

```bash
mkdir /tmp/k0sec-lab-permissoes-linux
cd /tmp/k0sec-lab-permissoes-linux
pwd
```

Resultado esperado para `pwd`:

```text
/tmp/k0sec-lab-permissoes-linux
```

### Checkpoint

Continue somente se o caminho exibido for exatamente `/tmp/k0sec-lab-permissoes-linux`.

## 2. Criar Arquivos de Teste

Crie um arquivo, um pequeno script e um diretório:

```bash
printf '%s\n' 'Anotação sem informação sensível.' > nota.txt
printf '%s\n' '#!/bin/sh' 'echo "Script executado"' > saudacao.sh
mkdir arquivos
touch arquivos/exemplo.txt
```

Defina permissões conhecidas para que o resultado não dependa da configuração do sistema:

```bash
chmod 640 nota.txt
chmod 640 saudacao.sh
chmod 750 arquivos
ls -ld nota.txt saudacao.sh arquivos
```

O início das três linhas deve ser:

```text
drwxr-x--- ... arquivos
-rw-r----- ... nota.txt
-rw-r----- ... saudacao.sh
```

Datas, nomes de usuário, grupo e ordem podem variar. Compare principalmente o tipo e as permissões.

### Checkpoint

- `nota.txt` e `saudacao.sh` começam com `-`, pois são arquivos.
- `arquivos` começa com `d`, pois é um diretório.
- Nenhum dos três itens concede permissão a outros usuários.

## 3. Usar `chmod` Simbólico

No modo simbólico, use `u`, `g` e `o` para usuário, grupo e outros; `+` adiciona e `-` remove uma permissão.

Remova a escrita do dono de `nota.txt` e confirme:

```bash
chmod u-w nota.txt
ls -l nota.txt
```

O início da linha deve mudar de `-rw-r-----` para:

```text
-r--r----- ... nota.txt
```

Uma tentativa de acrescentar conteúdo deve falhar com uma mensagem semelhante a `Permission denied`:

```bash
printf '%s\n' 'Tentativa de escrita.' >> nota.txt
```

Restaure a escrita, tente novamente e confirme o conteúdo:

```bash
chmod u+w nota.txt
printf '%s\n' 'Escrita restaurada.' >> nota.txt
cat nota.txt
```

Resultado esperado:

```text
Anotação sem informação sensível.
Escrita restaurada.
```

### Checkpoint

```bash
test -w nota.txt && echo "Escrita restaurada"
```

O resultado esperado é `Escrita restaurada`.

## 4. Entender a Execução

Mesmo com um conteúdo de script válido, `saudacao.sh` ainda não pode ser executado porque não possui `x`:

```bash
./saudacao.sh
```

A tentativa deve falhar com uma mensagem semelhante a `Permission denied`.

Adicione execução somente para o dono:

```bash
chmod u+x saudacao.sh
ls -l saudacao.sh
./saudacao.sh
```

Resultados esperados:

```text
-rwxr----- ... saudacao.sh
Script executado
```

### Checkpoint

```bash
test -x saudacao.sh && echo "Execução liberada para o usuário"
```

## 5. Usar `chmod` Numérico

No modo numérico, some os valores de cada grupo:

- `4` significa leitura.
- `2` significa escrita.
- `1` significa execução.
- `6` é `4 + 2`, ou leitura e escrita.
- `7` é `4 + 2 + 1`, ou leitura, escrita e execução.

Assim, `640` significa:

- Usuário: `6`, leitura e escrita (`rw-`).
- Grupo: `4`, somente leitura (`r--`).
- Outros: `0`, nenhuma permissão (`---`).

Aplique e confirme:

```bash
chmod 640 nota.txt
ls -l nota.txt
```

Resultado esperado:

```text
-rw-r----- ... nota.txt
```

Para o diretório, `750` permite que o dono liste, altere e acesse; que o grupo liste e acesse; e bloqueia outros:

```bash
chmod 750 arquivos
ls -ld arquivos
```

Resultado esperado:

```text
drwxr-x--- ... arquivos
```

## 6. Corrigir uma Permissão Incorreta

Suponha que `604` tenha sido aplicado por engano:

```bash
chmod 604 nota.txt
ls -l nota.txt
```

O início `-rw----r--` revela que outros usuários receberam leitura, enquanto o grupo perdeu essa permissão:

```text
-rw----r-- ... nota.txt
```

Corrija reaplicando o valor pretendido e confirme:

```bash
chmod 640 nota.txt
ls -l nota.txt
```

Resultado esperado:

```text
-rw-r----- ... nota.txt
```

## Erros Comuns

- **Executar em outra pasta:** confirme `pwd` antes de criar ou remover arquivos.
- **Confundir arquivo e diretório:** observe `-` ou `d` no primeiro caractere de `ls -l`.
- **Trocar a ordem dos grupos:** os números representam usuário, grupo e outros, nessa ordem.
- **Esquecer que diretórios precisam de `x`:** sem execução, não é possível entrar no diretório nem acessar seus itens pelo nome.
- **Usar `chmod -R` sem necessidade:** este laboratório altera cada item explicitamente e não usa mudanças recursivas.
- **Tentar resolver com `sudo`:** pare e revise o diretório e o dono dos arquivos; nenhum passo exige privilégios administrativos.
- **Apagar um caminho que já existia:** se o teste inicial não mostrar `Caminho livre`, não prossiga nem execute a limpeza.

## Perguntas de Reflexão

- Qual é a diferença entre leitura, escrita e execução em um arquivo?
- Por que `x` tem um significado diferente em diretórios?
- O que cada algarismo de `640` representa?
- Como `ls -l` ajuda a identificar uma permissão aplicada incorretamente?
- Por que conceder permissões a “outros” pode aumentar o risco?

## Resultado Esperado

Ao final, você deve conseguir:

- Diferenciar arquivo e diretório pela saída de `ls -l`.
- Interpretar permissões de usuário, grupo e outros.
- Alterar permissões com modos simbólico e numérico.
- Confirmar cada alteração antes de continuar.
- Corrigir uma permissão aplicada incorretamente.

## Encerramento ou Limpeza

Primeiro, saia do diretório e confirme o caminho exato que será removido:

```bash
cd /tmp
pwd
ls -la -- /tmp/k0sec-lab-permissoes-linux
```

O `pwd` deve mostrar `/tmp`. O `ls` deve mostrar somente os itens deste laboratório: `nota.txt`, `saudacao.sh` e `arquivos`.

O próximo comando remove recursivamente **apenas** `/tmp/k0sec-lab-permissoes-linux`, que foi criado no primeiro passo. Ele não usa variáveis, curingas, sua pasta pessoal ou diretórios do sistema:

```bash
rm -r -- /tmp/k0sec-lab-permissoes-linux
```

Confirme a limpeza:

```bash
test ! -e /tmp/k0sec-lab-permissoes-linux && echo "Limpeza concluída"
```

Resultado esperado:

```text
Limpeza concluída
```

Nenhum arquivo do usuário fora de `/tmp/k0sec-lab-permissoes-linux` é removido por esses comandos.

## Limites Éticos e Legais

Não altere permissões de arquivos de sistema ou arquivos de outras pessoas. Use apenas arquivos criados para este laboratório, em uma máquina própria ou em um ambiente expressamente autorizado.
