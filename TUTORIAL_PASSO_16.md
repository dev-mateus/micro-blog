# Tutorial - Passo 16: Criar componentes reutilizaveis

## 1. Objetivo do passo
Criar componentes compartilhados (`Header` e `PostCard`) para reduzir repeticao e padronizar UI.

## 2. O que sera aprendido
- Como transformar blocos repetidos em componentes.
- Como passar props para comportamento dinamico.
- Como reaproveitar regra de exibicao em varias paginas.

## 3. Por que este passo existe (antes do como)
Sem componentes reutilizaveis:
- cada pagina repete codigo;
- manutencao fica cara;
- mudancas visuais viram retrabalho.

## 4. Codigo necessario
Estrutura:

```txt
src/components/
├── Header.tsx
└── PostCard.tsx
```

Exemplo `Header.tsx`:

```ts
const { isAuthenticated, user, logout } = useAuth()
```

Exemplo `PostCard.tsx`:

```ts
const isOwner = isAuthenticated && user?.profile.username === post.profile.username
```

## 5. Explicacao linha a linha
- `Header`: centraliza navegacao e acoes de sessao.
- `PostCard`: padroniza render do post em feed e perfil.
- `isOwner`: define se botoes Editar/Excluir aparecem.
- `onDeleted`: callback para atualizar lista apos exclusao.

## 6. O que o aluno construiu
Voce construiu uma UI mais organizada, reutilizavel e facil de manter.
