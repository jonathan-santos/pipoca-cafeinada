---
title:  Criando um ambiente mínimo para React com Webpack 4 e Babel 7 parte II
date: 2019-04-01
category: dev
tags:
- react
- web
---

## Introdução

Olá e seja bem vindo novamente ao Pipoca Cafeinada, hoje teremos Web com React, melhorando o nosso ambiente mínimo para react, adicionando css loading e hot module replacement á um servidor de desenvolvimento do webpack, melhorando muito a nossa experiência de desenvolvimento.

Antes de mais nada, caso não tenha visto a primeira parte deste post, acesse ele [aqui][primeira-parte-post], e caso queira seguir este tutorial, siga os passos da primeira parte ou clone o repositório do resultado do primeiro post [aqui][link-repositorio-1].

## Adicionando o WebpackDevServer
Vamos adicionar uma nova dependência ao nosso projeto, o `webpack-dev-server`, com ele podemos criar um servidor de desenvolvimento local que nos ajudará muito a desenvolver o nossas aplicações, então instale ele:
``` bash
npm install --save-dev webpack-dev-server
```

Então alteraremos o `webpack.config.js` para configurarmos nosso servidor webpack, para que tenha um port diferente, sempre seja aberto automaticamente no navegador, tenha Hot-reloading ativado e acesse o nosso conteúdo, para isso coloque o seguinte código logo depois da propriedade module:
``` javascript
//...
module {
    //...
},
devServer: {
    port: 9000,
    hot: true,
    open: true,
    contentBase: 'dist'
}
//...
```

E modifique o `package.json` para que tenha um novo NPM Script para que execute o nosso servidor de desenvolvimento webpack:
``` js
//...
"scripts": {
    "build": "webpack --mode production",
    "start": "webpack-dev-server"
}
//...
```

Agora, quando rodarmos o script `start` no nosso projeto, irá ser iniciado um servidor de desenvolvimento com Hot Reloading pro React!
``` bash
npm start
```


## Adicionando loading de CSS á página
Feito o servidor de desenvolvimento, agora falta algo muito importante para o nosso ambiente estar pronto para uso, hot reloading de CSS. Para isso nós usaremos loaders de css para webpack, que pegará todo o css que criarmos e o juntará á nossa aplicação final no arquivo `dist/main.js`. Então instale os 2 pacotes que precisaremos:
``` bash
npm install --save-dev style-loader css-loader
```

Modifique a propriedade module no `webpack.config.js` para que use nossos novos loaders:
``` javascript
//...
module: {
    rules: [
        {
            test: /\.(js|jsx)$/,
            exclude: /node_modules/,
            use: {
                loader: "babel-loader"
            },
        },
        {
            test: /\.css$/,
            use: [ "style-loader", "css-loader" ]
        }
    ]
},
//...
```

Feito isso, precisaremos de arquivos css para a nossa aplicação, então crie a pasta `css` dentro de `src` e dentro dessa nova pasta crie o arquivo `styles.css`:
``` bash
mkdir src/css
touch src/css/styles.css
```

Agora, para que nossa aplicação use os nossos estilos criados dentro de `styles.css`, precisamos o importar para algum ponto da nossa aplicação, então em `index.js` coloque o seguinte `import`:
``` javascript
//...
import App from './app.jsx';
import './css/styles.css';
//...
```

Isso importará o css para a nossa aplicação React e resultará em os estilos aparecerem no nosso bundle final, ou seja, será afetado pelo Hot Module Replacement!

Então rode o script `start`, acesse a página no navegador e em seu editor de texto modifique o componente `app.jsx` e o stylesheet `styles.css`, você perceberá que as alterações que fizer serão aplicadas na sua página aberta sem a necessidade de recarregar a página, o que será muito útil no desenvolvimento.

Um repositório com o resultado final do passo a passo deste post está disponível [aqui][link-repositorio-2].

É isso por hoje, espero que tire bastante proveito destas modificações no projeto, até a próxima!

Música do post: [The Black Keys - Tighten Up][musica-do-post], um colega de trabalho me apresentou esta música um dia destes, ela é muito boa e o clipe é muito divertido, aproveite também.

[link-repositorio-1]: https://github.com/jonathan-santos/react-ambiente-minimo/tree/1.0.1
[link-repositorio-2]: https://github.com/jonathan-santos/react-ambiente-minimo/tree/2.0.0
[musica-do-post]: https://youtu.be/mpaPBCBjSVc
[primeira-parte-post]: /2019/03/11/criando-um-ambiente-minimo-para-react.html