<p><a href="../README.md"><button>Voltar para o README</button></a></p>

# Tutorial - Passo 9: Implementar modulo de perfis publicos

## 1. Objetivo do passo
Permitir visualizar perfil publico por `username` e listar os posts desse perfil.

## 2. O que sera aprendido
- Como criar rotas publicas por parametro de URL.
- Como buscar dados com Prisma por username.
- Como paginar resultados de forma previsivel, para evitar respostas muito grandes e manter a navegacao rapida.

## 2.1 Termos deste capitulo (explicacao rapida)
- Username: nome unico publico do perfil.
- Paginacao: dividir lista grande em partes menores.


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


## 7. Dicas
- Username e endereco publico do perfil: mantenha unico e claro.
- Use paginacao para evitar respostas muito grandes.
- Exiba posts mais recentes primeiro para melhor experiencia.

## 8. Erros comuns
- Nao tratar perfil inexistente.
- Esquecer `orderBy` e deixar feed confuso.
- Nao limitar quantidade de itens por pagina.

## 9. Checkpoints de aprendizado
- Perfil por username responde corretamente.
- Lista de posts do perfil funciona com pagina.
- Requisicao para username invalido retorna erro esperado.

## 10. Resumo do capitulo
Voce implementou a parte publica do microblog, permitindo navegar por autores e seus posts.

<p><a href="../README.md"><button>Voltar para o README</button></a></p>




