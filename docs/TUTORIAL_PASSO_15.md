<p><a href="../README.md"><button>Voltar para o README</button></a></p>

# Tutorial - Passo 15: Construir paginas principais

## 1. Objetivo do passo
Implementar as paginas do app: Home, Login, Register, Profile, CreatePost e EditPost.

## 2. O que sera aprendido
- Como montar fluxo completo de UX.
- Como consumir services dentro das paginas.
- Como tratar loading, erro e sucesso.

## 3. Por que este passo existe (antes do como)
Sem paginas, nao existe produto para o usuario final. E nelas que regras viram experiencia real.

## 4. Codigo necessario
Pasta de paginas:

```txt
src/pages/
â”œâ”€â”€ Home.tsx
â”œâ”€â”€ Login.tsx
â”œâ”€â”€ Register.tsx
â”œâ”€â”€ Profile.tsx
â”œâ”€â”€ CreatePost.tsx
â””â”€â”€ EditPost.tsx
```

Exemplo de fluxo em `Login.tsx`:

```ts
const data = await authService.login({ email, password })
login(data.user, data.token)
navigate('/')
```

Exemplo de fluxo em `Home.tsx`:

```ts
const data = await postsService.getFeed(page, 10)
setPosts(data.posts)
setTotal(data.total)
```

## 5. Explicacao linha a linha
- `Login`: autentica e salva sessao no contexto.
- `Register`: cadastra usuario/perfil.
- `Home`: mostra feed publico com paginaÃ§ao.
- `Profile`: mostra perfil e posts por username.
- `CreatePost`: cria post (rota protegida).
- `EditPost`: atualiza post existente com regra de dono.

## 6. O que o aluno construiu
Voce construiu a experiencia principal do microblog de ponta a ponta no frontend.

<p><a href="../README.md"><button>Voltar para o README</button></a></p>

