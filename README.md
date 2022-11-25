<p align="center"><img src="https://imgur.com/5fzMbyV.png" width="269"></p>

[![npm version](https://badge.fury.io/js/graphql-playground-react.svg)](https://badge.fury.io/js/graphql-playground-react) [![CircleCI](https://circleci.com/gh/prisma/graphql-playground.svg?style=shield)](https://circleci.com/gh/prisma/graphql-playground)

GraphQL IDE para melhores fluxos de trabalho de desenvolvimento (Assinaturas GraphQL, documentos interativos e colaboração). <br />
**Você pode baixar o [aplicativo para desktop](https://github.com/prisma/graphql-playground/releases) ou usar a versão web em graphqlbin.com: [Demo](https://graphqlbin.com/v2/6RQ6TM )**

[![](https://i.imgur.com/AE5W6OW.png)](https://graphqlbin.com/v2/6RQ6TM)

## Instalação

```sh
$ brew cask install graphql-playground
```

## Recursos

- ✨ Preenchimento automático sensível ao contexto e destaque de erros;
- 📚 Documentos interativos com várias colunas (suporte a teclado);
- ⚡️ Suporta assinaturas GraphQL em tempo real;
- ⚙ GraphQL Config support with multiple Projects & Endpoints;
- 🚥 Suporte ao Rastreamento Apollo.

## Perguntas frequentes

###Como isso é diferente do [GraphiQL](https://github.com/graphql/graphiql)?

O GraphQL Playground usa componentes do GraphiQL sob o capô, mas é um GraphQL IDE mais poderoso, permitindo melhores fluxos de trabalho de desenvolvimento (local). Comparado ao GraphiQL, o GraphQL Playground vem com os seguintes recursos adicionais:

- Documentação de esquema interativa e com várias colunas;
- Recarregamento automático do esquema;
- Suporte para assinaturas GraphQL;
- Histórico de consultas;
- Configuração de cabeçalhos HTTP;
- Guias.

Consulte a pergunta a seguir para obter mais recursos adicionais.

### Qual é a diferença entre o aplicativo para desktop e a versão web?

O aplicativo de desktop é o mesmo que a versão web, mas inclui estes recursos adicionais:

- Suporte parcial para [graphql-config](https://github.com/prismagraphql/graphql-config) permitindo recursos como configurações de vários ambientes (sem suporte para enviar cabeçalhos HTTP).
- Clique duas vezes em `*.graphql` files.

### Como funciona o GraphQL Bin?

Você pode facilmente compartilhar seus Playgrounds com outras pessoas clicando no botão "Compartilhar" e compartilhando o link gerado. Você pode pensar no GraphQL Bin como Pastebin para suas consultas GraphQL, incluindo o contexto (endpoint, cabeçalhos HTTP, abas abertas, etc.).

<a href="https://graphqlbin.com/OksD" target="_blank">
 <img src="https://camo.githubusercontent.com/daf8c64dbde3097fdbe782c0645552550d530a73/68747470733a2f2f696d6775722e636f6d2f48316e36346c4c2e706e67" alt="" data-canonical-src="https://imgur.com/H1n64lL.png" style="max-width:100%;">
</a>

> Você também pode encontrar a postagem no blog do anúncio [aqui](https://blog.graph.cool/introducing-graphql-playground-f1e0a018f05d).
> 
## Definições

No canto superior direito da janela do Playground, você pode clicar no ícone de configurações.
Estas são as configurações atualmente disponíveis:

```js
{
  'editor.cursorShape': 'line', // possible values: 'line', 'block', 'underline'
  'editor.fontFamily': `'Source Code Pro', 'Consolas', 'Inconsolata', 'Droid Sans Mono', 'Monaco', monospace`,
  'editor.fontSize': 14,
  'editor.reuseHeaders': true, // new tab reuses headers from last tab
  'editor.theme': 'dark', // possible values: 'dark', 'light'
  'general.betaUpdates': false,
  'prettier.printWidth': 80,
  'prettier.tabWidth': 2,
  'prettier.useTabs': false,
  'request.credentials': 'omit', // possible values: 'omit', 'include', 'same-origin'
  'schema.polling.enable': true, // enables automatic schema polling
  'schema.polling.endpointFilter': '*localhost*', // endpoint filter for schema polling
  'schema.polling.interval': 2000, // schema polling interval in ms
  'schema.disableComments': boolean,
  'tracing.hideTracingResponse': true,
}
```

## Uso

### Propriedades

O componente React `<Playground />` e todos os middlewares expõem as seguintes opções:

- `props` (Middlewares & React Component)
  - `endpoint` [`string`](optional) - the GraphQL endpoint url.
  - `subscriptionEndpoint` [`string`](optional) - the GraphQL subscriptions endpoint url.
  - `workspaceName` [`string`](optional) - in case you provide a GraphQL Config, you can name your workspace here
  - `config` [`string`](optional) - the JSON of a GraphQL Config. See an example [here](https://github.com/prismagraphql/graphql-playground/blob/master/packages/graphql-playground-react/src/localDevIndex.tsx#L47)
  - `settings` [`ISettings`](optional) - Editor settings in json format as [described here](https://github.com/prismagraphql/graphql-playground#settings)

```ts
interface ISettings {
  'editor.cursorShape': 'line' | 'block' | 'underline'
  'editor.fontFamily': string
  'editor.fontSize': number
  'editor.reuseHeaders': boolean
  'editor.theme': 'dark' | 'light'
  'general.betaUpdates': boolean
  'prettier.printWidth': number
  'prettier.tabWidth': number
  'prettier.useTabs': boolean
  'request.credentials': 'omit' | 'include' | 'same-origin'
  'schema.polling.enable': boolean
  'schema.polling.endpointFilter': string
  'schema.polling.interval': number
  'schema.disableComments': boolean
  'tracing.hideTracingResponse': boolean
}
```

- `schema` [`IntrospectionResult`](optional) -O resultado de uma consulta de introspecção (um objeto desta forma: `{__schema: {...}}`) O playground busca automaticamente o esquema do terminal. Isso só é necessário quando você deseja substituir o esquema.
- `tabs` [`Tab[]`](optional) - Uma matriz de guias para injetar. **Observação: ao usar esse recurso, as guias serão redefinidas sempre que a página for recarregada**

```ts
interface Tab {
	endpoint: string
	query: string
	name?: string
	variables?: string
	responses?: string[]
	headers?: { [key: string]: string }
}
```

Além disso, o aplicativo React fornece mais algumas propriedades:

- `props` (React Component)
- `createApolloLink` [`(session: Session, subscriptionEndpoint?: string) => ApolloLink`] -isso é o equivalente ao `fetcher` do GraphiQL. Para cada consulta que está sendo executada, esta função será chamada

`createApolloLink` só está disponível no componente React e não nos middlewares, porque o conteúdo deve ser serializável à medida que é impresso em um modelo HTML.

### Como Página HTML

Se você simplesmente deseja renderizar o HTML do Playground por conta própria, por exemplo, ao implementar um GraphQL Server, existem 2 opções para você:

1.  [O HTML mínimo necessário para renderizar o Playground](https://github.com/prismagraphql/graphql-playground/blob/master/packages/graphql-playground-html/minimal.html)
2.  [O Playground HTML com animação de carregamento completo](https://github.com/prismagraphql/graphql-playground/blob/master/packages/graphql-playground-html/withAnimation.html)

Observação: caso você não queira servir ativos de um CDN (como jsDelivr) e, em vez disso, usar uma cópia local, será necessário instalar `graphql-playground-react` do npm e, em seguida, substituir todas as instâncias de `//cdn .jsdelivr.net/npm` com `./node_modules`. Um exemplo pode ser encontrado [aqui](https://github.com/prismagraphql/graphql-playground/blob/master/packages/graphql-playground-html/minimalWithoutCDN.html)

### Como Componente de Reação

#### Instalação

```sh
yarn add graphql-playground-react
```

#### Uso

O GraphQL Playground fornece um componente React responsável por renderizar a interface do usuário e o gerenciamento de sessão.
Existem **3 dependências** necessárias para executar o componente React `graphql-playground-react`.

1.  _Open Sans_ and _Source Code Pro_ fonts
2.  Rendering the `<Playground />` component

O GraphQL Playground requer **React 16**.

Incluindo Fontes (`1.`)

```html
<link
	href="https://fonts.googleapis.com/css?family=Open+Sans:300,400,600,700|Source+Code+Pro:400,700"
	rel="stylesheet"
/>
```

Incluindo a folha de estilo e o componente (`2., 3.`)

```js
import React from 'react'
import ReactDOM from 'react-dom'
import { Provider } from 'react-redux'
import { Playground, store } from 'graphql-playground-react'

ReactDOM.render(
	<Provider store={store}>
		<Playground endpoint="https://api.graph.cool/simple/v1/swapi" />
	</Provider>,
	document.body,
)
```

### Como middleware de servidor

#### Instalação

```sh
# Pick the one that matches your server framework
yarn add graphql-playground-middleware-express  # for Express or Connect
yarn add graphql-playground-middleware-hapi
yarn add graphql-playground-middleware-koa
yarn add graphql-playground-middleware-lambda
```

#### Uso com exemplo

Temos um exemplo completo para cada um dos frameworks abaixo:

- **Expresso:** Veja [packages/graphql-playground-middleware-express/examples/basic](https://github.com/prismagraphql/graphql-playground/tree/master/packages/graphql-playground-middleware-express/examples/basic)

- **Hapi:** Ver [packages/graphql-playground-middleware-hapi](https://github.com/prismagraphql/graphql-playground/tree/master/packages/graphql-playground-middleware-hapi)

- **Koa:** See [packages/graphql-playground-middleware-koa](https://github.com/prismagraphql/graphql-playground/tree/master/packages/graphql-playground-middleware-koa)

- **Lambda (as serverless handler):** See [serverless-graphql-apollo](https://github.com/serverless/serverless-graphql-apollo) or a quick example below.

### Como manipulador sem servidor

#### Instalação

```sh
yarn add graphql-playground-middleware-lambda
```

#### Uso

`handler.js`

```js
importar lambdaPlayground de 'graphql-playground-middleware-lambda'
// or using require()
// const lambdaPlayground = require('graphql-playground-middleware-lambda').default

exports.graphqlHandler = function graphqlHandler(event, context, callback) {
	function callbackFilter(error, output) {
		// eslint-disable-next-line no-param-reassign
		output.headers['Access-Control-Allow-Origin'] = '*'
		callback(error, output)
	}

	const handler = graphqlLambda({ schema: myGraphQLSchema })
	return handler(event, context, callbackFilter)
}

exports.playgroundHandler = lambdaPlayground({
	endpoint: '/dev/graphql',
})
```

`serverless.yml`

```yaml
functions:
  graphql:
    handler: handler.graphqlHandler
    events:
      - http:
          path: graphql
          method: post
          cors: true
  playground:
    handler: handler.playgroundHandler
    events:
      - http:
          path: playground
          method: get
          cors: true
```

## Desenvolvimento

```sh
$ cd packages/graphql-playground-react
$ yarn
$ yarn start
```

Open
[localhost:3000/localDev.html?endpoint=https://api.graph.cool/simple/v1/swapi](http://localhost:3000/localDev.html?endpoint=https://api.graph.cool/simple/v1/swapi) for local development!

## Tema personalizado

De `graphql-playground-react@1.7.0` em diante, você pode fornecer uma propriedade `codeTheme` ao componente React para personalizar seu tema de cores.
Estas são as opções disponíveis:

```ts
export interface EditorColours {
	property: string
	comment: string
	punctuation: string
	keyword: string
	def: string
	qualifier: string
	attribute: string
	number: string
	string: string
	builtin: string
	string2: string
	variable: string
	meta: string
	atom: string
	ws: string
	selection: string
	cursorColor: string
	editorBackground: string
	resultBackground: string
	leftDrawerBackground: string
	rightDrawerBackground: string
}
```

### Versões

This is repository is a "mono repo" and contains multiple packages using [Yarn workspaces](https://yarnpkg.com/lang/en/docs/workspaces/). Please be aware that versions are **not** synchronised between packages. The versions of the [release page](https://github.com/graphcool/graphql-playground/releases) refer to the electron app.

### Pacotes

In the folder `packages` you'll find the following packages:

- `graphql-playground-electron`: Cross-platform electron app which uses `graphql-playground-react`
- `graphql-playground-html`: Simple HTML page rendering a version of `graphql-playground-react` hosted on JSDeliver
- `graphql-playground-middleware-express`: Express middleware using `graphql-playground-html`
- `graphql-playground-middleware-hapi`: Hapi middleware using `graphql-playground-html`
- `graphql-playground-middleware-koa`: Koa middleware using `graphql-playground-html`
- `graphql-playground-middleware-lambda`: AWS Lambda middleware using `graphql-playground-html`
- `graphql-playground-react`: Core of GraphQL Playground built with ReactJS

<a name="help-and-community" />

## Ajuda e comunidade [![Status do Slack](https://slack.prisma.io/badge.svg)](https://slack.prisma.io)

Join our [Slack community](http://slack.graph.cool/) if you run into issues or have questions. We love talking to you!

<p align="center"><a href="https://oss.prisma.io"><img src="https://imgur.com/IMU2ERq.png" alt="Prisma" height="170px"></a></p>
