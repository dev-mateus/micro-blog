<p><a href="../README.md"><button>Voltar para o README</button></a></p>

# Tutorial - Passo 14: Criar camada de integracao com API

## 1. Objetivo do passo
Centralizar chamadas HTTP no frontend com Axios, token JWT automatico e services por dominio.

## 2. O que sera aprendido
- Como configurar `baseURL` por ambiente.
- Como evitar erro de TypeScript com `import.meta.env`.
- Como injetar JWT no header automaticamente.
- Como organizar chamadas por `auth`, `posts` e `profiles`.

## 2.1 Termos deste capitulo (explicacao rapida)
- Axios: cliente HTTP usado para chamar a API.
- Interceptor: regra automatica antes de enviar requisicoes.


## 3. Por que este passo existe (antes do como)
Sem essa camada:
- cada pagina conhece detalhes da API;
- mudanca de endpoint quebra varios arquivos;
- autenticacao vira codigo duplicado.

## 4. Codigo necessario

### 4.1 Tipagem de ambiente do Vite
Arquivo: `src/vite-env.d.ts`

```ts
/// <reference types="vite/client" />
```

### 4.2 Cliente Axios central
Arquivo: `src/services/api.ts`

```ts
import axios from 'axios'

const api = axios.create({
  baseURL: import.meta.env.VITE_API_URL || 'http://localhost:3333',
})

api.interceptors.request.use((config) => {
  const token = localStorage.getItem('@microblog:token')
  if (token) {
    config.headers.Authorization = `Bearer ${token}`
  }
  return config
})

export default api
```

### 4.3 Services de dominio
Arquivo: `src/services/auth.service.ts`

```ts
import api from './api'

export interface RegisterData {
  email: string
  password: string
  username: string
  name: string
  bio?: string
}

export interface LoginData {
  email: string
  password: string
}

export const authService = {
  async register(data: RegisterData) {
    const response = await api.post('/auth/register', data)
    return response.data
  },
  async login(data: LoginData) {
    const response = await api.post('/auth/login', data)
    return response.data
  },
}
```

Arquivo: `src/services/posts.service.ts`

```ts
import api from './api'

export const postsService = {
  async getFeed(page = 1, limit = 10) {
    const response = await api.get('/posts', { params: { page, limit } })
    return response.data
  },
  async getById(id: string) {
    const response = await api.get(`/posts/${id}`)
    return response.data
  },
  async create(content: string) {
    const response = await api.post('/posts', { content })
    return response.data
  },
  async update(id: string, content: string) {
    const response = await api.put(`/posts/${id}`, { content })
    return response.data
  },
  async delete(id: string) {
    await api.delete(`/posts/${id}`)
  },
}
```

Arquivo: `src/services/profiles.service.ts`

```ts
import api from './api'

export const profilesService = {
  async getByUsername(username: string) {
    const response = await api.get(`/profiles/${username}`)
    return response.data
  },
  async getPostsByUsername(username: string, page = 1, limit = 10) {
    const response = await api.get(`/profiles/${username}/posts`, { params: { page, limit } })
    return response.data
  },
}
```

## 5. Explicacao linha a linha
- `vite-env.d.ts`: habilita tipos do Vite para `import.meta.env`.
- `axios.create`: cria cliente HTTP unico para o app.
- `baseURL`: define endereco base da API em um lugar so.
- `interceptor`: injeta token em toda chamada autenticada.
- `auth/posts/profiles service`: separa API por dominio de negocio.

## 6. O que o aluno construiu
Voce construiu uma camada de integracao limpa e escalavel, igual ao padrao do projeto final.


## 7. Dicas
- Centralizar chamadas HTTP evita repeticao e erro.
- Use services por dominio para manter codigo limpo.
- Deixe o token automatico no interceptor.

## 8. Erros comuns
- Chamar API direto em varias paginas sem padrao.
- Esquecer baseURL por ambiente.
- Nao enviar Bearer token nas rotas protegidas.

## 9. Checkpoint de aprendizado
- Cliente Axios esta centralizado.
- Token e anexado automaticamente nas requisicoes autenticadas.
- Services de auth, posts e profiles estao separados.

## 10. Resumo do capitulo
Voce estruturou a comunicacao frontend-backend de forma clara e reutilizavel.

<p><a href="../README.md"><button>Voltar para o README</button></a></p>




