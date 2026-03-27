<p><a href="../README.md"><button>Voltar para o README</button></a></p>

# Tutorial - Passo 9: Implementar modulo de perfis publicos

## 1. Objetivo do passo
Permitir visualizar perfil publico por `username` e listar os posts desse perfil.

## 2. O que sera aprendido
- Como criar rotas publicas por parametro de URL.
- Como buscar dados com Prisma por username.
- Como paginar resultados basicos.

## 3. Por que este passo existe (antes do como)
Microblog precisa de identidade publica. Sem perfil publico:
- nao existe pagina do autor;
- visitante nao consegue navegar por criador.

## 4. Codigo necessario
Rotas (`profiles.routes.ts`):

```ts
fastify.get('/profiles/:username', profilesController.getByUsername)
fastify.get('/profiles/:username/posts', profilesController.getPostsByUsername)
```

Service (trecho):

```ts
const profile = await prisma.profile.findUnique({ where: { username } })

const posts = await prisma.post.findMany({
  where: { profileId: profile.id },
  orderBy: { createdAt: 'desc' },
  skip,
  take: limit,
})
```

## 5. Explicacao linha a linha
- `:username`: parametro dinamico da URL.
- `findUnique({ where: { username } })`: busca perfil unico.
- `findMany(... skip/take ...)`: pagina resultados.
- `orderBy createdAt desc`: mostra mais recentes primeiro.

## 6. O que o aluno construiu
Voce construiu a camada publica de perfis, essencial para navegacao aberta do microblog.

<p><a href="../README.md"><button>Voltar para o README</button></a></p>

