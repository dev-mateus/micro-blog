<p><a href="../README.md"><button>Voltar para o README</button></a></p>

# Tutorial - Passo 12: Inicializar frontend com Vite + React + TS

## 1. Objetivo do passo
Criar a base tecnica do frontend com React, Vite, TypeScript e Tailwind pronta para evoluir nos proximos passos.

## 2. O que sera aprendido
- Como criar projeto React TS com Vite.
- Como configurar Tailwind corretamente.
- Como evitar erro de `import.meta.env` no TypeScript.
- Como estruturar arquivos iniciais (`main.tsx`, `App.tsx`, `index.css`).

## 2.1 Termos deste capitulo (explicacao rapida)
- Vite: ferramenta para iniciar e buildar o frontend rapidamente.
- Tailwind: biblioteca de classes utilitarias de CSS.


## 3. Por que este passo existe (antes do como)
Sem essa base:
- cada passo futuro vira remendo;
- erros de tooling aparecem cedo (Vite, TS, env);
- integrar com backend fica instavel.

## 4. Codigo necessario

### 4.1 Criar projeto
```bash
cd ..
npm create vite@latest frontend -- --template react-ts
cd frontend
npm install
```

### 4.2 Instalar dependencias
```bash
npm install react-router-dom axios
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

### 4.3 Ajustar `package.json` para ESM
```json
{
  "type": "module"
}
```

### 4.4 Configurar Tailwind
Arquivo: `tailwind.config.js`

```js
export default {
  content: ['./index.html', './src/**/*.{js,ts,jsx,tsx}'],
  theme: { extend: {} },
  plugins: [],
}
```

Arquivo: `src/index.css`

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### 4.5 Evitar erro de ambiente no TS
Arquivo: `src/vite-env.d.ts`

```ts
/// <reference types="vite/client" />
```

### 4.6 Arquivos base do app
Arquivo: `src/main.tsx`

```tsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App'
import './index.css'

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
)
```

Arquivo: `src/App.tsx`

```tsx
export default function App() {
  return <div>Frontend inicial pronto</div>
}
```

## 5. Explicacao linha a linha
- `create vite --template react-ts`: gera estrutura React + TypeScript.
- `react-router-dom` e `axios`: serao usados nos proximos passos.
- `type: module`: remove warning de modulo no ecossistema Vite.
- `@tailwind ...`: ativa utilitarios de estilo.
- `vite-env.d.ts`: garante tipagem de `import.meta.env`.
- `main.tsx`: ponto de entrada da aplicacao.

## 6. O que o aluno construiu
Voce construiu uma base frontend robusta e sem armadilhas de configuracao para continuar o projeto.


## 7. Dicas
- Pense no frontend como vitrine: precisa ser organizado desde o inicio.
- Configure Tailwind e tipagem de ambiente antes de criar telas.
- Teste o app base no navegador logo apos setup.

## 8. Erros comuns
- Esquecer `vite-env.d.ts` e quebrar `import.meta.env`.
- Nao ajustar `type: module` quando necessario.
- Configurar Tailwind parcialmente.

## 9. Checkpoints de aprendizado
- Projeto React TS sobe sem erro.
- Estilos base do Tailwind estao ativos.
- Dependencias principais foram instaladas.

## 10. Resumo do capitulo
Voce montou a base do frontend para evoluir com seguranca nos proximos passos.

<p><a href="../README.md"><button>Voltar para o README</button></a></p>




