# Tutorial - Passo 18: Resolver erros comuns de projeto real

## 1. Objetivo do passo
Corrigir erros de tipagem e ambiente comuns para manter o projeto estavel.

## 2. O que sera aprendido
- Como corrigir tipagem de rota no Fastify.
- Como corrigir `import.meta.env` no frontend.
- Como eliminar warning de modulo no Vite.
- Como lidar com lock do Prisma no Windows.

## 3. Por que este passo existe (antes do como)
Projetos reais sempre tem ajustes apos integrar tudo. Saber corrigir isso faz parte do desenvolvimento profissional.

## 4. Codigo necessario
### 4.1 Tipagem de params no Fastify (`posts.routes.ts`)

```ts
type PostIdParams = { id: string }
fastify.put<{ Params: PostIdParams }>(...) 
fastify.delete<{ Params: PostIdParams }>(...)
```

### 4.2 Tipagem Vite env (`frontend/src/vite-env.d.ts`)

```ts
/// <reference types="vite/client" />
```

### 4.3 Padronizar modulo ESM (`frontend/package.json`)

```json
{
  "type": "module"
}
```

### 4.4 Lock Prisma (EPERM)
- Pare processos que usam Prisma Client (backend dev).
- Rode novamente:

```bash
npx prisma migrate dev --name init
```

### 4.5 Execucao com variaveis de ambiente no backend
Se o backend nao carregar `.env` automaticamente, execute com variaveis na sessao:

```powershell
$env:JWT_SECRET='dev-secret'
$env:FRONTEND_URL='http://localhost:5173'
$env:PORT='3333'
npm run dev
```

## 5. Explicacao linha a linha
- tipagem `Params`: alinha handler com contrato da rota.
- `vite/client`: adiciona tipos para `import.meta.env`.
- `type: module`: evita warning de modulo em configs JS.
- parar backend durante migrate: libera arquivo nativo do engine.
- definir env na sessao: evita falha por `JWT_SECRET` ausente.

## 6. O que o aluno construiu
Voce construiu resiliência tecnica: projeto compila limpo e roda sem erros comuns de setup.
