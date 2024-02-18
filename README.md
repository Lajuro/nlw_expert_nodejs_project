# Criação de Ferramenta de Votos Usando WebSocket

Neste NLW Expert, a Rocketseat, na trilha de Node.js, idealizou o projeto onde será o back-end utilizando Web Sockets para computação de votos.

- [Criação de Ferramenta de Votos Usando WebSocket](#criação-de-ferramenta-de-votos-usando-websocket)
  - [TechStack](#techstack)
  - [Aulas](#aulas)
    - [Aula 01](#aula-01)
      - [Criando o projeto](#criando-o-projeto)
      - [Iniciando a API](#iniciando-a-api)
        - [Explicação do Código Fastify](#explicação-do-código-fastify)
          - [Importação do Fastify](#importação-do-fastify)
          - [Criação da Instância do Fastify](#criação-da-instância-do-fastify)
          - [Configuração da Rota GET](#configuração-da-rota-get)
          - [Iniciando o Servidor](#iniciando-o-servidor)
      - [Docker](#docker)
        - [Criando o Docker Compose](#criando-o-docker-compose)
          - [Versão](#versão)
          - [Serviços](#serviços)
          - [PostgreSQL (postgres)](#postgresql-postgres)
          - [Redis (redis)](#redis-redis)
          - [Volumes](#volumes)
          - [Resumo](#resumo)
        - [Rodando o Docker Compose](#rodando-o-docker-compose)
          - [Conferindo os logs](#conferindo-os-logs)
      - [Testando conexão com o banco de dados](#testando-conexão-com-o-banco-de-dados)
        - [Configurando o Prisma](#configurando-o-prisma)
        - [Entendendo o arquivo schema.prisma](#entendendo-o-arquivo-schemaprisma)
      - [Criando uma tabela (model) no Prisma](#criando-uma-tabela-model-no-prisma)
        - [Criando a tabela no banco de dados](#criando-a-tabela-no-banco-de-dados)
      - [Acessando a interface do Prisma](#acessando-a-interface-do-prisma)
      - [Criando uma rota para acessar o banco de dados](#criando-uma-rota-para-acessar-o-banco-de-dados)
      - [Instalando e configurando o Zod](#instalando-e-configurando-o-zod)
      - [Adicionando a enquete no banco de dados](#adicionando-a-enquete-no-banco-de-dados)
    - [Aula 2](#aula-2)
      - [Organizando a estrutura do projeto](#organizando-a-estrutura-do-projeto)
        - [Atualizando o server.ts](#atualizando-o-serverts)
      - [Configurando um novo model](#configurando-um-novo-model)
        - [Rodando a migração](#rodando-a-migração)
    - [Aula 02: By ChatGPT](#aula-02-by-chatgpt)
      - [Configuração Inicial](#configuração-inicial)
        - [Estrutura de Pastas](#estrutura-de-pastas)
        - [Conexão com o Banco de Dados](#conexão-com-o-banco-de-dados)
      - [Organização das Rotas](#organização-das-rotas)
        - [Estruturação de Rotas](#estruturação-de-rotas)
        - [Criação de Arquivos de Rotas](#criação-de-arquivos-de-rotas)
      - [Modelo de Dados para Votação](#modelo-de-dados-para-votação)
        - [Tabela `PollOption`](#tabela-polloption)
        - [Relacionamento com Tabela `Poll`](#relacionamento-com-tabela-poll)
      - [Implementação das Rotas](#implementação-das-rotas)
        - [Rota de Criação de Enquete](#rota-de-criação-de-enquete)
        - [Rota de Detalhes da Enquete](#rota-de-detalhes-da-enquete)
      - [Lógica de Votação e Cookies](#lógica-de-votação-e-cookies)
        - [Identificação Única de Usuários](#identificação-única-de-usuários)
        - [Implementação da Rota de Votação](#implementação-da-rota-de-votação)
      - [Estruturação da Tabela de Votos](#estruturação-da-tabela-de-votos)
        - [Modelo `Vote`](#modelo-vote)
        - [Constraint de Unicidade](#constraint-de-unicidade)
    - [Aula 03: By ChatGPT](#aula-03-by-chatgpt)
      - [Introdução à Última Aula](#introdução-à-última-aula)
        - [Objetivos da Aula](#objetivos-da-aula)
      - [Implementação do Redis para Caching](#implementação-do-redis-para-caching)
        - [Configuração do Redis](#configuração-do-redis)
        - [Estrutura de Dados no Redis](#estrutura-de-dados-no-redis)
      - [Comunicação em Tempo Real com WebSockets](#comunicação-em-tempo-real-com-websockets)
        - [Introdução ao WebSocket](#introdução-ao-websocket)
        - [Configuração do WebSocket no Fastify](#configuração-do-websocket-no-fastify)
      - [Implementação do Sistema de Votação com Redis e WebSocket](#implementação-do-sistema-de-votação-com-redis-e-websocket)
        - [Atualização dos Votos com Redis](#atualização-dos-votos-com-redis)
        - [Envio de Dados via WebSocket](#envio-de-dados-via-websocket)
      - [Estrutura de Pub/Sub para Eventos de Votação](#estrutura-de-pubsub-para-eventos-de-votação)
        - [Criação de Canais de Eventos](#criação-de-canais-de-eventos)
        - [Inscrição e Publicação em Canais](#inscrição-e-publicação-em-canais)

## TechStack

- Node.js (Node v20.11.0 / NPM 10.4.0)
- TypeScript
- Fastify
- Prisma
- PostreSQL
- Redis
- WebSockets

## Aulas

### Aula 01

É hora de começar nosso projeto! Vamos montar o setup com TypeScript, Fastify e Prisma, entender conceitos de API, como método HTTP e rotas, além de colocar a mão na massa para criar nossa primeira tabela utilizando Prisma.

#### Criando o projeto

Abra o terminal, pois iremos criar a pasta do projeto e fazer as configurações iniciais:

1. Crie, em um local de preferência, a pasta do projeto, como sugestão pode criar como `nlw/expert/aulas/polls`.

```bash
cd ~
mkdir -p nlw/expert/aulas/polls

cd nlw/expert/aulas/polls

code .
```

> Basicamente esse comando irá para a pasta raíz `~`, que é a pasta do usuário do PC, e então no comando seguinte é o de criação da pasta. A flag `-p` serve para poder criar as pastas e subpastas de uma única vez.
>
> Então, após criar a pasta, irá acessar a pasta `polls` e em seguida será acessado aberto com o Visual Studio Code na pasta atual, outra forma seria trocar o `cd` do comando anterior (Change Directory), pelo próprio `code`, pois irá resultar na mesma coisa.
>
> **Obs:** No meu caso, criei uma pasta com o nome `rocketseat` na minha pasta de usuário `~`, então adapte da melhor forma que possa atender sua necessidade.

2. Com o projeto aberto no Visual Studio Code, chegou a hora de configurarmos o projeto, então abra o terminal do VSCode e digite os seguintes comandos:

```bash
npm init -y
npm install typescript @types/node -D
```

> O primeiro comando inicia um projeto Node.js e a flag `-y` indica que será criado da maneira `default`, seria a mesma coisa que rodar sem a flag e confirmar o padrão de cada prompt.
>
> O segundo comando faz a instalação das primeiras dependências, o `typescript`, e a tipagem para o node que é o `@types/node`, e a flag `-D` indica que são dependências de desenvolvimento, ou seja, no `package.json` será adicionado em `devDependencies`, ao invés de ser em `dependencies`.

3. Vamos agora inicializar o TypeScript, para isso digite o seguinte no terminal:

```bash
npx tsc --init
```

> O comando `npx tsc --init` é usado para inicializar um novo projeto TypeScript. Vamos decompor este comando para entender melhor cada parte:
>
> 1. **npx**: É uma ferramenta de execução de pacotes do Node.js que vem com npm (Node Package Manager). `npx` permite executar pacotes que estão instalados localmente na pasta `node_modules` de um projeto ou baixá-los e executá-los on-the-fly se eles não estiverem instalados.
>
> 2. **tsc**: É a abreviação de TypeScript Compiler. Este é o compilador do TypeScript, responsável por converter código TypeScript (`.ts` ou `.tsx` para arquivos JavaScript `.js`).
>
> 3. **--init**: Esta é uma opção de linha de comando para o compilador TypeScript. Quando usada, ela instrui o compilador a criar um novo arquivo `tsconfig.json` no diretório atual.
>
> O arquivo `tsconfig.json` é fundamental em um projeto TypeScript. Ele contém várias configurações que determinam como o compilador deve funcionar. Por exemplo, você pode especificar quais arquivos incluir na compilação, quais regras de compilação aplicar, e configurar opções para o mapeamento de código-fonte, entre outras coisas.
>
> Portanto, ao executar `npx tsc --init` em um diretório, você está preparando esse diretório para ser um projeto TypeScript, criando um arquivo `tsconfig.json` com configurações padrão. A partir daí, você pode editar este arquivo para personalizar as configurações de acordo com as necessidades do seu projeto.

4. Abra o navegador e acesse o seguinte endereço: [https://github.com/microsoft/TypeScript/wiki/Node-Target-Mapping](https://github.com/microsoft/TypeScript/wiki/Node-Target-Mapping)

5. Nesse repositório da Microsoft, você irá encontrar como você deve configurar o seu `tsconfig.json`, no caso da versão 20 do Node, está da seguinte forma:

```json
{
  "compilerOptions": {
    "lib": ["ES2023"],
    "module": "node16",
    "target": "ES2022"
  }
}
```

Basta acessar o arquivo `tsconfig.json` e alterar cada um desses campos conforme está descrito. O resto pode deixar exatamente como está.

6. Agora, vamos criar uma pasta `src` na raíz do projeto e dentro da pasta, criamos uma outra pasta, com o nome `http` e dentro dessa pasta crie um arquivo chamado `server.ts`, que será o arquivo principal do nosso servidor HTTP.

7. Para não precisarmos ficar rodando o compilador do TypeScript a cada execução, vamos instalar um biblioteca que faz todo esse processo de maneira extremamente simples, que é o `tsx`, também como uma dependência de desenvolvimento:

```bash
npm install tsx -D
```

8. Criar um script de `dev` para execução de maneira mais simples, onde bastará digitar `npm run dev` no terminal, para isso, acesse o arquivo `package.json` e em `scripts`, altere o `test` por `dev`, tudo ficará da seguinte forma:

```json
"scripts": {
  "dev": "tsx watch src/http/server.ts"
},
```

> TSX é uma versão melhorada do `ts-node`, utilizando o ESBuild para transpilar arquivos TypeScript para JavaScript de maneira rápida. Ele foi desenvolvido para ser um substituto completo para Node, permitindo usar TSX como um REPL TypeScript se instalado globalmente. A funcionalidade `watch` do TSX pode ser usada para observar mudanças em um arquivo específico. [Fonte](https://dev.to/_staticvoid/how-to-run-typescript-natively-in-nodejs-with-tsx-3a0c)

9. Rode o comando `npm run dev` e teste editar o arquivo `server.ts` para observar o funcionamento, para parar pressione `CTRL + C`.

> **Sugestão de código**
>
> ```js
> console.log("Hello World")
> ```
> </br>

#### Iniciando a API

Agora que fizemos as configurações iniciais do projeto, vamos iniciar a API e iremos utilizar o [Fastify](https://fastify.dev/), ao invés do Express.js.

Para instalar, digite o seguinte no terminal:

```bash
npm install fastify
```

O código é o seguinte:

```js
import fastify from 'fastify'

const app = fastify()

app.get('/hello', () => {
  return 'Hello NLW'
})

app.listen({ port: 3333 }).then(() => {
  console.log('HTTP server running!');
})
```

##### Explicação do Código Fastify

Este código é um exemplo básico de um servidor HTTP usando o Fastify, um framework web moderno para Node.js.

###### Importação do Fastify

```javascript
import fastify from 'fastify'
```

Aqui, importamos o `fastify` do módulo `fastify`. Isso nos permite criar uma instância do servidor Fastify.

###### Criação da Instância do Fastify

```javascript
const app = fastify()
```

Nesta linha, criamos uma instância do Fastify chamando `fastify()`. Esta instância, armazenada na variável `app`, é usada para configurar rotas, plugins e iniciar o servidor.

###### Configuração da Rota GET

```javascript
app.get('/hello', () => {
  return 'Hello NLW'
})
```

Aqui, configuramos uma rota GET para o caminho `/hello`. Quando essa rota é acessada, a função de callback é chamada e retorna a string `'Hello NLW'`. Isso significa que, se acessarmos `http://localhost:3333/hello` no navegador, veremos a mensagem 'Hello NLW'.

###### Iniciando o Servidor

```javascript
app.listen({ port: 3333 }).then(() => {
  console.log('HTTP server running!');
})
```

Finalmente, iniciamos o servidor para escutar na porta `3333`. O método `listen` é assíncrono e retorna uma Promise. Quando o servidor estiver pronto e escutando, a mensagem `'HTTP server running!'` será exibida no console. Isso indica que o servidor está ativo e pode receber requisições HTTP.

#### Docker

Vamos utilizar o Docker, então para isso é importante que esteja instalado em sua máquina, basta acessar o site e baixar de acordo com o seu sistema operacional.

[Acesse a página do Docker para download clicando aqui.](https://docs.docker.com/desktop/install/windows-install/)

##### Criando o Docker Compose

Começamos criando um arquivo chamado `docker-compose.yml` na pasta raíz do projeto.

**O que é o Docker Compose?**

O `docker-compose.yml` é um arquivo usado pelo Docker Compose, uma ferramenta para definir e executar aplicações Docker multi-contêineres. O arquivo usa a linguagem YAML (YAML Ain't Markup Language) para especificar como os serviços (contêineres) da sua aplicação são construídos, conectados e executados. Aqui estão alguns pontos-chave sobre o `docker-compose.yml`:

1. **Definição de Serviços**: Cada serviço no `docker-compose.yml` representa um contêiner. Você pode definir vários serviços, cada um com suas próprias configurações, como a imagem Docker a ser usada, variáveis de ambiente, volumes, portas expostas, etc.

2. **Configuração de Rede**: O Docker Compose facilita a configuração de redes para comunicação entre contêineres. Você pode definir redes personalizadas e especificar quais serviços se comunicam em quais redes.

3. **Volumes**: Volumes são usados para persistência de dados e compartilhamento de arquivos entre o contêiner e o host ou entre contêineres. No `docker-compose.yml`, você pode especificar volumes para cada serviço.

4. **Dependências**: O arquivo permite que você defina dependências entre serviços. Por exemplo, um serviço de aplicativo web pode depender de um serviço de banco de dados, garantindo que o banco de dados esteja pronto antes de iniciar o aplicativo.

5. **Variáveis de Ambiente**: Você pode definir variáveis de ambiente para cada serviço, o que é útil para configurar o comportamento do contêiner ou passar credenciais de forma segura.

6. **Escala e Atualização**: Com o Docker Compose, você pode escalar serviços facilmente, aumentando ou diminuindo o número de réplicas de um serviço. Além disso, atualizar um serviço para usar uma nova imagem é simplificado.

7. **Execução com um Único Comando**: Uma vez que o `docker-compose.yml` esteja configurado, você pode iniciar toda a sua aplicação com um único comando (`docker-compose up`). Isso torna mais fácil a implantação e o teste de ambientes complexos.

8. **Desenvolvimento e Teste**: Docker Compose é frequentemente usado em ambientes de desenvolvimento e teste, pois permite que desenvolvedores e testadores recriem um ambiente de aplicação complexo com facilidade.

O `docker-compose.yml` torna a gestão de aplicações multi-contêineres mais simples e declarativa, proporcionando uma maneira conveniente de orquestrar vários aspectos de contêineres Docker juntos.

O arquivo ficará da seguinte forma:

```yml
version: '3.7'

services:
  postgres:
    image: bitnami/postgresql:latest
    ports:
      - '5432:5432'
    environment:
      - POSTGRES_USER=docker
      - POSTGRES_PASSWORD=docker
      - POSTGRES_DB=polls
    volumes:
      - polls_pg_data:/bitnami/postgresql

  redis:
    image: bitnami/redis:latest
    environment:
      - ALLOW_EMPTY_PASSWORD=yes
    ports:
      - '6379:6379'
    volumes:
      - 'polls_redis_data:/bitnami/redis/data'

volumes:
  polls_pg_data:
  polls_redis_data:
```

Este `docker-compose.yml` define uma aplicação Docker com dois serviços: `postgres` e `redis`. Vamos analisar cada seção do arquivo:

###### Versão

```yaml
version: '3.7'
```
Esta linha especifica a versão do Docker Compose a ser usada. A versão '3.7' é uma das versões mais recentes que suporta configurações específicas para serviços, redes e volumes.

###### Serviços

O arquivo define dois serviços: `postgres` e `redis`.

###### PostgreSQL (postgres)

```yaml
services:
  postgres:
    image: bitnami/postgresql:latest
    ports:
      - '5432:5432'
    environment:
      - POSTGRES_USER=docker
      - POSTGRES_PASSWORD=docker
      - POSTGRES_DB=polls
    volumes:
      - polls_pg_data:/bitnami/postgresql
```

- **Imagem**: `bitnami/postgresql:latest` é a imagem Docker usada para criar o contêiner do PostgreSQL. `latest` significa que ele usará a versão mais recente dessa imagem.
- **Portas**: A porta `5432` (padrão do PostgreSQL) no contêiner é mapeada para a mesma porta no host, permitindo a conexão com o banco de dados.
- **Variáveis de Ambiente**: Define as credenciais e o nome do banco de dados: usuário (`docker`), senha (`docker`) e o nome do banco de dados (`polls`).
- **Volumes**: `polls_pg_data:/bitnami/postgresql` mapeia um volume chamado `polls_pg_data` para o caminho `/bitnami/postgresql` no contêiner, permitindo a persistência dos dados do PostgreSQL.

###### Redis (redis)

```yaml
  redis:
    image: bitnami/redis:latest
    environment:
      - ALLOW_EMPTY_PASSWORD=yes
    ports:
      - '6379:6379'
    volumes:
      - 'polls_redis_data:/bitnami/redis/data'
```

- **Imagem**: `bitnami/redis:latest` é a imagem Docker usada para criar o contêiner do Redis. Novamente, `latest` indica a versão mais recente.
- **Variáveis de Ambiente**: `ALLOW_EMPTY_PASSWORD=yes` permite que o Redis seja iniciado sem uma senha definida.
- **Portas**: A porta `6379` (padrão do Redis) no contêiner é mapeada para a mesma porta no host.
- **Volumes**: `polls_redis_data:/bitnami/redis/data` mapeia um volume chamado `polls_redis_data` para o caminho `/bitnami/redis/data` no contêiner, permitindo a persistência dos dados do Redis.

###### Volumes

```yaml
volumes:
  polls_pg_data:
  polls_redis_data:
```

Esta seção define os volumes usados pelos serviços. `polls_pg_data` é usado pelo PostgreSQL e `polls_redis_data` pelo Redis. Os volumes são úteis para persistir dados mesmo quando os contêineres são destruídos e recriados.

###### Resumo

O arquivo `docker-compose.yml` cria uma configuração de aplicação com um banco de dados PostgreSQL e um servidor Redis, ambos com volumes para persistência de dados. Ele é útil para aplicações que precisam de um banco de dados e um armazenamento em cache ou armazenamento de sessão, como aplicações web.

##### Rodando o Docker Compose

Agora que já configurou o arquivo `docker-compose.yml`, basta rodarmos o comando que irá executar esse arquivo, no seu terminal digite o seguinte:

```bash
docker compose up -d
```

###### Conferindo os logs

Com a máquina em execução, agora você pode verificar se ela está no ar, para isso apenas digite o seguinte comando no terminal:

```bash
docker ps
```

Esse comando irá listar os dois containers, um do `redis` e o outro do `postgresql`.

Agora para você poder checar o log é muito simples, apenas copie o `CONTAINER ID` do container que quiser checar os logs e então digite o seguinte comando no terminal:

```bash
docker logs <CONTAINER ID>
```

#### Testando conexão com o banco de dados

Existem diversas formas de se conectar no banco de dados, sendo formas de mais baixo nível e outras de mais alto nível.

A forma que iremos utilizar para conexão, será um `ORM`, e o que iremos utilizar será o `Prisma`.

##### Configurando o Prisma

Vamos instalar primeiramente o Prisma como uma dependência de desenvolvimento, para isso, digite o seguinte no terminal:

```bash
npm install -D prisma
```

E agora vamos inicializar o prisma, digitando o seguinte comando:

```bash
npx prisma init
```

Agora que foi criado alguns arquivos e inicializado o Prisma, vamos configurar o arquivo gerado `.env`, nesse arquivo já está com a estrutura para utilização do `PostgreSQL`, então vamos apenas corrigir os dados de acordo com o que passamos na receita do Docker Compose, pegando o que foi preenchido no `POSTGRES_USER`, `POSTGRES_PASSWORD` e `POSTGRES_DB`.

Ficará da seguinte maneira:

```env
DATABASE_URL="postgresql://docker:docker@localhost:5432/polls?schema=public"
```

> **Importante**</br>
> Instale a extensão do Prisma no seu Visual Studio Code, pois ela irá ajudar no desenvolvimento.
>
> Além disso, para facilitar, abra as configurações do Visual Studio Code (Pressione CTRL + SHIFT + P e digite `JSON` e abra o arquivo de configurações do usuário em JSON) e adicione em algum lugar o seguinte:
>
> ```json
> "[prisma]": {
>    "editor.formatOnSave": true
>  },
> ```
>
> Quando salvar o arquivo será então formatado automaticamente.

##### Entendendo o arquivo schema.prisma

O arquivo `schema.prisma` gerado pela inicialização do Prisma serve para representar o banco de dados, com as tabelas e colunas.

#### Criando uma tabela (model) no Prisma

Iremos criar uma tabela no `schema.prisma`, abra esse arquivo de dentro da pasta `prisma` e adicione a seguinte tabela:

```typescript
model Poll {
  id        String   @id @default(uuid())
  title     String
  createdAt DateTime @default(now())
  updatedAt DateTime @default(now())
}
```

O modelo `Poll` descrito no Prisma é uma definição de como uma entidade (neste caso, uma enquete) é representada no banco de dados. Vamos analisar cada parte do modelo:

1. **`model Poll { ... }`**: Esta linha indica o início da definição do modelo chamado `Poll`. No Prisma, um modelo representa uma tabela no banco de dados.

2. **`id String @id @default(uuid())`**: 
   - `id`: Nome da coluna.
   - `String`: Tipo de dados da coluna, neste caso, uma string.
   - `@id`: Indica que esta coluna é a chave primária da tabela.
   - `@default(uuid())`: Define o valor padrão para esta coluna como um UUID (Universal Unique Identifier) gerado automaticamente. Isso significa que sempre que um novo registro for criado, um UUID único será atribuído automaticamente a esta coluna.

3. **`title String`**: 
   - `title`: Nome de outra coluna na tabela.
   - `String`: Indica que o tipo de dados desta coluna é uma string.

4. **`createdAt DateTime @default(now())`**:
   - `createdAt`: Nome da coluna.
   - `DateTime`: Tipo de dados da coluna, neste caso, uma data e hora.
   - `@default(now())`: Define que o valor padrão desta coluna é a data e hora atuais no momento da criação do registro.

5. **`updatedAt DateTime @default(now())`**:
   - `updatedAt`: Nome da coluna.
   - `DateTime`: Tipo de dados da coluna, semelhante ao `createdAt`.
   - `@default(now())`: Também define que o valor padrão é a data e hora atuais. Normalmente, essa coluna é atualizada cada vez que o registro é modificado.

Resumindo, este modelo `Poll` define uma tabela com quatro colunas: `id`, `title`, `createdAt` e `updatedAt`, onde `id` é a chave primária e as colunas `createdAt` e `updatedAt` são automaticamente configuradas para registrar as datas de criação e atualização de cada registro na tabela. Este modelo é útil para gerenciar enquetes, com cada registro representando uma enquete diferente.

##### Criando a tabela no banco de dados

Após configurar o arquivo `schema.prisma`, será necessário rodar um comando no terminal que irá criar efetivamente no banco de dados, no terminal digite o seguinte:

```bash
npx prisma migrate dev
```

Quando você executar esse comando ele irá pegar novas informações inseridas no `schema.prisma`, no caso a tabela `Poll`, e vai te pedir um nome para essa migração, geralmente colocamos exatamente o que foi feito, então `create poll` (Pode ser escrito de qualquer forma que descreva bem o que fez, para ser mais facilmente encontrado e entendido o que é essa migração.).

Veja que após rodar esse comando, foi criado uma tabela `migrations`, nele você observará que terá lá uma pasta com o nome que você deu e com o `sql`.

#### Acessando a interface do Prisma

Para acessar a interface do Prisma é muito simples, basta que digite o seguinte comando:

```bash
npx prisma studio
```

Será aberto no seu navegador, uma interface web onde você pode manipular o banco de dados da sua aplicação.

#### Criando uma rota para acessar o banco de dados

Vamos modificar a rota que tínhamos antes, ela por enquanto ficará da seguinte forma:

```typescript
app.post('/polls', (req) => {
  console.log(req.body);

  return req.body
})
```

Basicamente, estamos criando uma rota que será para criação de enquetes, como é criação, então usamos o método `POST`. O nome da rota foi dado como `polls` no plural, porém pode ser no singular, o que melhor agradar o seu gosto pessoal. O que for enviado pelo `body` da `request`, será mostrado no terminal através do `console.log` e na `response` também retornará o corpo da requisição.

Isso, obviamente é apenas para iniciar, mas vamos dar muito mais corpo para essa requisição, e para isso iremos utilizar uma biblioteca que valida as informações enviadas na requisição.

#### Instalando e configurando o Zod

Para instalar essa biblioteca que valida as informações que serão passadas na requisição, rode o seguinte comando:

```bash
npm install zod
```

O Zod nos permite *tipar* (definir o tipo), além de podermos declarar os parâmetros que podemos aceitar da requisição, para isso, no topo do arquivo adicionamos o import do zod:

```js
import { z } from 'zod'
```

E então dentro da nossa rota, vamos fazer a seguinte configuração:

```js
app.post('/polls', (req) => {
  const createPollBody = z.object({
    title: z.string()
  })

  const { title } = createPollBody.parse(req.body)

  return req.body
})
```

Como o tipo dos dados que iremos enviar na requisição é um objeto, então utilizamos o `z.object()` para isso e dentro dele iremos apontar os parâmetros e o tipo deles. E então, fazemos o `parse` desse objeto, passando o `body` da requisição, ou seja, o Zod irá validar se aquele objeto é igual ao objeto que foi definido, e o mais bacana é que o Visual Studio Code conseguirá já deduzir os dados dessa request, pois foi configurado isso no `createPollBody`. Para ver isso, basta pressionar o atalho `CTRL + ESPAÇO` dentro das chaves, e verá que aparecerá `title` como uma sugestão.

#### Adicionando a enquete no banco de dados

É bastante simples adicionar no banco de dados, basta no início do arquivo adicionar o seguinte import:

```typescript
import { PrismaClient } from "@prisma/client"
```

Depois mais abaixo, no caso adicionei abaixo da inicialização do Fastify, criar a variável da inicialização do Prisma, que ficou assim:

```typescript
const prisma = new PrismaClient()
```

E então, dentro da rota, vamos transformar a `arrow function` em uma função assíncrona, já que a resposta da criação no banco de dados é algo que leva um tempo, portanto é uma Promise, e também adicionar o segundo parâmetro para a `response` dessa rota, que no fastify é chamado de `reply`, ficando da seguinte forma:

```typescript
app.post('/polls', async (req, reply) => {
// [...]
}
```

Agora iremos criar a enquete dentro da tabela `poll`, e após a criação será retornado o `id` da enquete criada e para que o status code seja correto, é passado como `201`, então essa rota ficará da seguinte forma:

```typescript
app.post('/polls', async (req, reply) => {
  const createPollBody = z.object({
    title: z.string()
  })

  const { title } = createPollBody.parse(req.body)

  const poll = await prisma.poll.create({
    data: {
      title
    }
  })

  return reply.status(201).send({
    pollId: poll.id
  })
})
```

É interessante notar que na variável `prisma` fica tudo corretamente salvo, então quando você digita `prisma`, a Intelisense já irá sugerir a tabela criada que é a tabela `poll`, e como iremos criar algo, é utilizado o método `create`, dentro desse método temos um objeto, onde podemos passar o parâmetro `data`, dentro desse objeto, ao pressionar o atalho `CTRL + ESPAÇO`, você terá a indicação de todas as colunas da tabela `poll`, porém como todas as outras já possuem um `default`, então passamos apenas o `title`, e já que é o exato mesmo nome da variável que foi armazenado o título, escrevemos apenas `title` ao invés de ter que ser `title: title`.

### Aula 2

A decolagem foi um sucesso e agora é hora de avançar. Vamos dar sequência à criação das nossas rotas! Vamos adicionar as opções da votação e, em seguida, vamos implementar as rotas de buscar uma votação pelo ID e a rota para votar na opção selecionada, validando as lógicas de ação do usuário.

Vamos começar criando uma pasta em `src` chamada `lib` e dentro dela um arquivo com o nome `prisma.ts`, nele iremos configurar o Prisma. O código dele ficará da seguinte forma:

```typescript
import { PrismaClient } from "@prisma/client"

export const prisma = new PrismaClient()
```

Basicamente, será para podermos importar o Prisma em qualquer lugar que quisermos acessar o banco de dados.

Vamos também limpar temporariamente o arquivo `server.ts`, pois vamos organizar o projeto seguindo boas práticas, basicamente removemos alguns imports e a rota também, então copie essa rota pois iremos utilizar no próximo passo, o código ficará da seguinte forma no `server.ts`:

```typescript
import fastify from 'fastify'

const app = fastify()

app.listen({ port: 3333 }).then(() => {
  console.log('HTTP server running!');
})
```

Claro que da forma que está não funcionará nada por enquanto, mas logo mais iremos adicionar algumas coisas que fará voltar a funcionar como deveria.

#### Organizando a estrutura do projeto

Para deixarmos o projeto mais organizado, iremos criar uma pasta `routes` dentro da pasta `http`, onde ficarão todos os arquivos de cada rota da API. Primeirament iremos criar o arquivo `create-poll.ts` e nele traremos o código da rota `POST` que tínhamos no arquivo `server.ts`.

O nosso arquivo `create-poll.ts` ficará da seguinte forma:

```typescript
import z from "zod"
import { prisma } from "../../lib/prisma"
import { FastifyInstance } from "fastify"

export async function createPoll(app: FastifyInstance) {
  app.post('/polls', async (req, reply) => {
    const createPollBody = z.object({
      title: z.string()
    })

    const { title } = createPollBody.parse(req.body)

    const poll = await prisma.poll.create({
      data: {
        title
      }
    })

    return reply.status(201).send({
      pollId: poll.id
    })
  })
}
```

Como estamos utilizando essa organização de projeto, é necessário que criemos uma função assíncrona e que irá receber o `app` criado no `server.ts`, lá iremos adicionar um método que se chama `register`, ou seja, irá registrar essa rota na API e passará o app para a função no primeiro parâmetro.

O `FastifyInstance` serve para que possamos *tipar* o parâmetro da função.

##### Atualizando o server.ts

Agora que configuramos o arquivo de rota, vamos registrar ele no `server.ts`, para isso faça o seguinte:

```typescript
import fastify from 'fastify'
import { createPoll } from './routes/create-poll';

const app = fastify()

app.register(createPoll)

app.listen({ port: 3333 }).then(() => {
  console.log('HTTP server running!');
})
```

Importamos a rota `createPoll` e registramos ela no app do Fastify.

#### Configurando um novo model

Vamos no arquivo `schema.prisma` e vamos criar um novo model que irá ser um relacionamento `1 <> N`, ou seja, uma única enquete, poderá ter `N` opções, para isso vamos criar um model chamado `PollOption`:

```prisma
model PollOption {
  id     String @id @default(uuid())
  title  String

  poll Poll
}
```

Ao adicionar essa configuração e salvar o arquivo, como adicionar o `formatOnSave`, assim que salvar o arquivo será formatado e fará o relacionamento sozinho, ficará algo assim:

```prisma
model Poll {
  id         String       @id @default(uuid())
  title      String
  createdAt  DateTime     @default(now())
  updatedAt  DateTime     @default(now())
  PollOption PollOption[]
}

model PollOption {
  id    String @id @default(uuid())
  title String

  poll   Poll   @relation(fields: [pollId], references: [id])
  pollId String
}
```

Apenas por questão de organização vamos reposicionar o `pollId` mais pra cima, junto com as outras propriedades e vamos renomear o `PollOption` para `options`, ficando assim:

```prisma
model Poll {
  id        String       @id @default(uuid())
  title     String
  createdAt DateTime     @default(now())
  updatedAt DateTime     @default(now())
  options   PollOption[]
}

model PollOption {
  id     String @id @default(uuid())
  title  String
  pollId String

  poll Poll @relation(fields: [pollId], references: [id])
}
```

##### Rodando a migração

Como fizemos antes, após criar o model, vamos fazer a migração, para isso rode o seguinte comando no termina:

```bash
npx prisma migrate dev
```

E no campo que pedirá um nome para essa migração, como sugestão, coloque algo como: `create poll options`

### Aula 02: By ChatGPT

#### Configuração Inicial

##### Estrutura de Pastas

- **Lib/Prisma**: 
  - Crie uma pasta chamada `Lib` e dentro dela, uma subpasta `Prisma`.
  - Essa estrutura centraliza a conexão com o banco de dados PostgreSQL.

##### Conexão com o Banco de Dados

- **Exportando Prisma Client**: 
  - No arquivo dentro de `Lib/Prisma`, importe e exporte o `PrismaClient`.
  - Exemplo:

    ```javascript
    import { PrismaClient } from '@prisma/client';
    export const prisma = new PrismaClient();
    ```

#### Organização das Rotas

##### Estruturação de Rotas

- **Pasta de Rotas**:
  - Dentro da pasta `http`, crie uma subpasta chamada `Routes`.
  - Cada rota da aplicação terá um arquivo separado nesta pasta.

##### Criação de Arquivos de Rotas

- **Funções de Rota**:
  - Cada arquivo na pasta `Routes` deve exportar uma função assíncrona.
  - A função deve receber `app` (instância do Fastify) como parâmetro.
  - Exemplo de Estrutura de Rota:

    ```javascript
    async function routes(app) {
      app.get('/example', async (request, reply) => {
        // Lógica da rota
      });
    }
    export default routes;
    ```

#### Modelo de Dados para Votação

##### Tabela `PollOption`

- **Criação do Modelo**:
  - No esquema do Prisma, adicione o modelo `PollOption`.
  - Inclua campos como `id`, `title`, e `createdAt`.
  - Estabeleça um relacionamento com a tabela `Poll`.

##### Relacionamento com Tabela `Poll`

- **Definindo Relações**:
  - No modelo `PollOption`, defina a relação com a tabela `Poll`.
  - Utilize as funcionalidades do Prisma para criar e gerenciar essas relações.

#### Implementação das Rotas

##### Rota de Criação de Enquete

- **Rota `/create-poll`**:
  - Implemente uma rota POST para criar novas enquetes.
  - A rota deve aceitar um título de enquete e opções como parte da requisição.

##### Rota de Detalhes da Enquete

- **Rota `/get-poll`**:
  - Implemente uma rota GET para obter detalhes de uma enquete específica.
  - Utilize a funcionalidade `include` do Prisma para obter detalhes das opções relacionadas.

#### Lógica de Votação e Cookies

##### Identificação Única de Usuários

- **Uso de Cookies**:
  - Utilize `fastify-cookie` para gerenciar cookies.
  - Crie e armazene um `sessionID` em um cookie para cada usuário.

##### Implementação da Rota de Votação

- **Rota `/vote-on-poll`**:
  - Crie uma rota POST para registrar votos em uma opção específica.
  - Verifique se um usuário já votou para evitar duplicação de votos.

#### Estruturação da Tabela de Votos

##### Modelo `Vote`

- **Criação do Modelo de Votos**:
  - Adicione o modelo `Vote` ao esquema Prisma.
  - Estabeleça relações com `Poll` e `PollOption`.

##### Constraint de Unicidade

- **Unique Constraint**:
  - Garanta que a combinação `sessionID` e `pollID` seja única na tabela `Vote`.

### Aula 03: By ChatGPT

O próximo nível está cada vez mais próximo! Para finalizar nosso projeto, vamos adicionar o Redis para lidar com o cache dos votos e organizar o ranking das opções mais votadas. Também vamos implementar a funcionalidade de realtime utilizando websockets.

#### Introdução à Última Aula

##### Objetivos da Aula

- Finalização da aplicação.
- Aprendizado sobre o uso de Redis para caching de votação.
- Comunicação em tempo real entre backend e frontend usando WebSockets.

#### Implementação do Redis para Caching

##### Configuração do Redis

- **Uso do Redis**: Redis é utilizado para armazenar a contagem de votos de cada enquete, proporcionando uma resposta mais rápida comparada à consulta direta no banco de dados PostgreSQL.

##### Estrutura de Dados no Redis

- **ZSETs para Ranking**: Uso de estruturas de dados `ZSETs` (Sorted Sets) no Redis para armazenar e ordenar votos.

#### Comunicação em Tempo Real com WebSockets

##### Introdução ao WebSocket

- **WebSocket vs HTTP**: Diferenciação entre o protocolo WebSocket, utilizado para comunicação em tempo real, e o protocolo HTTP tradicional.

##### Configuração do WebSocket no Fastify

- **Instalação do Módulo `fastify-websocket`**: Uso do módulo para habilitar a funcionalidade WebSocket no Fastify.

#### Implementação do Sistema de Votação com Redis e WebSocket

##### Atualização dos Votos com Redis

- **Atualização e Sincronização**: Ao receber um voto, atualizar a contagem no Redis e sincronizar essa informação com o frontend em tempo real.

##### Envio de Dados via WebSocket

- **Envio de Atualizações de Votos**: Utilização do WebSocket para enviar os dados atualizados de votos para o frontend.

#### Estrutura de Pub/Sub para Eventos de Votação

##### Criação de Canais de Eventos

- **Canais por Enquete**: Cada enquete terá seu próprio canal para envio de eventos de votação.

##### Inscrição e Publicação em Canais

- **Mecanismo de Pub/Sub**: Implementação de um mecanismo de publicação e inscrição para gerenciar os eventos de votos das enquetes.