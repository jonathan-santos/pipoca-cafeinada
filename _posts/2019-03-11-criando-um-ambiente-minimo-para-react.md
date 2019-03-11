---
layout: post
title:  "Criando um ambiente mínimo para React com Webpack 4 e Babel 7"
tags: dev web react js
---

## Introdução

Olá e seja bem vindo novamente ao Pipoca Cafeinada, hoje teremos Web com React, mais especificamente, como criar um ambiente de desenvolvimento de React mínimo  que é muito bom pra entender como tudo o stack para React (React, NPM, Babel, Webpack) funciona. Então vamos começar:

Antes de mais nada, para se usar o React de maneira proficiente, é essencial aprender e usar o [npm][npm], para baixarmos e organizarmos as dependências e scripts do projeto, mas está fora do escopo desse post explicar o que é o NPM, para isso veja um tutorial em [português][npm-tutorial-pt] ou [inglês][npm-tutorial-en]. Outra coisa que faremos hoje será utilizar a linha de comando, novamente fora do escopo do post, se não se sentir a vontade com isso, aprenda em [português][cmd-tutorial-pt] ou [inglês][cmd-tutorial-en]. Quanto aos comandos de linha de comando que usarei neste post, são todos comandos `bash`, você ainda pode criar os arquivos e pastas usando um explorador de arquivos, mas ainda precisará da linha de comando para usar os comandos npm, então é, pratique a linha de comando, pelo menos os comandos básicos.

## Criando a estrutura básica do projeto

Primeiramente vamos criar uma pasta que onde nossa projeto residirá, então:
{% highlight bash %}
cd Desktop
mkdir projeto-react
{% endhighlight %}

Então criaremos o package.json e já iremos instalar os pacotes essenciais para este projeto, que no caso são os pacotes `react` e `react-dom`:
{% highlight bash %}
npm init -y
npm install react react-dom
{% endhighlight %}

## Criando o /dist

Primeiro nós criaremos a pasta que guardará os arquivos públicos do nosso projeto. Para isso criaremos a pasta `/dist`, onde ficarão salvos os arquivos que serão públicos ao usuário, como o `index.html`:
{% highlight bash %}
mkdir dist
touch dist/index.html
{% endhighlight %}

Preencha o `index.html` com o seguinte código:
{% highlight html %}
<!DOCTYPE HTML>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Projeto React</title>
</head>
<body>
    <div class="app"></div>
    <script src="./main.js"></script>
</body>
</html>
{% endhighlight %}

## Criando o /src

Crie a pasta `/src`, onde estará o código do nosso projeto, de início, teremos o `index.js` (ponto de entrada da nossa aplicação React) e o `app.jsx` (Componente base da aplicação):
{% highlight bash %}
mkdir src
touch src/index.js
touch src/app.js
{% endhighlight %}

Preencha o `index.js` com o seguinte código:
{% highlight javascript %}
import React from 'react';
import ReactDOM from 'react-dom';
import App from './app';

ReactDOM.render(
    <App />,
    document.querySelector('.app')
);
{% endhighlight %}

Preencha o `app.js` com o seguinte código:
{% highlight javascript %}
import React from 'react';

export default class App extends React.Component {
    render() {
        return (
            <h1>Olá mundo</h1>
        );
    }
}
{% endhighlight %}

## Babel e Webpack

Agora, instalaremos e configuraremos o Babel e o Webpack. Nós usaremos o Webpack para converter o código ES6 e React usados dentro do código js e jsx na pasta `src` para ES5, permitindo com que a nossa aplicação rode em mais navegadores. E quanto ao Webpack, ele serve para criar "bundles", ou pacotes, no nosso caso, empacotaremos o código inteiro da nossa aplicação React já convertido para ES5 em um único arquivo que o nosso `index.html` usará.

Instale o babel:
{% highlight bash %}
npm install --save-dev @babel/core @babel/preset-env @babel/preset-react
{% endhighlight %}

