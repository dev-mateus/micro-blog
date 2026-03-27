<p><a href="../README.md"><button>Voltar para o README</button></a></p>

# Tutorial - Passo 19: Preparar para deploy (sem publicar)

## 1. Objetivo do passo
Deixar backend e frontend prontos para deploy em qualquer provedor (Render para API e Vercel para frontend).

## 2. O que sera aprendido
- Como validar o build de producao localmente antes de preparar deploy.
- Quais variaveis cada aplicacao precisa.
- Quais comandos cada plataforma deve executar.

## 2.1 Termos deste capitulo (explicacao rapida)
- Deploy: publicar aplicacao em ambiente online.
- Build de producao: versao otimizada para publicar.


## 3. Por que este passo existe (antes do como)
Projeto que roda localmente pode falhar em nuvem se:
- comando de build estiver errado;
- variavel obrigatoria estiver faltando;
- diretorio raiz do servico estiver errado.

## 4. Codigo necessario

### 4.1 Scripts corretos
Backend (`backend/package.json`):

```json
{
  "scripts": {
    "build": "tsc",
    "start": "node dist/server.js"
  }
}
```

Frontend (`frontend/package.json`):

```json
{
  "scripts": {
    "build": "tsc && vite build",
    "preview": "vite preview"
  }
}
```

### 4.2 Variaveis obrigatorias
Backend:
- `DATABASE_URL`
- `JWT_SECRET`
- `FRONTEND_URL`
- `PORT` (quando necessario)

Frontend:
- `VITE_API_URL`

### 4.3 Matriz de deploy (recomendada)
- Render (backend)
1. Root Directory: `backend`
2. Build Command: `npm install && npm run build`
3. Start Command: `npm run start`

- Vercel (frontend)
1. Root Directory: `frontend`
2. Build Command: `npm run build`
3. Output Directory: `dist`

### 4.4 Validacao local antes de publicar
```bash
cd backend
npm run build

cd ../frontend
npm run build
```

## 5. Explicacao linha a linha
- `build` backend: transforma TS em JS para ambiente de producao.
- `start` backend: roda a versao compilada.
- `build` frontend: gera assets estaticos em `dist`.
- `VITE_API_URL`: conecta frontend no endpoint publico da API.
- root directory correto: evita o erro comum de build na pasta errada.

## 6. O que o aluno construiu
Voce construiu um projeto realmente pronto para subir em nuvem, com configuracao clara por servico.


## 7. Dicas
- Build local e ensaio geral do deploy.
- Mantenha tabela simples com variaveis e comandos por servico.
- Defina claramente root directory em cada plataforma.

## 8. Erros comuns
- Configurar comando certo na pasta errada.
- Esquecer variavel obrigatoria em producao.
- Publicar sem validar build local antes.

## 9. Checkpoints de aprendizado
- Backend builda e inicia com script de producao.
- Frontend gera `dist` sem erro.
- Configuracoes de Render/Vercel estao mapeadas.

## 10. Resumo do capitulo
Voce deixou o projeto pronto para publicacao com menor risco de falha em nuvem.

<p><a href="../README.md"><button>Voltar para o README</button></a></p>




