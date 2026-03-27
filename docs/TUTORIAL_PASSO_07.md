<p><a href="../README.md"><button>Voltar para o README</button></a></p>

# Tutorial - Passo 7: Implementar autenticacao

## 1. Objetivo do passo
Implementar cadastro e login com senha segura (bcrypt), validacao de entrada e emissao de JWT.

## 2. O que sera aprendido
- Como dividir autenticacao em routes, controller e service.
- Como hashear senha no cadastro.
- Como validar credenciais no login.
- Como gerar token JWT com dados do usuario.
- Como tipar `request.user` no Fastify para TypeScript.

## 3. Por que este passo existe (antes do como)
Autenticacao e o portao de acesso do sistema. Sem ela:
- nao ha identidade de usuario;
- nao ha como aplicar regras de permissao;
- dados sensiveis ficam expostos.

## 4. Codigo necessario

### 4.1 Tipagem JWT para Fastify
Arquivo: `src/@types/fastify-jwt.d.ts`

```ts
import '@fastify/jwt'

declare module '@fastify/jwt' {
  interface FastifyJWT {
    payload: {
      userId: string
      profileId: string
      username: string
    }
    user: {
      userId: string
      profileId: string
      username: string
    }
  }
}
```

### 4.2 Rotas
Arquivo: `src/modules/auth/auth.routes.ts`

```ts
import { FastifyInstance } from 'fastify'
import { authController } from './auth.controller'

export async function authRoutes(fastify: FastifyInstance) {
  const controller = authController(fastify)

  fastify.post('/auth/register', controller.register)
  fastify.post('/auth/login', controller.login)
}
```

### 4.3 Controller
Arquivo: `src/modules/auth/auth.controller.ts`

```ts
import { FastifyRequest, FastifyReply, FastifyInstance } from 'fastify'
import { z } from 'zod'
import { AuthService } from './auth.service'

const registerSchema = z.object({
  email: z.string().email('E-mail invalido'),
  password: z.string().min(6, 'Senha deve ter ao menos 6 caracteres'),
  username: z.string().min(3).max(30).regex(/^[a-zA-Z0-9_]+$/),
  name: z.string().min(1).max(100),
  bio: z.string().max(300).optional(),
})

const loginSchema = z.object({
  email: z.string().email('E-mail invalido'),
  password: z.string().min(1, 'Senha obrigatoria'),
})

const authService = new AuthService()

export function authController(fastify: FastifyInstance) {
  return {
    async register(request: FastifyRequest, reply: FastifyReply) {
      const parsed = registerSchema.safeParse(request.body)
      if (!parsed.success) {
        return reply.status(400).send({ message: 'Dados invalidos', errors: parsed.error.errors })
      }

      try {
        const result = await authService.register(parsed.data)
        return reply.status(201).send(result)
      } catch (err: unknown) {
        const message = err instanceof Error ? err.message : 'Erro interno'
        return reply.status(409).send({ message })
      }
    },

    async login(request: FastifyRequest, reply: FastifyReply) {
      const parsed = loginSchema.safeParse(request.body)
      if (!parsed.success) {
        return reply.status(400).send({ message: 'Dados invalidos', errors: parsed.error.errors })
      }

      try {
        const result = await authService.login(parsed.data, fastify)
        return reply.status(200).send(result)
      } catch (err: unknown) {
        const message = err instanceof Error ? err.message : 'Erro interno'
        return reply.status(401).send({ message })
      }
    },
  }
}
```

### 4.4 Service
Arquivo: `src/modules/auth/auth.service.ts`

```ts
import bcrypt from 'bcrypt'
import { FastifyInstance } from 'fastify'
import prisma from '../../shared/prisma/client'

interface RegisterInput {
  email: string
  password: string
  username: string
  name: string
  bio?: string
}

interface LoginInput {
  email: string
  password: string
}

export class AuthService {
  async register(data: RegisterInput) {
    const existingUser = await prisma.user.findUnique({ where: { email: data.email } })
    if (existingUser) throw new Error('Email ja cadastrado')

    const existingProfile = await prisma.profile.findUnique({ where: { username: data.username } })
    if (existingProfile) throw new Error('Username ja esta em uso')

    const hashedPassword = await bcrypt.hash(data.password, 10)

    const user = await prisma.user.create({
      data: {
        email: data.email,
        password: hashedPassword,
        profile: {
          create: {
            username: data.username,
            name: data.name,
            bio: data.bio ?? null,
          },
        },
      },
      include: { profile: true },
    })

    return {
      id: user.id,
      email: user.email,
      profile: user.profile
        ? {
            id: user.profile.id,
            username: user.profile.username,
            name: user.profile.name,
            bio: user.profile.bio,
          }
        : null,
    }
  }

  async login(data: LoginInput, fastify: FastifyInstance) {
    const user = await prisma.user.findUnique({
      where: { email: data.email },
      include: { profile: true },
    })

    if (!user || !user.profile) throw new Error('Credenciais invalidas')

    const passwordMatch = await bcrypt.compare(data.password, user.password)
    if (!passwordMatch) throw new Error('Credenciais invalidas')

    const token = fastify.jwt.sign({
      userId: user.id,
      profileId: user.profile.id,
      username: user.profile.username,
    })

    return {
      token,
      user: {
        id: user.id,
        email: user.email,
        profile: {
          id: user.profile.id,
          username: user.profile.username,
          name: user.profile.name,
          bio: user.profile.bio,
        },
      },
    }
  }
}
```

## 5. Explicacao linha a linha
- `registerSchema/loginSchema`: valida formato antes da regra de negocio.
- `AuthService`: concentra regra, evitando logica pesada no controller.
- `bcrypt.hash`: protege senha no banco.
- `bcrypt.compare`: compara senha digitada com hash salvo.
- `jwt.sign`: cria token com identificacao do usuario.
- `fastify-jwt.d.ts`: permite TypeScript reconhecer `request.user` nas rotas protegidas.

## 6. O que o aluno construiu
Voce construiu autenticacao completa e segura: cadastro, login e token JWT prontos para autorizar acoes privadas.


## 7. Dicas
- Pense no login como portaria: so entra quem prova identidade.
- Valide entrada com zod para proteger seu backend.
- Nunca salve senha em texto puro.

## 8. Erros comuns
- Nao hashear senha antes de salvar.
- Retornar erros genericos sem contexto para o frontend.
- Esquecer de tipar `request.user` para o Fastify.

## 9. Checkpoint de aprendizado
- Registro cria usuario e perfil.
- Login retorna token JWT valido.
- Erros de validacao respondem com status apropriado.

## 10. Resumo do capitulo
Voce implementou autenticacao segura com cadastro, login e emissao de token.

<p><a href="../README.md"><button>Voltar para o README</button></a></p>