Crie o arquivo de configuração do babel, o `.babelrc`, na raiz do projeto:
{% highlight bash %}
touch .babelrc
{% endhighlight %}

Coloque o seguinte código no `.babelrc`:
{% highlight javascript %}
{
    "presets": ["@babel/preset-env", "@babel/npreset-react"]
}
{% endhighlight %}

Agora instale o Webpack:
{% highlight bash %}
npm install --save-dev webpack webpack-cli babel-loader
{% endhighlight %}

Crie o arquivo de configuração do webpack, o `webpack.config.js`, na raiz do projeto:
{% highlight bash %}
touch webpack.config.js
{% endhighlight %}

Coloque o seguinte código no `webpack.config.js`:
{% highlight javascript %}
module.exports = {
    module: {
        rules: [
            {
                test: /\.(js|jsx)$/,
                exclude: /node_modules/,
                use: {
                    loader: "babel-loader"
                }
            }
        ]
    }
}
{% endhighlight %}

Terminado de realizar estes passos, seu projeto deve estar parecido com o seguinte:
![Estrutura do projeto](/assets/img/estruturaAmbienteMinimoReact.jpg)

## NPM Script e .gitignore

Agora que terminamos instalar e configurar toda a estrutura do projeto, vamos criar um NPM Script que quando executado cria o bundle final do JS do nosso projeto, que estará em `/dist`. Então em `package.json` crie ou adicione o seguinte valor na propriedade scripts:
{% highlight javascript %}
//...
"scripts": [
    "build": "webpack --mode production"
]
//...
{% endhighlight %}

O comando build que criamos compilará o arquivo `main.js` dentro de `/dist`:
{% highlight bash %}
npm run build
{% endhighlight %}

O Webpack irá criar um bundle de todo os seus arquivos .js e .jsx dentro de `/src` no arquivo `main.js` que estará dentro da pasta `/dist`. Então se você abrir o seu arquivo `index.html` dentro de `/dist`, você deverá ver:
![Resultado da aplicação executada](/assets/img/resultadoAmbienteMinimoReact.jpg)

E para terminar, criaremos o `.gitignore` na raiz do projeto, para quem usa o git em seus projetos:
{% highlight bash %}
touch .gitignore
{% endhighlight %}

Coloque o seguinte código no `.gitignore`:
{% highlight gitignore %}
/node_modules/
/dist/main.js
{% endhighlight %}

Um repositório com o resultado final do passo a passo deste post está disponível [aqui](https://github.com/jonathan-santos/react-ambiente-minimo).

<br>

E é isso por hoje, há muito mais que podemos adicionar a o nosso projeto para facilitar ainda mais o desenvolvimento dos nosso projetos como um servidor de hot-reloading para desenvolvimento, compilação de sass para css com o webpack e até um simples servidor node + express que mandará a nossa aplicação ao ar, mas isso fica para uma próxima vez. Espero que tenha gostado e até a próxima!

<br> 

Música do post: [Adam Joseph ft. Lady Gaga - 100 PEOPLE][musica-do-post], encontrei essa música no twitter esses dias atrás junto com um vídeo de umas garotas dançando muito bem, [se divirta][origem-musica-do-post].

[npm]: "https://npmjs.com/"
[npm-tutorial-pt]: "https://medium.com/tableless/criando-o-meu-novo-site-4-utilizando-npm-para-instala%C3%A7%C3%A3o-de-pacotes-6c7cea2ab4b3"
[npm-tutorial-en]: "https://medium.com/beginners-guide-to-mobile-web-development/introduction-to-npm-and-basic-npm-commands-18aa16f69f6b"
[cmd-tutorial-pt]: "https://tutorial.djangogirls.org/pt/intro_to_command_line/"
[cmd-tutorial-en]: "https://tutorial.djangogirls.org/en/intro_to_command_line/"
[musica-do-post]: "https://www.youtube.com/watch?v=4Tyval51ML4"
[origem-musica-do-post]: "https://twitter.com/GagaDelGrey/status/1101940272571842564" 