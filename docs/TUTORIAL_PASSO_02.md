<p><a href="../README.md"><button>Voltar para o README</button></a></p>

# Tutorial - Passo 2: Preparar o ambiente de desenvolvimento

## 1. Objetivo do passo
Preparar seu computador e a estrutura inicial de pastas para conseguir desenvolver o microblog com seguranÃ§a e sem travar por problemas de ambiente.

## 2. O que serÃ¡ aprendido
- Por que configurar ambiente antes de programar.
- Quais ferramentas sao obrigatorias para este projeto.
- Como criar a estrutura de pastas `backend/` e `frontend/`.
- Como validar que tudo esta pronto para os proximos passos.

## 3. Por que este passo existe (antes do como)
Muitos iniciantes pulam essa etapa e vao direto para o codigo. O problema:
- o projeto nao roda por falta de Node;
- o banco nao conecta por falta de PostgreSQL;
- comandos falham por estarem no diretorio errado.

Quando o ambiente esta correto, voce gasta tempo aprendendo desenvolvimento, e nao corrigindo erros de instalacao.

## 4. Codigo necessario
Neste passo, o "codigo" e a preparacao do ambiente e comandos de terminal.

### 4.1 Instalar ferramentas obrigatorias
- Node.js (recomendado: versao LTS moderna)
- npm (vem junto com Node.js)
- PostgreSQL **ou** Docker Desktop
- VS Code

### 4.2 Criar pasta do projeto
No terminal, escolha um local e execute:

```bash
mkdir micro-blog
cd micro-blog
```

### 4.3 Criar estrutura inicial de pastas
```bash
mkdir backend frontend
```

### 4.4 Validar instalacoes
```bash
node -v
npm -v
```

Se for usar Docker para banco:

```bash
docker --version
```

Se for usar PostgreSQL local, valide que ele esta instalado e rodando com a ferramenta que voce usa (pgAdmin, service manager, etc).

## 5. Explicacao linha a linha
### Bloco: criar pasta do projeto
- `mkdir micro-blog`
  - Cria a pasta raiz onde tudo vai morar.
- `cd micro-blog`
  - Entra na pasta para que os proximos comandos criem arquivos no lugar certo.

### Bloco: criar backend e frontend
- `mkdir backend frontend`
  - Cria duas aplicacoes separadas:
  - `backend`: API, regras e banco.
  - `frontend`: interface React que roda no navegador.

### Bloco: validar Node e npm
- `node -v`
  - Mostra a versao do Node instalada.
- `npm -v`
  - Mostra a versao do gerenciador de pacotes.

Se algum desses comandos falhar, a instalacao do Node nao esta pronta.

### Bloco: validar Docker (opcional)
- `docker --version`
  - Confirma que o Docker CLI existe na maquina.

Se voce for usar PostgreSQL em container, esse comando precisa funcionar.

## 6. Checklist de saida deste passo
Ao final, voce deve ter:
- pasta raiz `micro-blog`
- pasta `backend/`
- pasta `frontend/`
- Node e npm funcionando
- opcionalmente Docker funcionando

Estrutura esperada:

```txt
micro-blog/
â”œâ”€â”€ backend/
â””â”€â”€ frontend/
```

## 7. Como este passo se conecta com o proximo
No Passo 3, voce vai entrar em `backend/` e iniciar o projeto TypeScript com dependencias da API.

Sem este passo, os comandos do backend podem ser executados no diretorio errado e quebrar tudo.

## 8. O que o aluno construiu neste passo
Voce construiu a base tecnica do projeto:
- um ambiente pronto para desenvolvimento;
- uma organizacao inicial correta de pastas;
- validacao minima para seguir sem bloqueios.

Em resumo: agora seu computador esta pronto para transformar o roteiro em codigo real.


## 7. Dicas
- Trate este passo como montar a bancada antes da oficina.
- Mantenha backend e frontend separados desde o inicio.
- Valide versoes das ferramentas antes de continuar.

## 8. Erros comuns
- Instalar ferramenta e nao testar se funciona.
- Misturar arquivos de backend e frontend na mesma pasta.
- Pular validacao do Docker/Postgres.

## 9. Checkpoint de aprendizado
- Node e npm respondem no terminal.
- Banco ou Docker esta pronto para uso.
- Estrutura de pastas do projeto foi criada corretamente.

## 10. Resumo do capitulo
Voce preparou um ambiente estavel para desenvolver sem perder tempo com problemas de setup.

<p><a href="../README.md"><button>Voltar para o README</button></a></p>

