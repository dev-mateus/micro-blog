<p><a href="../README.md"><button>Voltar para o README</button></a></p>

# Tutorial - Passo 8: Implementar autorizacao no backend

## 1. Objetivo do passo
Garantir que apenas usuarios autenticados acessem rotas privadas e que somente o dono do post possa editar/excluir.

## 2. O que sera aprendido
- Diferenca entre autenticacao e autorizacao.
- Como usar middleware JWT no Fastify.
- Como validar ownership no service.

## 2.1 Termos deste capitulo (explicacao rapida)
- Autenticacao: confirmar quem e o usuario.
- Autorizacao: definir o que esse usuario pode fazer.


## 3. Por que este passo existe (antes do como)
Mesmo com login funcionando, sem autorizacao correta:
- qualquer usuario pode alterar dados de outro;
- o frontend pode ser burlado com chamadas diretas na API.

A regra de permissao precisa estar no backend, porque o frontend pode ser burlado com chamadas diretas na API.

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
- `request.jwtVerify()`: valida assinatura e expiracao do token.
- `preHandler: [authenticate]`: bloqueia rota sem token valido.
- `post.profileId !== profileId`: confere se o autor do token e o dono do post.

## 6. O que o aluno construiu
Voce construiu a camada de seguranca real do sistema, impedindo acesso indevido por usuarios nao autorizados.


## 7. Dicas
- Autenticacao diz quem e; autorizacao diz o que pode fazer.
- Mantenha checagem de permissao no backend, nao so na interface.
- Reutilize middleware para proteger rotas sensiveis.

## 8. Erros comuns
- Confiar apenas em botoes escondidos no frontend.
- Esquecer de validar ownership no service.
- Tratar `Forbidden` de forma inconsistente.

## 9. Checkpoints de aprendizado
- Rota protegida sem token retorna 401.
- Usuario nao consegue editar/excluir post de outro.
- Fluxo autorizado funciona para o dono do recurso, com resposta de sucesso e dados atualizados.

## 10. Resumo do capitulo
Voce protegeu os recursos criticos com regras corretas de permissao.

<p><a href="../README.md"><button>Voltar para o README</button></a></p>




