<p><a href="../README.md"><button>Voltar para o README</button></a></p>

# Tutorial - Passo 11: Validar backend completo

## 1. Objetivo do passo
Validar que o backend compila, sobe e respeita regras de acesso e permissao do projeto.

## 2. O que sera aprendido
- Como fazer validacao tecnica antes de integrar com frontend.
- Como testar endpoints publicos e protegidos.
- Como validar respostas de erro esperadas (401, 403, 404).

## 2.1 Termos deste capitulo (explicacao rapida)
- Endpoint: rota da API acessada pelo cliente.
- Status HTTP: codigo que indica sucesso ou tipo de erro.


## 3. Por que este passo existe (antes do como)
Se voce pular esta etapa, bugs de backend aparecem depois no frontend e ficam mais caros de diagnosticar.

## 4. Codigo necessario

### 4.1 Build e execucao
```bash
cd backend
npm run build
npm run dev
```

### 4.2 Testes publicos
```bash
curl http://localhost:3333/health
curl "http://localhost:3333/posts?page=1&limit=10"
curl http://localhost:3333/posts/ID_QUE_NAO_EXISTE
```

### 4.3 Testes de seguranca
Sem token (esperado 401):

```bash
curl -X POST http://localhost:3333/posts -H "Content-Type: application/json" -d "{\"content\":\"teste\"}"
```

Com token de usuario A tentando editar post de usuario B (esperado 403).

Com id inexistente em update/delete (esperado 404).

## 5. Explicacao linha a linha
- `npm run build`: garante que TypeScript esta consistente.
- `GET /health`: verifica se API realmente subiu.
- `GET /posts`: valida feed publico e paginacao.
- `GET /posts/:id` com id invalido: valida tratamento de not found.
- `POST /posts` sem token: valida bloqueio de autenticacao (401).
- editar post de outro usuario: valida autorizacao por dono (403).

## 6. O que o aluno construiu
Voce construiu confianca no backend: ele nao so funciona, como protege as regras de negocio corretamente.


## 7. Dicas
- Teste casos felizes e casos de falha.
- Valide primeiro pelo terminal para isolar problemas.
- Guarde uma lista de cenarios de regressao para repetir sempre.

## 8. Erros comuns
- Testar apenas endpoints de sucesso.
- Pular build antes da integracao com frontend.
- Nao validar respostas de erro (401, 403, 404).

## 9. Checkpoint de aprendizado
- Build backend concluido sem erro.
- Endpoints publicos e protegidos testados.
- Cenarios de seguranca validados com resultado esperado.

## 10. Resumo do capitulo
Voce confirmou que o backend esta funcional e seguro antes da integracao completa.

<p><a href="../README.md"><button>Voltar para o README</button></a></p>




