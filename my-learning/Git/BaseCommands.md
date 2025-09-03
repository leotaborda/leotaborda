# Comandos Básicos do Git

Guia de referência rápida para os comandos Git mais utilizados no desenvolvimento de software. Este documento serve como cola de consulta para operações essenciais de controle de versão.

---

## Inicialização e Clonagem

### git init
Inicializa um novo repositório Git local.

```bash
# Criar repositório no diretório atual
git init

# Criar repositório em novo diretório
git init nome-do-projeto

# Criar repositório bare (sem working directory)
git init --bare
```

**Quando usar:** Ao começar um projeto novo do zero ou transformar um diretório existente em repositório Git.

### git clone
Cria uma cópia local de um repositório remoto.

```bash
# Clonar repositório
git clone https://github.com/usuario/repositorio.git

# Clonar com nome personalizado
git clone https://github.com/usuario/repositorio.git meu-projeto

# Clonar apenas último commit (shallow clone)
git clone --depth 1 https://github.com/usuario/repositorio.git

# Clonar branch específica
git clone -b develop https://github.com/usuario/repositorio.git
```

**Quando usar:** Para trabalhar em um projeto existente ou criar uma cópia local de repositório remoto.

---

## Área de Staging e Commits

### git add
Adiciona arquivos à área de staging (preparação para commit).

```bash
# Adicionar arquivo específico
git add arquivo.txt

# Adicionar vários arquivos
git add arquivo1.txt arquivo2.js

# Adicionar todos os arquivos modificados
git add .
git add -A

# Adicionar apenas arquivos modificados (não novos)
git add -u

# Adicionar interativamente
git add -i

# Adicionar partes específicas de um arquivo
git add -p arquivo.txt
```

**Área de Staging:** Local temporário onde ficam as mudanças preparadas para o próximo commit.

### git commit
Salva as mudanças da área de staging no histórico do repositório.

```bash
# Commit com mensagem inline
git commit -m "feat: adicionar funcionalidade de login"

# Commit com editor para mensagem detalhada
git commit

# Commit pulando o staging (apenas arquivos já rastreados)
git commit -am "fix: corrigir bug de validação"

# Alterar último commit (não publicado)
git commit --amend

# Commit sem executar hooks
git commit --no-verify -m "docs: atualizar README"
```

**Boa prática:** Use mensagens descritivas seguindo padrões como Conventional Commits.

---

## Sincronização com Repositórios Remotos

### git push
Envia commits locais para o repositório remoto.

```bash
# Push para branch atual
git push

# Push especificando remote e branch
git push origin main

# Push primeira vez (configurar upstream)
git push -u origin feature/nova-funcionalidade

# Push forçado (cuidado!)
git push --force

# Push forçado mais seguro
git push --force-with-lease

# Push de todas as branches
git push --all

# Push incluindo tags
git push --tags
```

**Upstream:** Branch remota que está vinculada à sua branch local.

### git pull
Baixa e integra mudanças do repositório remoto.

```bash
# Pull da branch atual
git pull

# Pull especificando remote e branch
git pull origin main

# Pull com rebase (ao invés de merge)
git pull --rebase

# Pull apenas se for fast-forward
git pull --ff-only
```

### git fetch vs git pull

| Comando | O que faz | Quando usar |
|---------|-----------|-------------|
| `git fetch` | Baixa mudanças sem integrar | Verificar mudanças antes de integrar |
| `git pull` | Baixa e integra mudanças | Sincronizar branch local com remota |

```bash
# git fetch - apenas baixa
git fetch origin
git fetch origin main

# Verificar diferenças após fetch
git log HEAD..origin/main
git diff HEAD origin/main

# git pull = git fetch + git merge
git pull origin main
# É equivalente a:
git fetch origin main
git merge origin/main
```

**Vantagem do fetch:** Permite revisar mudanças antes de integrá-las ao código local.

---

## Informações e Status

### git status
Mostra o estado atual do repositório.

```bash
# Status completo
git status

# Status resumido
git status -s
git status --short

# Status sem arquivos não rastreados
git status -uno
```

**Interpretação da saída:**
```bash
On branch main                    # Branch atual
Your branch is up to date...      # Status em relação ao remote

Changes to be committed:          # Arquivos na staging area
  modified:   arquivo.txt

Changes not staged for commit:    # Arquivos modificados não staged
  modified:   outro.txt

Untracked files:                  # Arquivos não rastreados
  novo-arquivo.txt
```

### git log
Exibe histórico de commits.

```bash
# Log padrão
git log

# Log condensado (uma linha por commit)
git log --oneline

# Log com gráfico de branches
git log --graph --oneline

# Log com estatísticas de arquivos
git log --stat

# Log dos últimos N commits
git log -n 5
git log --max-count=5

# Log por período
git log --since="2 weeks ago"
git log --until="2024-01-01"

# Log por autor
git log --author="Nome do Autor"

# Log formatado personalizado
git log --pretty=format:"%h - %an, %ar: %s"

# Log de uma branch específica
git log feature/nova-funcionalidade

# Log entre commits
git log commit1..commit2
```

