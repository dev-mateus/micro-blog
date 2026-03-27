<p><a href="../README.md"><button>Voltar para o README</button></a></p>

# Tutorial - Passo 4: Configurar Prisma e banco de dados

## 1. Objetivo do passo
Configurar o Prisma no backend, modelar as entidades principais do microblog e aplicar a primeira migracao no PostgreSQL.

## 2. O que sera aprendido
- Por que usar ORM (Prisma) em vez de escrever SQL manual em tudo.
- Como criar o `schema.prisma` com `User`, `Profile` e `Post`.
- Como configurar variaveis de ambiente para conexao com banco.
- Como gerar e aplicar migracoes.
- Como validar se o banco ficou alinhado com o codigo.

## 3. Por que este passo existe (antes do como)
Sem banco modelado corretamente:
- voce nao consegue salvar usuarios e posts;
- as relacoes podem ficar erradas (exemplo: um post sem autor);
- cada nova funcionalidade vira um risco de quebra de dados.

Este passo cria a base de dados do sistema de forma previsivel e versionada.

## 4. Codigo necessario

### 4.1 Entrar na pasta backend
```bash
cd backend
```

### 4.2 Inicializar Prisma
```bash
npx prisma init
```

Esse comando cria:
- pasta `prisma/`
- arquivo `prisma/schema.prisma`
- configuracao inicial de `.env`

### 4.3 Definir variaveis de ambiente
No arquivo `.env`, configure pelo menos:

```env
DATABASE_URL="postgresql://postgres:postgres@localhost:5432/microblog"
JWT_SECRET="dev-secret"
FRONTEND_URL="http://localhost:5173"
PORT=3333
```

Ajuste `DATABASE_URL` conforme seu usuario/senha/host reais.

### 4.4 Criar schema Prisma
Substitua o conteudo de `prisma/schema.prisma` por:

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id        String   @id @default(uuid())
  email     String   @unique
  password  String
  createdAt DateTime @default(now())
  profile   Profile?
}

model Profile {
  id        String   @id @default(uuid())
  username  String   @unique
  name      String
  bio       String?
  userId    String   @unique
  createdAt DateTime @default(now())
  user      User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  posts     Post[]
}

model Post {
  id        String   @id @default(uuid())
  content   String
  profileId String
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  profile   Profile  @relation(fields: [profileId], references: [id], onDelete: Cascade)
}
```

### 4.5 Aplicar migracao
```bash
npx prisma migrate dev --name init
```

### 4.6 Gerar cliente Prisma
```bash
npx prisma generate
```

### 4.7 Validar schema
```bash
npx prisma validate
```

## 5. Explicacao linha a linha

### Bloco: `generator`
- `generator client { provider = "prisma-client-js" }`
  - Diz ao Prisma para gerar o cliente JS/TS que seu backend vai usar.

### Bloco: `datasource`
- `provider = "postgresql"`
  - Define qual banco sera usado.
- `url = env("DATABASE_URL")`
  - Le URL do banco via variavel de ambiente (boa pratica de seguranca).

### Model `User`
- `id` com `@default(uuid())`
  - Cada usuario ganha identificador unico global.
- `email @unique`
  - Nao permite duas contas com mesmo email.
- `password`
  - Guarda hash da senha (nao senha em texto).
- `profile Profile?`
  - Relacao 1:1 com perfil.

### Model `Profile`
- `username @unique`
  - Permite URL publica unica (`/perfil/:username`).
- `bio String?`
  - Campo opcional.
- `userId String @unique`
  - Garante um unico perfil por usuario.
- `user @relation(...)`
  - Define relacao com `User` e comportamento de delecao.
- `posts Post[]`
  - Um perfil pode ter varios posts.

### Model `Post`
- `content String`
  - Conteudo textual do post.
- `profileId String`
  - FK apontando para dono do post.
- `createdAt` e `updatedAt`
  - Auditoria basica de criacao/edicao.
- `profile @relation(...)`
  - Conecta post ao perfil autor.

### Comandos de migracao
- `prisma migrate dev --name init`
  - Cria e aplica migracao no banco local.
- `prisma generate`
  - Gera cliente Prisma para uso no TypeScript.
- `prisma validate`
  - Verifica se schema esta valido.

## 6. Como este passo se conecta com o proximo
No Passo 5, voce vai subir o servidor base com Fastify usando esse banco ja modelado.

Sem este Passo 4, o backend nao teria estrutura de dados para registrar usuarios e posts.

## 7. O que o aluno construiu neste passo
Voce construiu a base persistente do projeto:
- schema de banco versionado;
- relacoes corretas entre usuario, perfil e post;
- migracao aplicada no PostgreSQL;
- cliente Prisma pronto para uso nos services.

Em resumo: agora o projeto saiu de estrutura de codigo para estrutura real de dados.


## 7. Dicas
- Pense no schema como um mapa do banco de dados.
- Nomeie migracoes com clareza para facilitar historico.
- Sempre valide schema e cliente apos alteracoes.

## 8. Erros comuns
- Alterar modelos e esquecer de rodar migracao.
- Relacionamentos inconsistentes entre User, Profile e Post.
- Usar DATABASE_URL incorreta.

## 9. Checkpoint de aprendizado
- Migracao foi aplicada com sucesso.
- Prisma Client foi gerado.
- Tabelas e relacoes principais existem no banco.

## 10. Resumo do capitulo
Voce conectou aplicacao e banco com estrutura consistente, criando base confiavel para as regras de negocio.

<p><a href="../README.md"><button>Voltar para o README</button></a></p>

