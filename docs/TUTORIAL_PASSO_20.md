<p><a href="../README.md"><button>Voltar para o README</button></a></p>

# Tutorial - Passo 20: Checklist final de conclusao

## 1. Objetivo do passo
Confirmar que voce chegou no mesmo resultado final esperado, incluindo funcionalidade, seguranca e prontidao de build.

## 2. O que sera aprendido
- Como validar entrega final de projeto real.
- Como checar regras de negocio com cenarios objetivos.
- Como identificar se algo essencial ainda esta faltando.

## 3. Por que este passo existe (antes do como)
Sem checklist final, e comum entregar algo incompleto mesmo "parecendo pronto".

## 4. Codigo necessario

Checklist tecnico:

```txt
[ ] Backend sobe sem erro
[ ] Frontend sobe sem erro
[ ] Banco conectado e migracoes aplicadas
[ ] Build backend (npm run build) sem erro
[ ] Build frontend (npm run build) sem erro
```

Checklist funcional:

```txt
[ ] Cadastro funciona
[ ] Login retorna token JWT
[ ] Feed publico funciona para visitante
[ ] Perfil publico por username funciona
[ ] Usuario autenticado cria post
[ ] Usuario autenticado edita proprio post
[ ] Usuario autenticado exclui proprio post
```

Checklist de seguranca:

```txt
[ ] POST /posts sem token retorna 401
[ ] PUT /posts/:id de outro usuario retorna 403
[ ] DELETE /posts/:id de outro usuario retorna 403
[ ] GET /posts/:id com id inexistente retorna 404
```

Comandos de validacao de build:

```bash
cd backend
npm run build

cd ../frontend
npm run build
```

## 5. Explicacao linha a linha
- checklist tecnico: garante que ambiente e build estao corretos.
- checklist funcional: garante que produto atende requisitos do usuario.
- checklist de seguranca: garante que backend protege recursos criticos.
- comandos de build: garantem prontidao para deploy.

## 6. O que o aluno construiu
Voce construiu e validou um microblog full stack completo, com arquitetura modular, autenticacao, autorizacao e fluxo real funcionando.

Parabens: voce terminou o projeto com criterio de entrega profissional.

<p><a href="../README.md"><button>Voltar para o README</button></a></p>

