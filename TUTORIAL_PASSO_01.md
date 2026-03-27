# Tutorial - Passo 1: Entender o produto antes de codar

## 1. Objetivo do passo
Antes de abrir o editor e escrever código, o objetivo aqui é transformar a ideia do microblog em regras claras de negócio.

## 2. O que será aprendido
- Por que requisitos bem definidos economizam tempo.
- Como separar o que o sistema faz e o que ele não faz.
- Como escrever regras simples que guiem backend e frontend.

## 3. Por que este passo existe (antes do como)
Se voce comeca a programar sem clareza, acontece isto:
- o backend cria endpoints que depois mudam;
- o frontend monta telas que nao servem para o fluxo real;
- surgem retrabalhos e bugs de regra de negocio.

Quando voce define as regras antes, o codigo fica mais simples, previsivel e seguro.

## 4. Codigo necessario
Neste passo, o "codigo" e um documento de requisitos. Ele e o ponto de partida do projeto.

Crie um arquivo de especificacao inicial com o conteudo abaixo:

```md
# Especificacao inicial - Microblog

## Objetivo
Criar um microblog com autenticacao e posts de texto.

## Escopo (o que entra)
- Cadastro de usuario
- Login com JWT
- Perfil publico por username
- Criacao, edicao e exclusao de posts pelo dono
- Feed publico de posts

## Fora de escopo (o que nao entra agora)
- Curtidas
- Comentarios
- Seguir usuarios
- Upload de imagens

## Regras de negocio
1. Visitantes podem ver feed, perfil e posts.
2. Usuario autenticado pode gerenciar apenas seu perfil.
3. Usuario autenticado pode criar e gerenciar apenas seus posts.
4. Permissoes devem ser validadas no backend.
5. Frontend nao pode ser usado como camada de seguranca.

## Entidades principais
- User: conta de acesso
- Profile: dados publicos do usuario
- Post: conteudo textual publicado no perfil
```

## 5. Explicacao linha a linha do arquivo de especificacao
### Bloco: Titulo e objetivo
- `# Especificacao inicial - Microblog`
  - Define o nome do documento e o contexto.
- `## Objetivo`
  - Explica em uma frase o que o sistema precisa resolver.

### Bloco: Escopo
- `## Escopo (o que entra)`
  - Lista somente funcionalidades desta versao.
- Itens de escopo
  - Viram backlog tecnico (rotas, telas e testes).

### Bloco: Fora de escopo
- `## Fora de escopo (o que nao entra agora)`
  - Protege o projeto contra "scope creep".
- Itens fora de escopo
  - Devem ser ignorados nesta fase para manter foco.

### Bloco: Regras de negocio
- Regras `1` a `5`
  - Sao as regras que backend e frontend devem respeitar.
  - A regra mais importante para seguranca e: validacao de permissao no backend.

### Bloco: Entidades principais
- `User`, `Profile`, `Post`
  - Antecipam o desenho do banco e dos modulos da API.

## 6. Como este passo se conecta com os proximos
- Passo 2 (ambiente): voce prepara o setup com base no que sera construido.
- Passo 3 e 4 (backend + banco): as entidades viram modelos Prisma.
- Passo 12 em diante (frontend): o escopo vira paginas e fluxos da interface.

## 7. O que o aluno construiu neste passo
Voce construiu a base conceitual do projeto:
- um contrato funcional do produto;
- um limite claro do que implementar agora;
- regras de negocio que guiam todas as decisoes tecnicas.

Sem este passo, o projeto nasce confuso. Com este passo, ele nasce com direcao.
