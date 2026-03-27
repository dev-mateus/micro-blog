<p><a href="../README.md"><button>Voltar para o README</button></a></p>

# Tutorial - Passo 3: Inicializar o backend com TypeScript

## 1. Objetivo do passo
Criar a base do backend com Node.js + TypeScript, incluindo dependencias principais e scripts para desenvolvimento.

## 2. O que sera aprendido
- Por que iniciar backend com estrutura minima organizada.
- Como criar o `package.json` do backend.
- Como instalar as bibliotecas essenciais da API.
- Como configurar compilacao TypeScript para ambiente Node.
- Como validar se o backend esta pronto para os proximos passos.

## 3. Por que este passo existe (antes do como)
Se voce tentar implementar rotas e autenticacao sem essa base:
- o projeto nao compila;
- os imports quebram por falta de dependencias;
- voce nao consegue rodar em modo dev e perde produtividade.

Este passo cria um terreno estavel para codar as funcionalidades com seguranca.

## 4. Codigo necessario

### 4.1 Entrar na pasta backend
```bash
cd backend
```

### 4.2 Inicializar o projeto Node
```bash
npm init -y
```

### 4.3 Instalar dependencias de runtime (usadas pela aplicacao)
```bash
npm install fastify @fastify/cors @fastify/jwt @prisma/client bcrypt zod
```

### 4.4 Instalar dependencias de desenvolvimento
```bash
npm install -D typescript tsx prisma @types/node @types/bcrypt
```

### 4.5 Criar arquivo `tsconfig.json`
Use este conteudo:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "lib": ["ES2020"],
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

### 4.6 Ajustar scripts no `package.json`
Deixe assim (mantendo nome/versao do seu projeto):

```json
{
  "scripts": {
    "dev": "tsx watch src/server.ts",
    "build": "tsc",
    "start": "node dist/server.js",
    "db:migrate": "prisma migrate dev",
    "db:generate": "prisma generate"
  }
}
```

### 4.7 Criar estrutura minima de codigo
```bash
mkdir src
```

Crie `src/server.ts` com este codigo inicial:

```ts
import Fastify from 'fastify'

const app = Fastify({ logger: true })

app.get('/health', async () => {
  return { status: 'ok' }
})

app
  .listen({ port: 3333, host: '0.0.0.0' })
  .catch((err) => {
    app.log.error(err)
    process.exit(1)
  })
```

### 4.8 Rodar em modo desenvolvimento
```bash
npm run dev
```

Teste no navegador:
- `http://localhost:3333/health`

Esperado:
```json
{"status":"ok"}
```

## 5. Explicacao linha a linha

### 5.1 `npm init -y`
- Cria `package.json` automaticamente.
- Sem ele, nao ha controle de scripts/dependencias.

### 5.2 Dependencias de runtime
- `fastify`: framework HTTP da API.
- `@fastify/cors`: habilita chamadas do frontend para backend.
- `@fastify/jwt`: suporte a autenticacao JWT.
- `@prisma/client`: cliente de acesso ao banco via Prisma.
- `bcrypt`: hash de senha.
- `zod`: validacao de dados de entrada.

### 5.3 Dependencias de desenvolvimento
- `typescript`: compilador TS.
- `tsx`: executa TypeScript diretamente em desenvolvimento.
- `prisma`: CLI para schema e migracoes.
- `@types/node` e `@types/bcrypt`: tipagens para TypeScript.

### 5.4 `tsconfig.json`
- `target: ES2020`
  - Define recursos JS gerados pela compilacao.
- `module: commonjs`
  - Padrao de modulos para Node neste backend.
- `rootDir: ./src`
  - Diz onde esta o codigo fonte.
- `outDir: ./dist`
  - Diz para onde vai o JS compilado.
- `strict: true`
  - Ativa regras fortes de tipagem (recomendado para qualidade).

### 5.5 Scripts
- `dev`
  - Executa backend com recarga automatica em TS.
- `build`
  - Compila para JavaScript em `dist`.
- `start`
  - Sobe a versao compilada (producao/local validacao).
- `db:migrate` e `db:generate`
  - Preparam comandos de banco que voce usara nos proximos passos.

### 5.6 `src/server.ts`
- `import Fastify from 'fastify'`
  - Importa o framework da API.
- `const app = Fastify({ logger: true })`
  - Cria instancia do servidor com logs ativos.
- `app.get('/health', ...)`
  - Rota de verificacao simples para saber se esta no ar.
- `listen({ port: 3333, host: '0.0.0.0' })`
  - Inicia servidor acessivel localmente.
- `.catch(...)`
  - Trata falha de inicializacao e encerra processo com erro.

## 6. Como este passo se conecta com o proximo
No Passo 4, voce vai configurar o Prisma e modelar o banco (`User`, `Profile`, `Post`).

Sem este Passo 3, nao existe uma aplicacao backend pronta para receber banco, rotas e regras.

## 7. O que o aluno construiu neste passo
Voce construiu o esqueleto funcional do backend:
- projeto Node inicializado;
- TypeScript configurado;
- dependencias principais instaladas;
- servidor Fastify rodando com rota de health;
- base pronta para evoluir para banco e autenticacao.

Em resumo: agora voce tem um backend vivo e preparado para crescer com qualidade.

<p><a href="../README.md"><button>Voltar para o README</button></a></p>

