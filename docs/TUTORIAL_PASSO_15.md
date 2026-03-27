<p><a href="../README.md"><button>Voltar para o README</button></a></p>

# Tutorial - Passo 15: Construir paginas principais

## 1. Objetivo do passo
Implementar as paginas do app: Home, Login, Register, Profile, CreatePost e EditPost.

## 2. O que sera aprendido
- Como montar um fluxo completo de uso da aplicacao, com estados de carregamento, erro e sucesso.
- Como consumir services dentro das paginas.
- Como tratar loading, erro e sucesso.

## 2.1 Termos deste capitulo (explicacao rapida)
- Fluxo de tela: ordem em que o usuario navega pelas paginas.
- Estado de loading: periodo em que a tela aguarda resposta da API.


## 3. Por que este passo existe (antes do como)
Sem paginas, nao existe produto para o usuario final. E nas paginas que as regras de negocio viram experiencia real de uso.

## 4. Codigo necessario
Pasta de paginas:

```txt
src/pages/
|-- Home.tsx
|-- Login.tsx
|-- Register.tsx
|-- Profile.tsx
|-- CreatePost.tsx
`-- EditPost.tsx
```

Exemplo de fluxo em `Login.tsx`:

```ts
const data = await authService.login({ email, password })
login(data.user, data.token)
navigate('/')
```

Exemplo de fluxo em `Home.tsx`:

```ts
const data = await postsService.getFeed(page, 10)
setPosts(data.posts)
setTotal(data.total)
```

## 5. Explicacao linha a linha
- `Login`: autentica e salva sessao no contexto.
- `Register`: cadastra usuario/perfil.
- `Home`: mostra feed publico com paginacao.
- `Profile`: mostra perfil e posts por username.
- `CreatePost`: cria post (rota protegida).
- `EditPost`: atualiza post existente com regra de dono.

## 6. O que o aluno construiu
Voce construiu a experiencia principal do microblog de ponta a ponta no frontend.


## 7. Dicas
- Cada pagina deve tratar loading, sucesso e erro.
- Comece com fluxo principal: login -> feed -> criar post.
- Evite logica pesada no JSX, extraia para funcoes.

## 8. Erros comuns
- Nao tratar erro de rede.
- Navegacao quebrada entre telas principais.
- Misturar responsabilidade de varias paginas no mesmo arquivo.

## 9. Checkpoints de aprendizado
- Usuario consegue cadastrar, logar e navegar no app.
- Feed e perfil carregam dados reais da API sem quebrar a navegacao.
- Criacao e edicao de post funcionam na interface com feedback visual de sucesso ou erro.

## 10. Resumo do capitulo
Voce transformou regras tecnicas em experiencia real de uso.

<p><a href="../README.md"><button>Voltar para o README</button></a></p>




