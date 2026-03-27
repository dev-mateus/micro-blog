<p><a href="../README.md"><button>Voltar para o README</button></a></p>

# Tutorial - Passo 5: Subir servidor base do backend

## 1. Objetivo do passo
Subir um servidor Fastify funcional com CORS, JWT, rota de health e registro dos modulos principais.

## 2. O que sera aprendido
- Por que criar um servidor central antes de implementar tudo.
- Como configurar CORS e JWT no Fastify.
- Como registrar rotas dos modulos (`auth`, `profiles`, `posts`).
- Como executar o backend com variaveis de ambiente corretamente.

## 2.1 Termos deste capitulo (explicacao rapida)
- CORS: regra de quem pode chamar sua API no navegador.
- JWT: token que funciona como cracha digital do usuario.


## 3. Por que este passo existe (antes do como)
Sem um `server.ts` bem definido:
- cada modulo fica "solto";
- as rotas nao ficam acessiveis;
- voce nao consegue testar a API inteira de forma integrada.

Esse passo cria o ponto de entrada oficial do backend.

## 4. Codigo necessario

Arquivo: `backend/src/server.ts`

```ts
import Fastify from 'fastify'
import cors from '@fastify/cors'
import jwt from '@fastify/jwt'
import { authRoutes } from './modules/auth/auth.routes'
import { profilesRoutes } from './modules/profiles/profiles.routes'
import { postsRoutes } from './modules/posts/posts.routes'

if (!process.env.JWT_SECRET) {
  console.error('FATAL: JWT_SECRET nao esta definido nas variaveis de ambiente')
  process.exit(1)
}

const fastify = Fastify({ logger: true })

async function start() {
  await fastify.register(cors, {
    origin: process.env.FRONTEND_URL || 'http://localhost:5173',
    credentials: true,
  })

  await fastify.register(jwt, {
    secret: process.env.JWT_SECRET as string,
  })

  await fastify.register(authRoutes)
  await fastify.register(profilesRoutes)
  await fastify.register(postsRoutes)

  fastify.get('/health', async () => ({ status: 'ok' }))

  const port = parseInt(process.env.PORT || '3333', 10)
  await fastify.listen({ port, host: '0.0.0.0' })
}

start().catch((err) => {
  console.error(err)
  process.exit(1)
})
```

Execucao no Windows PowerShell (sem dotenv):

```powershell
$env:JWT_SECRET='dev-secret'
$env:FRONTEND_URL='http://localhost:5173'
$env:PORT='3333'
npm run dev
```

Teste:

```bash
curl http://localhost:3333/health
```

Resposta esperada:

```json
{"status":"ok"}
```

## 5. Explicacao linha a linha
- `import { authRoutes ... }`: traz os modulos para dentro do servidor central.
- `if (!process.env.JWT_SECRET) ...`: impede subir API insegura sem segredo JWT.
- `Fastify({ logger: true })`: ativa logs uteis para debug.
- `register(cors, ...)`: libera chamadas do frontend para o backend.
- `register(jwt, ...)`: adiciona suporte a assinatura/verificacao de token.
- `register(authRoutes/profilesRoutes/postsRoutes)`: publica os endpoints desses dominos.
- `get('/health', ...)`: endpoint rapido de monitoramento.
- `listen(...)`: sobe servidor na porta configurada.
- `catch(...)`: evita erro silencioso de inicializacao.

## 6. O que o aluno construiu
Voce construiu o "nucleo" do backend: um servidor que sobe, responde e conecta todos os modulos da API.


## 7. Dicas
- A rota de health e seu termometro rapido da API.
- Carregue variaveis de ambiente antes de subir o servidor.
- Registre modulos no server logo no inicio do projeto.

## 8. Erros comuns
- Subir sem JWT_SECRET configurado.
- Esquecer de registrar uma rota/modulo no server.
- Configurar CORS com origem incorreta.

## 9. Checkpoint de aprendizado
- `GET /health` responde `status: ok`.
- Servidor sobe sem falha de ambiente.
- Rotas principais estao registradas no arquivo central.

## 10. Resumo do capitulo
Voce colocou o backend no ar com configuracao minima, segura e pronta para evoluir por modulos.

<p><a href="../README.md"><button>Voltar para o README</button></a></p>




