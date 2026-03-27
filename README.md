# Roteiro de aprendizado: Microblog Full Stack do zero ao projeto final

## Objetivo final
Construir um microblog completo com:
1. Backend em Node.js + Fastify + TypeScript + Prisma + PostgreSQL + JWT
2. Frontend em React + Vite + TypeScript + Tailwind + React Router + Axios
3. Fluxo completo de cadastro, login, perfil público e CRUD de posts
4. Organização modular e pronta para evolução/deploy

## Etapa 1: Entender o produto antes de codar
Tutorial detalhado: [TUTORIAL_PASSO_01.md](docs/TUTORIAL_PASSO_01.md)
1. Defina o que o sistema faz e o que não faz.
2. Regras principais:
1. Visitante vê feed e perfis públicos
2. Usuário logado cria/edita/exclui apenas os próprios posts
3. Perfil público por username único
3. Resultado esperado:
1. Documento simples com requisitos e regras de negócio

## Etapa 2: Preparar ambiente de desenvolvimento
Tutorial detalhado: [TUTORIAL_PASSO_02.md](docs/TUTORIAL_PASSO_02.md)
1. Instalar Node.js
2. Instalar PostgreSQL ou Docker
3. Instalar VS Code e extensão Prisma (opcional)
4. Criar pasta raiz com duas subpastas:
1. backend
2. frontend

## Etapa 3: Inicializar backend com TypeScript
Tutorial detalhado: [TUTORIAL_PASSO_03.md](docs/TUTORIAL_PASSO_03.md)
1. Criar package do backend
2. Instalar Fastify, Prisma, bcrypt, JWT, zod
3. Configurar tsconfig
4. Criar script de desenvolvimento e build

## Etapa 4: Configurar Prisma e banco
Tutorial detalhado: [TUTORIAL_PASSO_04.md](docs/TUTORIAL_PASSO_04.md)
1. Criar schema com modelos:
1. User
2. Profile
3. Post
2. Definir relacionamentos:
1. User 1:1 Profile
2. Profile 1:N Post
3. Criar arquivo de ambiente com DATABASE_URL
4. Rodar migração

## Etapa 5: Subir servidor base do backend
Tutorial detalhado: [TUTORIAL_PASSO_05.md](docs/TUTORIAL_PASSO_05.md)
1. Criar server principal
2. Configurar CORS
3. Configurar JWT
4. Criar rota de health

## Etapa 6: Criar estrutura modular por domínio
Tutorial detalhado: [TUTORIAL_PASSO_06.md](docs/TUTORIAL_PASSO_06.md)
1. Criar módulos:
1. auth
2. profiles
3. posts
2. Em cada módulo, separar:
1. routes
2. controller
3. service
3. Criar shared para middleware e prisma client

## Etapa 7: Implementar autenticação
Tutorial detalhado: [TUTORIAL_PASSO_07.md](docs/TUTORIAL_PASSO_07.md)
1. Registro:
1. Validar dados
2. Verificar email e username únicos
3. Hashear senha com bcrypt
4. Criar User e Profile
2. Login:
1. Validar credenciais
2. Gerar JWT com dados do usuário

## Etapa 8: Implementar autorização no backend
Tutorial detalhado: [TUTORIAL_PASSO_08.md](docs/TUTORIAL_PASSO_08.md)
1. Criar middleware de autenticação JWT
2. Proteger rotas sensíveis
3. Garantir no service de posts:
1. só dono edita
2. só dono exclui

## Etapa 9: Implementar módulo de perfis públicos
Tutorial detalhado: [TUTORIAL_PASSO_09.md](docs/TUTORIAL_PASSO_09.md)
1. Buscar perfil por username
2. Listar posts de um perfil por username
3. Adicionar paginação simples

## Etapa 10: Implementar módulo de posts
Tutorial detalhado: [TUTORIAL_PASSO_10.md](docs/TUTORIAL_PASSO_10.md)
1. Criar post autenticado
2. Editar post com validação de dono
3. Excluir post com validação de dono
4. Buscar post por id (público)
5. Feed público paginado

## Etapa 11: Validar backend completo
Tutorial detalhado: [TUTORIAL_PASSO_11.md](docs/TUTORIAL_PASSO_11.md)
1. Rodar build TypeScript
2. Testar endpoints no Insomnia/Postman
3. Testar cenários:
1. visitante lendo feed
2. usuário criando post
3. usuário tentando editar post de outro (deve falhar)

## Etapa 12: Inicializar frontend com Vite + React + TS
Tutorial detalhado: [TUTORIAL_PASSO_12.md](docs/TUTORIAL_PASSO_12.md)
1. Criar app React com template TypeScript
2. Instalar Tailwind, React Router e Axios
3. Configurar Tailwind e arquivo de estilos base

## Etapa 13: Criar base de navegação e estado global
Tutorial detalhado: [TUTORIAL_PASSO_13.md](docs/TUTORIAL_PASSO_13.md)
1. Criar provider de autenticação
2. Persistir token e usuário em storage
3. Configurar rotas:
1. públicas
2. protegidas
3. de visitante

## Etapa 14: Criar camada de integração com API
Tutorial detalhado: [TUTORIAL_PASSO_14.md](docs/TUTORIAL_PASSO_14.md)
1. Configurar cliente Axios com baseURL
2. Adicionar interceptor para enviar Bearer token
3. Criar services por domínio:
1. auth
2. posts
3. profiles

## Etapa 15: Construir páginas principais
Tutorial detalhado: [TUTORIAL_PASSO_15.md](docs/TUTORIAL_PASSO_15.md)
1. Home
2. Login
3. Register
4. Profile
5. CreatePost
6. EditPost

## Etapa 16: Criar componentes reutilizáveis
Tutorial detalhado: [TUTORIAL_PASSO_16.md](docs/TUTORIAL_PASSO_16.md)
1. Header com estado logado/deslogado
2. Card de post com ações condicionais

## Etapa 17: Integrar ponta a ponta
Tutorial detalhado: [TUTORIAL_PASSO_17.md](docs/TUTORIAL_PASSO_17.md)
1. Subir backend + frontend + banco
2. Fazer fluxo real:
1. cadastro
2. login
3. criar post
4. editar/excluir
5. abrir perfil público

## Etapa 18: Resolver erros comuns de projeto real
Tutorial detalhado: [TUTORIAL_PASSO_18.md](docs/TUTORIAL_PASSO_18.md)
1. Tipagem de rotas Fastify com params
2. Tipagem de ambiente do Vite
3. Ajuste de module type no frontend para evitar warning
4. Locks do Prisma durante migração

## Etapa 19: Preparar para deploy (sem publicar ainda)
Tutorial detalhado: [TUTORIAL_PASSO_19.md](docs/TUTORIAL_PASSO_19.md)
1. Backend:
1. variáveis de ambiente
2. scripts de build/start
2. Frontend:
1. build de produção
2. API URL por ambiente
3. Compatibilidade:
1. frontend para Vercel
2. backend para Render

## Etapa 20: Checklist final de conclusão
Tutorial detalhado: [TUTORIAL_PASSO_20.md](docs/TUTORIAL_PASSO_20.md)
1. Backend sobe sem erro
2. Frontend sobe sem erro
3. Banco conectado
4. Migrações aplicadas
5. Cadastro e login funcionando
6. Perfil público por username funcionando
7. CRUD de posts com regra de dono funcionando
8. Visitante só visualiza
9. Build de backend e frontend funcionando