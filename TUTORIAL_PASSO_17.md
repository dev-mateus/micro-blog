# Tutorial - Passo 17: Integrar ponta a ponta

## 1. Objetivo do passo
Subir banco, backend e frontend ao mesmo tempo e validar o fluxo real do microblog do inicio ao fim.

## 2. O que sera aprendido
- Como organizar 3 processos locais (DB, API e frontend).
- Como evitar erro de diretorio na hora de rodar comandos.
- Como validar jornada completa do usuario.

## 3. Por que este passo existe (antes do como)
Partes isoladas podem parecer corretas, mas a aplicacao real so existe quando tudo conversa junto.

## 4. Codigo necessario

### 4.1 Subir banco
```bash
docker run --name microblog-db -e POSTGRES_PASSWORD=postgres -e POSTGRES_DB=microblog -p 5432:5432 -d postgres
```

Se o container ja existir:

```bash
docker start microblog-db
```

### 4.2 Terminal 1 - Backend
Entre na pasta correta:

```bash
cd backend
```

PowerShell (Windows) para definir variaveis e subir:

```powershell
$env:JWT_SECRET='dev-secret'
$env:FRONTEND_URL='http://localhost:5173'
$env:PORT='3333'
npx prisma migrate dev --name init
npm run dev
```

### 4.3 Terminal 2 - Frontend
Entre na pasta correta:

```bash
cd frontend
npm run dev
```

### 4.4 Validacao rapida de disponibilidade
```bash
curl http://localhost:3333/health
```

Abra no navegador:
- `http://localhost:5173`

### 4.5 Fluxo de validacao funcional
1. Cadastrar usuario
2. Fazer login
3. Criar post
4. Editar post proprio
5. Excluir post proprio
6. Abrir `/perfil/:username`
7. Tentar editar post de outro usuario (deve falhar)

## 5. Explicacao linha a linha
- `docker run/start`: garante banco acessivel em `localhost:5432`.
- `cd backend` e `cd frontend`: evita erro de rodar comando na raiz errada.
- `prisma migrate dev`: garante schema aplicado no banco antes da API.
- backend dev + frontend dev: criam ambiente real de integracao.
- fluxo funcional: valida regra tecnica e regra de negocio.

## 6. O que o aluno construiu
Voce construiu a aplicacao completa em execucao real, com todas as camadas integradas.
