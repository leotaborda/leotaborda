# Guia de Padrões de Commit

Este documento estabelece as diretrizes para padronização de mensagens de commit neste repositório, visando manter o histórico de versionamento organizado, legível e consistente.

[![Conventional Commits](https://img.shields.io/badge/Conventional%20Commits-1.0.0-yellow.svg)](https://conventionalcommits.org)
[![SemVer](https://img.shields.io/badge/semver-2.0.0-blue)](https://semver.org/)

---

## Objetivos

- Criar um histórico de commits claro e padronizado
- Facilitar a leitura e compreensão das mudanças realizadas
- Permitir automação em processos como versionamento semântico e geração de changelog
- Garantir que todos os contribuidores sigam o mesmo padrão de comunicação
- Melhorar a rastreabilidade e manutenibilidade do código

---

## Estrutura de um Commit

Cada mensagem de commit deve seguir a seguinte estrutura baseada na especificação **Conventional Commits**:

```
<tipo>[escopo opcional]: <descrição>

[corpo opcional]

[rodapé opcional]
```

### Campos Obrigatórios e Opcionais

| Campo | Status | Descrição | Limite de Caracteres |
|-------|--------|-----------|---------------------|
| **Tipo** | Obrigatório | Identifica a categoria da mudança | - |
| **Escopo** | Opcional | Indica o módulo ou área afetada | - |
| **Descrição** | Obrigatório | Resumo conciso da alteração | 72 |
| **Corpo** | Opcional | Explicação detalhada quando necessário | 72 por linha |
| **Rodapé** | Opcional | Metadados (issues, breaking changes) | - |

---

## Tipos de Commits

### Tipos Principais

| Tipo | Descrição | Exemplo |
|------|-----------|---------|
| `feat` | Nova funcionalidade para o usuário | `feat: adicionar autenticação de dois fatores` |
| `fix` | Correção de bug | `fix: resolver erro de validação de formulário` |
| `docs` | Mudanças na documentação | `docs: atualizar guia de instalação` |
| `style` | Formatação, espaços em branco, etc. | `style: aplicar padrão de formatação ESLint` |
| `refactor` | Refatoração de código sem mudança de funcionalidade | `refactor: simplificar lógica de autenticação` |
| `test` | Adição ou modificação de testes | `test: implementar testes unitários para API` |
| `chore` | Tarefas de manutenção e configuração | `chore: atualizar dependências do projeto` |

### Tipos Complementares

| Tipo | Descrição | Exemplo |
|------|-----------|---------|
| `perf` | Melhorias de performance | `perf: otimizar consultas ao banco de dados` |
| `build` | Mudanças no sistema de build | `build: configurar Webpack para produção` |
| `ci` | Mudanças na integração contínua | `ci: adicionar pipeline de deploy automático` |
| `revert` | Reversão de commits anteriores | `revert: reverter alterações do commit a1b2c3d` |

---

## Exemplos de Implementação

### Commits Simples

```bash
feat: implementar sistema de cache Redis
fix: corrigir memory leak na conexão WebSocket
docs: adicionar documentação da API REST
refactor: extrair validações para módulo separado
test: criar suite de testes de integração
```

### Commits com Escopo

```bash
feat(auth): implementar login via OAuth2
fix(database): resolver timeout em consultas complexas
docs(api): documentar endpoints de usuário
style(frontend): padronizar nomenclatura de componentes
```

### Commit Completo

```bash
feat(payment): integrar gateway de pagamento Stripe

Implementa processamento de pagamentos utilizando Stripe API v3.
Inclui validação de cartões, processamento de webhooks e
tratamento de erros para diferentes cenários de falha.

- Adiciona middleware de validação de pagamentos
- Implementa retry automático para falhas temporárias
- Inclui logging detalhado para auditoria

Resolves: #234, #267
Breaking-change: altera estrutura de resposta da API de pagamentos
```

---

## Boas Práticas

### Diretrizes Gerais

**Formato da Mensagem:**
- Use o modo imperativo na descrição ("adicionar" ao invés de "adicionado")
- Mantenha a primeira linha com no máximo 72 caracteres
- Use letras minúsculas no tipo e descrição
- Não termine a descrição com ponto final

**Conteúdo:**
- Seja específico sobre o que foi alterado
- Explique o "porquê" no corpo quando a mudança não for óbvia
- Faça commits atômicos (uma mudança lógica por commit)
- Referencie issues e pull requests quando relevante

### Breaking Changes

Para mudanças que quebram compatibilidade, use uma das abordagens:

**Opção 1 - Exclamação:**
```bash
feat!: alterar estrutura da API de usuários
```

**Opção 2 - Rodapé:**
```bash
feat: alterar estrutura da API de usuários

BREAKING CHANGE: endpoint /users agora retorna objeto paginado
```

### Referenciando Issues

```bash
fix: corrigir validação de email inválido

Closes: #123
Fixes: #456, #789
Resolves: #101112
```

---

## Exemplos por Cenário

### Desenvolvimento de Features

```bash
feat(user-profile): implementar edição de perfil
feat(dashboard): adicionar gráfico de vendas mensais
feat(api): criar endpoint para relatórios personalizados
```

### Correção de Bugs

```bash
fix(auth): resolver expiração prematura de sessão
fix(database): corrigir constraint de foreign key
fix(frontend): ajustar layout responsivo em dispositivos móveis
```

### Manutenção e Configuração

```bash
chore: atualizar Node.js para versão 18 LTS
chore(deps): bump lodash de 4.17.20 para 4.17.21
build: configurar Docker para ambiente de desenvolvimento
```

### Documentação

```bash
docs: criar guia de contribuição para desenvolvedores
docs(api): adicionar exemplos de uso para webhooks
docs: atualizar README com instruções de deploy
```

---

## Ferramentas de Automação

### Commitizen

Ferramenta para criação interativa de commits padronizados:

```bash
npm install -g commitizen cz-conventional-changelog
echo '{ "path": "cz-conventional-changelog" }' > ~/.czrc
```

Uso: `git cz` ao invés de `git commit`

### Commitlint

Validação automática de mensagens de commit:

```bash
npm install --save-dev @commitlint/config-conventional @commitlint/cli
echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js
```

### Husky

Pre-commit hooks para validação:

```bash
npm install --save-dev husky
npx husky add .husky/commit-msg 'npx --no-install commitlint --edit "$1"'
```

---

## Configuração Recomendada

### package.json

```json
{
  "devDependencies": {
    "@commitlint/cli": "^17.0.0",
    "@commitlint/config-conventional": "^17.0.0",
    "husky": "^8.0.0",
    "commitizen": "^4.2.0",
    "cz-conventional-changelog": "^3.3.0"
  },
  "config": {
    "commitizen": {
      "path": "./node_modules/cz-conventional-changelog"
    }
  }
}
```

### .gitmessage (Template)

```
# <tipo>[escopo opcional]: <descrição>
#
# [corpo opcional]
#
# [rodapé opcional]

# Tipos disponíveis:
# feat     Nova funcionalidade
# fix      Correção de bug
# docs     Documentação
# style    Formatação
# refactor Refatoração
# test     Testes
# chore    Manutenção
```

---

## Validação de Commits

### Checklist de Qualidade

Antes de confirmar um commit, verifique:

- [ ] O tipo está correto para a mudança realizada?
- [ ] A descrição é clara e específica?
- [ ] Está escrita no modo imperativo?
- [ ] Não excede 72 caracteres no título?
- [ ] Issues relacionadas foram referenciadas?
- [ ] Breaking changes estão adequadamente documentadas?
- [ ] O commit representa uma única mudança lógica?

### Padrões a Evitar

```bash
# Evite mensagens genéricas
❌ fix: correções
❌ update: atualizações
❌ changes: mudanças

# Evite commits muito grandes
❌ feat: implementar todo o sistema de autenticação

# Evite modo não imperativo
❌ fixed: corrigido bug de validação
❌ added: adicionada nova feature
```

---

## Integração com Versionamento Semântico

Os tipos de commit se relacionam diretamente com o versionamento semântico:

| Tipo de Commit | Impacto na Versão | Exemplo |
|----------------|------------------|---------|
| `fix` | PATCH (1.0.1) | Correções de bug |
| `feat` | MINOR (1.1.0) | Novas funcionalidades |
| `BREAKING CHANGE` | MAJOR (2.0.0) | Mudanças incompatíveis |

---

## Referências

- [Conventional Commits Specification](https://www.conventionalcommits.org/)
- [Semantic Versioning](https://semver.org/)
- [Angular Commit Message Guidelines](https://github.com/angular/angular/blob/master/CONTRIBUTING.md#commit)
- [Commitizen Documentation](https://commitizen-tools.github.io/commitizen/)
- [Commitlint Documentation](https://commitlint.js.org/)

---

## Conclusão

A adoção consistente destes padrões de commit resulta em:

- **Histórico mais limpo e navegável**
- **Facilita code review e debugging**
- **Permite automação de releases e changelogs**
- **Melhora a comunicação entre desenvolvedores**
- **Facilita a manutenção a longo prazo**

O investimento inicial em padronização se traduz em maior produtividade e qualidade do desenvolvimento ao longo do tempo.