**Formato personalizado útil:**
```bash
# Alias recomendado
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

### git diff
Mostra diferenças entre estados do repositório.

```bash
# Diferenças não staged (working directory vs staging)
git diff

# Diferenças staged (staging vs último commit)
git diff --cached
git diff --staged

# Diferenças entre working directory e último commit
git diff HEAD

# Diferenças entre commits
git diff commit1 commit2

# Diferenças em arquivo específico
git diff arquivo.txt
git diff HEAD -- arquivo.txt

# Diferenças entre branches
git diff main..feature-branch

# Estatísticas resumidas
git diff --stat

# Ignorar mudanças de espaço em branco
git diff --ignore-space-change
```

---

## Combinações Úteis de Comandos

### Workflow Básico
```bash
# 1. Verificar status
git status

# 2. Adicionar mudanças
git add .

# 3. Verificar o que será commitado
git diff --cached

# 4. Fazer commit
git commit -m "feat: implementar nova funcionalidade"

# 5. Enviar para remoto
git push
```

### Sincronização com Remoto
```bash
# 1. Verificar mudanças remotas
git fetch

# 2. Ver diferenças
git log HEAD..origin/main

# 3. Integrar mudanças
git pull

# 4. Continuar desenvolvimento
git status
```

### Verificação Antes de Push
```bash
# 1. Ver últimos commits
git log --oneline -5

# 2. Verificar diferenças com remoto
git diff origin/main

# 3. Fazer push
git push
```

---

## Comandos para Troubleshooting

### Desfazer Mudanças
```bash
# Desfazer mudanças não staged
git checkout -- arquivo.txt
git restore arquivo.txt

# Remover arquivo da staging area
git reset arquivo.txt
git restore --staged arquivo.txt

# Desfazer último commit (mantém mudanças)
git reset --soft HEAD~1

# Desfazer último commit (remove mudanças)
git reset --hard HEAD~1
```

### Informações Detalhadas
```bash
# Ver configurações
git config --list

# Ver remotes configurados
git remote -v

# Ver branches locais e remotas
git branch -a

# Ver último commit
git show

# Ver informações de commit específico
git show commit-hash
```

---

## Configurações Úteis

### Configurações Iniciais
```bash
# Configurar identidade
git config --global user.name "Seu Nome"
git config --global user.email "seu.email@example.com"

# Configurar editor padrão
git config --global core.editor "code --wait"

# Configurar merge tool
git config --global merge.tool vimdiff
```

### Aliases Recomendados
```bash
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual '!gitk'
```

---

## Referência Rápida

### Fluxo de Estados
```
Working Directory → Staging Area → Repository → Remote
       ↓                ↓              ↓          ↓
   git add         git commit     git push    
       ↑                ↑              ↑          ↑
  git restore    git restore    git pull    git fetch
                   --staged
```

### Comandos por Categoria

**Inicialização:**
- `git init` - Criar repositório
- `git clone` - Clonar repositório

**Mudanças Locais:**
- `git add` - Staged changes
- `git commit` - Salvar mudanças
- `git status` - Ver status
- `git diff` - Ver diferenças

**Histórico:**
- `git log` - Ver commits
- `git show` - Detalhes do commit

**Sincronização:**
- `git fetch` - Baixar mudanças
- `git pull` - Baixar e integrar
- `git push` - Enviar mudanças

### Parâmetros Importantes

| Parâmetro | Descrição | Exemplo |
|-----------|-----------|---------|
| `--all` | Todos os arquivos/branches | `git add --all` |
| `--cached` | Área de staging | `git diff --cached` |
| `--oneline` | Formato condensado | `git log --oneline` |
| `--graph` | Visualização gráfica | `git log --graph` |
| `--stat` | Estatísticas | `git log --stat` |
| `-u` | Configurar upstream | `git push -u origin main` |
| `-f` | Forçar operação | `git push -f` |

---

## Boas Práticas

### Commits
- Faça commits pequenos e focados
- Use mensagens descritivas
- Teste antes de fazer commit
- Evite commit de arquivos temporários

### Sincronização
- Sempre `git pull` antes de `git push`
- Use `git fetch` para verificar mudanças
- Resolva conflitos cuidadosamente
- Faça backup antes de operações destrutivas

### Organização
- Use `.gitignore` para excluir arquivos desnecessários
- Mantenha histórico limpo
- Configure aliases para comandos frequentes
- Documente processos específicos do projeto

---

Este guia cobre os comandos Git essenciais para desenvolvimento diário. Para operações mais avançadas, consulte a documentação oficial do Git ou guias especializados em branching, merging e workflows específicos.