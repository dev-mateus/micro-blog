<p><a href="../README.md"><button>Voltar para o README</button></a></p>

# Tutorial - Passo 16: Criar componentes reutilizaveis

## 1. Objetivo do passo
Criar componentes compartilhados (`Header` e `PostCard`) para reduzir repeticao e padronizar UI.

## 2. O que sera aprendido
- Como transformar blocos repetidos em componentes.
- Como passar props para comportamento dinamico.
- Como reaproveitar regra de exibicao em varias paginas.

## 2.1 Termos deste capitulo (explicacao rapida)
- Componente reutilizavel: bloco de interface usado em mais de uma tela.
- Props: dados enviados para um componente funcionar.


## 3. Por que este passo existe (antes do como)
Sem componentes reutilizaveis:
- cada pagina repete codigo;
- manutencao fica cara;
- mudancas visuais viram retrabalho.

## 4. Codigo necessario
Estrutura:

```txt
src/components/
|-- Header.tsx
`-- PostCard.tsx
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


## 7. Dicas
- Componente reutilizavel e como peca de Lego.
- Padronize props para facilitar reuso.
- Concentre comportamento repetido no mesmo componente.

## 8. Erros comuns
- Copiar e colar blocos iguais em varias paginas.
- Componente com props confusas.
- Nao usar condicoes de exibicao para acoes de dono.

## 9. Checkpoint de aprendizado
- Header e reutilizado em paginas relevantes.
- PostCard exibe dados de forma consistente.
- Acoes de editar/excluir aparecem so para o dono.

## 10. Resumo do capitulo
Voce melhorou manutencao e consistencia visual com componentes reutilizaveis.

<p><a href="../README.md"><button>Voltar para o README</button></a></p>




