<p><a href="../README.md"><button>Voltar para o README</button></a></p>

# Tutorial - Passo 6: Criar estrutura modular por dominio

## 1. Objetivo do passo
Organizar o backend em modulos (`auth`, `profiles`, `posts`) e separar responsabilidades em `routes`, `controller` e `service`.

## 2. O que sera aprendido
- Por que modularizar por dominio.
- Como separar entrada HTTP, validacao e regra de negocio.
- Como criar area compartilhada para utilitarios comuns.

## 3. Por que este passo existe (antes do como)
Quando tudo fica em um arquivo:
- o projeto cresce e fica dificil de manter;
- bugs aumentam por acoplamento;
- testes e evolucao ficam lentos.

Modularizacao melhora leitura e manutencao.

## 4. Codigo necessario
Criar estrutura:

```txt
src/
â”œâ”€â”€ modules/
â”‚   â”œâ”€â”€ auth/
â”‚   â”‚   â”œâ”€â”€ auth.routes.ts
â”‚   â”‚   â”œâ”€â”€ auth.controller.ts
â”‚   â”‚   â””â”€â”€ auth.service.ts
â”‚   â”œâ”€â”€ profiles/
â”‚   â”‚   â”œâ”€â”€ profiles.routes.ts
â”‚   â”‚   â”œâ”€â”€ profiles.controller.ts
â”‚   â”‚   â””â”€â”€ profiles.service.ts
â”‚   â””â”€â”€ posts/
â”‚       â”œâ”€â”€ posts.routes.ts
â”‚       â”œâ”€â”€ posts.controller.ts
â”‚       â””â”€â”€ posts.service.ts
â””â”€â”€ shared/
    â”œâ”€â”€ middlewares/
    â””â”€â”€ prisma/
```

Arquivo compartilhado do Prisma (`src/shared/prisma/client.ts`):

```ts
import { PrismaClient } from '@prisma/client'

const prisma = new PrismaClient()

export default prisma
```

## 5. Explicacao linha a linha
- `routes`: define endpoint e metodo HTTP.
- `controller`: recebe request/reply e chama service.
- `service`: implementa regra de negocio e acesso ao banco.
- `shared/prisma/client.ts`: uma unica instancia de Prisma no projeto.

## 6. O que o aluno construiu
Voce construiu a arquitetura base do backend para escalar sem virar codigo monolitico desorganizado.

<p><a href="../README.md"><button>Voltar para o README</button></a></p>

