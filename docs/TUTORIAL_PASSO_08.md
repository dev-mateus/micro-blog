<p><a href="../README.md"><button>Voltar para o README</button></a></p>

# Tutorial - Passo 8: Implementar autorizacao no backend

## 1. Objetivo do passo
Garantir que apenas usuarios autenticados acessem rotas privadas e que somente o dono do post possa editar/excluir.

## 2. O que sera aprendido
- Diferenca entre autenticacao e autorizacao.
- Como usar middleware JWT no Fastify.
- Como validar ownership no service.

## 3. Por que este passo existe (antes do como)
Mesmo com login funcionando, sem autorizacao correta:
- qualquer usuario pode alterar dados de outro;
- o frontend pode ser burlado com chamadas diretas na API.

Regra de permissao precisa estar no backend.

## 4. Codigo necessario
Middleware (`src/shared/middlewares/auth.middleware.ts`):

```ts
import { FastifyRequest, FastifyReply } from 'fastify'

export async function authenticate(request: FastifyRequest, reply: FastifyReply) {
  try {
    await request.jwtVerify()
  } catch {
    reply.status(401).send({ message: 'Nao autorizado' })
  }
}
```

Uso em rota protegida:

```ts
fastify.post('/posts', { preHandler: [authenticate] }, postsController.create)
```

Validacao de dono no service:

```ts
if (post.profileId !== profileId) throw new Error('Forbidden')
```

## 5. Explicacao linha a linha
- `request.jwtVerify()`: valida assinatura e expiraÃ§ao do token.
- `preHandler: [authenticate]`: bloqueia rota sem token valido.
- `post.profileId !== profileId`: confere se o autor do token e o dono do post.

## 6. O que o aluno construiu
Voce construiu a camada de seguranca real do sistema, impedindo acesso indevido por usuarios nao autorizados.

<p><a href="../README.md"><button>Voltar para o README</button></a></p>

