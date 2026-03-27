<p><a href="../README.md"><button>Voltar para o README</button></a></p>

# Tutorial - Passo 6: Criar estrutura modular por dominio

## 1. Objetivo do passo
Organizar o backend em modulos (`auth`, `profiles`, `posts`) e separar responsabilidades em `routes`, `controller` e `service`.

## 2. O que sera aprendido
- Por que modularizar por dominio.
- Como separar entrada HTTP, validacao e regra de negocio.
- Como criar area compartilhada para utilitarios comuns.

## 2.1 Termos deste capitulo (explicacao rapida)
- Modulo: grupo de arquivos de uma area do sistema.
- Service: camada com regra de negocio.


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
|-- modules/
|   |-- auth/
|   |   |-- auth.routes.ts
|   |   |-- auth.controller.ts
|   |   `-- auth.service.ts
|   |-- profiles/
|   |   |-- profiles.routes.ts
|   |   |-- profiles.controller.ts
|   |   `-- profiles.service.ts
|   `-- posts/
|       |-- posts.routes.ts
|       |-- posts.controller.ts
|       `-- posts.service.ts
`-- shared/
    |-- middlewares/
    `-- prisma/
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


## 7. Dicas
- Imagine modulos como setores de uma empresa: cada um com sua funcao.
- Deixe controller focado em HTTP e service em regra de negocio.
- Mantenha o Prisma client em area compartilhada para evitar duplicacao.

## 8. Erros comuns
- Colocar regra de negocio em rotas.
- Duplicar acesso ao banco em varios arquivos sem padrao.
- Misturar responsabilidades de modulos diferentes.

## 9. Checkpoint de aprendizado
- Estrutura `routes/controller/service` existe em cada modulo.
- Voce sabe onde adicionar nova regra de negocio.
- Existe um ponto unico de acesso ao Prisma.

## 10. Resumo do capitulo
Voce organizou o backend para crescer com clareza e manutencao simples.

<p><a href="../README.md"><button>Voltar para o README</button></a></p>




