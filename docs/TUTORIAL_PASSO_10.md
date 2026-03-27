<p><a href="../README.md"><button>Voltar para o README</button></a></p>

# Tutorial - Passo 10: Implementar modulo de posts

## 1. Objetivo do passo
Implementar feed publico e CRUD de posts com seguranca, validacao e controle de permissao por dono.

## 2. O que sera aprendido
- Como criar rotas publicas e protegidas de posts.
- Como validar payload com Zod.
- Como retornar status HTTP corretos (201, 204, 403, 404).
- Como aplicar regra de ownership no service.

## 3. Por que este passo existe (antes do como)
Posts sao o coracao do microblog. Se esse modulo estiver errado:
- qualquer usuario altera post de outro;
- feed quebra por consulta mal feita;
- API nao representa as regras de negocio.

## 4. Codigo necessario

### 4.1 Rotas
Arquivo: `src/modules/posts/posts.routes.ts`

```ts
import { FastifyInstance } from 'fastify'
import { authenticate } from '../../shared/middlewares/auth.middleware'
import { postsController } from './posts.controller'

type PostIdParams = { id: string }

export async function postsRoutes(fastify: FastifyInstance) {
  fastify.get('/posts', postsController.getFeed)
  fastify.get<{ Params: PostIdParams }>('/posts/:id', postsController.getById)

  fastify.post('/posts', { preHandler: [authenticate] }, postsController.create)
  fastify.put<{ Params: PostIdParams }>(
    '/posts/:id',
    { preHandler: [authenticate] },
    postsController.update,
  )
  fastify.delete<{ Params: PostIdParams }>(
    '/posts/:id',
    { preHandler: [authenticate] },
    postsController.delete,
  )
}
```

### 4.2 Controller
Arquivo: `src/modules/posts/posts.controller.ts`

```ts
import { FastifyRequest, FastifyReply } from 'fastify'
import { z } from 'zod'
import { PostsService } from './posts.service'

const postBodySchema = z.object({
  content: z.string().min(1, 'Conteudo nao pode ser vazio').max(500, 'Maximo 500 caracteres'),
})

type PostIdParams = { id: string }
type PaginationQuery = { page?: string; limit?: string }

const postsService = new PostsService()

export const postsController = {
  async create(request: FastifyRequest, reply: FastifyReply) {
    const parsed = postBodySchema.safeParse(request.body)
    if (!parsed.success) {
      return reply.status(400).send({ message: 'Dados invalidos', errors: parsed.error.errors })
    }

    const post = await postsService.create(parsed.data.content, request.user.profileId)
    return reply.status(201).send(post)
  },

  async update(request: FastifyRequest<{ Params: PostIdParams }>, reply: FastifyReply) {
    const parsed = postBodySchema.safeParse(request.body)
    if (!parsed.success) {
      return reply.status(400).send({ message: 'Dados invalidos', errors: parsed.error.errors })
    }

    try {
      const post = await postsService.update(request.params.id, parsed.data.content, request.user.profileId)
      return reply.send(post)
    } catch (err: unknown) {
      const message = err instanceof Error ? err.message : 'Erro interno'
      if (message === 'Forbidden') return reply.status(403).send({ message: 'Proibido' })
      if (message === 'Post nao encontrado') return reply.status(404).send({ message })
      return reply.status(500).send({ message })
    }
  },

  async delete(request: FastifyRequest<{ Params: PostIdParams }>, reply: FastifyReply) {
    try {
      await postsService.delete(request.params.id, request.user.profileId)
      return reply.status(204).send()
    } catch (err: unknown) {
      const message = err instanceof Error ? err.message : 'Erro interno'
      if (message === 'Forbidden') return reply.status(403).send({ message: 'Proibido' })
      if (message === 'Post nao encontrado') return reply.status(404).send({ message })
      return reply.status(500).send({ message })
    }
  },

  async getById(request: FastifyRequest<{ Params: PostIdParams }>, reply: FastifyReply) {
    try {
      const post = await postsService.getById(request.params.id)
      return reply.send(post)
    } catch {
      return reply.status(404).send({ message: 'Post nao encontrado' })
    }
  },

  async getFeed(request: FastifyRequest<{ Querystring: PaginationQuery }>, reply: FastifyReply) {
    const page = Math.max(1, parseInt(request.query.page ?? '1', 10))
    const limit = Math.min(50, Math.max(1, parseInt(request.query.limit ?? '10', 10)))
    const result = await postsService.getFeed(page, limit)
    return reply.send(result)
  },
}
```

### 4.3 Service
Arquivo: `src/modules/posts/posts.service.ts`

```ts
import prisma from '../../shared/prisma/client'

const POST_SELECT = {
  id: true,
  content: true,
  createdAt: true,
  updatedAt: true,
  profile: {
    select: { username: true, name: true },
  },
} as const

export class PostsService {
  async create(content: string, profileId: string) {
    return prisma.post.create({ data: { content, profileId }, select: POST_SELECT })
  }

  async update(postId: string, content: string, profileId: string) {
    const post = await prisma.post.findUnique({ where: { id: postId } })
    if (!post) throw new Error('Post nao encontrado')
    if (post.profileId !== profileId) throw new Error('Forbidden')

    return prisma.post.update({
      where: { id: postId },
      data: { content },
      select: POST_SELECT,
    })
  }

  async delete(postId: string, profileId: string) {
    const post = await prisma.post.findUnique({ where: { id: postId } })
    if (!post) throw new Error('Post nao encontrado')
    if (post.profileId !== profileId) throw new Error('Forbidden')

    await prisma.post.delete({ where: { id: postId } })
  }

  async getById(postId: string) {
    const post = await prisma.post.findUnique({ where: { id: postId }, select: POST_SELECT })
    if (!post) throw new Error('Post nao encontrado')
    return post
  }

  async getFeed(page: number, limit: number) {
    const skip = (page - 1) * limit
    const [posts, total] = await Promise.all([
      prisma.post.findMany({ orderBy: { createdAt: 'desc' }, skip, take: limit, select: POST_SELECT }),
      prisma.post.count(),
    ])

    return { posts, total, page, limit }
  }
}
```

## 5. Explicacao linha a linha
- `posts.routes.ts`: define o contrato HTTP do modulo.
- `authenticate` nas rotas de escrita: bloqueia usuario nao logado.
- `postBodySchema`: valida conteudo no controller antes da regra de negocio.
- `PostsService`: concentra regra de ownership e acesso ao banco.
- `if (post.profileId !== profileId)`: garante que so o dono altera/remove.
- `getFeed` com `skip/take`: paginacao simples para feed publico.

## 6. O que o aluno construiu
Voce construiu o modulo central da aplicacao, com feed publico e CRUD seguro de posts.


## 7. Dicas
- CRUD e como um ciclo completo de vida do post.
- Defina status HTTP corretos para facilitar debug no frontend.
- Ownership deve ser checado sempre no backend.

## 8. Erros comuns
- Permitir conteudo vazio.
- Nao diferenciar 403 e 404.
- Atualizar/excluir sem verificar dono.

## 9. Checkpoint de aprendizado
- Criar, editar e excluir post funciona para o dono.
- Visitante consegue ler feed e post publico.
- API retorna 403/404 nos cenarios corretos.

## 10. Resumo do capitulo
Voce concluiu o modulo central do produto com regras de negocio consistentes.

<p><a href="../README.md"><button>Voltar para o README</button></a></p>

