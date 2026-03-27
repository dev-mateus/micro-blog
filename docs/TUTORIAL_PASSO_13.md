<p><a href="../README.md"><button>Voltar para o README</button></a></p>

# Tutorial - Passo 13: Criar base de navegacao e estado global

## 1. Objetivo do passo
Configurar o estado global de autenticacao e as rotas publicas/protegidas do frontend.

## 2. O que sera aprendido
- Como usar Context API para sessao de usuario.
- Como persistir login com localStorage.
- Como proteger rotas com React Router.
- Como ligar App -> Provider -> Router corretamente.

## 2.1 Termos deste capitulo (explicacao rapida)
- Context: estado compartilhado entre varias telas.
- Rota protegida: pagina que exige usuario logado.


## 3. Por que este passo existe (antes do como)
Sem essa base:
- o usuario perde sessao ao atualizar pagina;
- a navegacao privada nao funciona direito;
- cada pagina tenta gerenciar autenticacao sozinha.

## 4. Codigo necessario

### 4.1 Contexto de autenticacao
Arquivo: `src/contexts/AuthContext.tsx`

```tsx
import { createContext, useContext, useState, useEffect, ReactNode } from 'react'

interface Profile {
  id: string
  username: string
  name: string
  bio: string | null
}

interface AuthUser {
  id: string
  email: string
  profile: Profile
}

interface AuthContextData {
  user: AuthUser | null
  token: string | null
  isAuthenticated: boolean
  login: (user: AuthUser, token: string) => void
  logout: () => void
}

const AuthContext = createContext<AuthContextData>({} as AuthContextData)

export function AuthProvider({ children }: { children: ReactNode }) {
  const [user, setUser] = useState<AuthUser | null>(null)
  const [token, setToken] = useState<string | null>(null)

  useEffect(() => {
    const storedToken = localStorage.getItem('@microblog:token')
    const storedUser = localStorage.getItem('@microblog:user')
    if (storedToken && storedUser) {
      setToken(storedToken)
      setUser(JSON.parse(storedUser) as AuthUser)
    }
  }, [])

  function login(userData: AuthUser, userToken: string) {
    setUser(userData)
    setToken(userToken)
    localStorage.setItem('@microblog:token', userToken)
    localStorage.setItem('@microblog:user', JSON.stringify(userData))
  }

  function logout() {
    setUser(null)
    setToken(null)
    localStorage.removeItem('@microblog:token')
    localStorage.removeItem('@microblog:user')
  }

  return (
    <AuthContext.Provider value={{ user, token, isAuthenticated: !!user, login, logout }}>
      {children}
    </AuthContext.Provider>
  )
}

export function useAuth() {
  return useContext(AuthContext)
}
```

### 4.2 Router com protecao de rotas
Arquivo: `src/router.tsx`

```tsx
import { BrowserRouter, Routes, Route, Navigate } from 'react-router-dom'
import { useAuth } from './contexts/AuthContext'
import Home from './pages/Home'
import Login from './pages/Login'
import Register from './pages/Register'
import Profile from './pages/Profile'
import CreatePost from './pages/CreatePost'
import EditPost from './pages/EditPost'

function ProtectedRoute({ children }: { children: React.ReactNode }) {
  const { isAuthenticated } = useAuth()
  return isAuthenticated ? <>{children}</> : <Navigate to="/login" replace />
}

function GuestRoute({ children }: { children: React.ReactNode }) {
  const { isAuthenticated } = useAuth()
  return !isAuthenticated ? <>{children}</> : <Navigate to="/" replace />
}

export function AppRouter() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/perfil/:username" element={<Profile />} />

        <Route path="/login" element={<GuestRoute><Login /></GuestRoute>} />
        <Route path="/register" element={<GuestRoute><Register /></GuestRoute>} />

        <Route path="/post/novo" element={<ProtectedRoute><CreatePost /></ProtectedRoute>} />
        <Route path="/post/editar/:id" element={<ProtectedRoute><EditPost /></ProtectedRoute>} />

        <Route path="*" element={<Navigate to="/" replace />} />
      </Routes>
    </BrowserRouter>
  )
}
```

### 4.3 Ligar App no Provider e Router
Arquivo: `src/App.tsx`

```tsx
import { AuthProvider } from './contexts/AuthContext'
import { AppRouter } from './router'

export default function App() {
  return (
    <AuthProvider>
      <AppRouter />
    </AuthProvider>
  )
}
```

## 5. Explicacao linha a linha
- `AuthProvider`: encapsula e distribui estado de sessao.
- `useEffect` de carga inicial: restaura sessao apos refresh.
- `login/logout`: atualizam estado + localStorage.
- `ProtectedRoute`: bloqueia rota privada sem login.
- `GuestRoute`: evita abrir login/register quando usuario ja esta autenticado.
- `App` com Provider + Router: conecta toda a estrutura global.

## 6. O que o aluno construiu
Voce construiu a espinha dorsal de navegacao e autenticacao do frontend, igual ao fluxo do projeto final.


## 7. Dicas
- Context funciona como um armario central de estado.
- Mantenha login/logout em um unico lugar para evitar duplicacao.
- Proteja rotas sensiveis com componentes guardioes.

## 8. Erros comuns
- Espalhar estado de autenticacao por varias paginas.
- Esquecer persistencia no localStorage.
- Deixar rota protegida acessivel sem sessao.

## 9. Checkpoints de aprendizado
- Sessao persiste apos recarregar pagina.
- Rotas protegidas redirecionam quando nao autenticado.
- Rotas de visitante bloqueiam usuario ja logado.

## 10. Resumo do capitulo
Voce criou a espinha dorsal de navegacao e sessao do frontend.

<p><a href="../README.md"><button>Voltar para o README</button></a></p>




