# REACT CONFIGURATION WITH WEBPACK


## Setting up the project

1. Create a directory for the project:

    `mkdir webpack-react`

2. Create a minimal directory structure inside webpack-react for holding the code:

    `mkdir src`

3. Initialize the project by running:

    `npm init`


***
## Setting up webpack

1. Install webpack and webpack-cli by running:

    `npm i webpack webpack-cli --save-dev`

2. Now add the webpack command inside package.json:

    `"scripts": { "build": "webpack --mode production" }`


***
## Setting up Babel

1. Install this dependencies:
   
    `npm i @babel/core babel-loader @babel/preset-env @babel/preset-react --save-dev`

2. Create a new file named .babelrc inside the project folder with the following code:

~~~
`{
  "presets": ["@babel/preset-env", "@babel/preset-react"]
 }`
~~~

3. Create a file named webpack.config.js and fill it like so:

~~~
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
};
~~~


***
## Create React components

1. Install React and React Dom

    `npm i react react-dom`

2. Create a minimal directory structure for holding the component:

    `mkdir -p src/components/`

3. Create the component in src/components/Form.js:
   
~~~
import React, { Component } from "react";
import ReactDOM from "react-dom";

class Form extends Component {
  constructor() {
    super();

    this.state = {
      value: ""
    };

    this.handleChange = this.handleChange.bind(this);
  }

  handleChange(event) {
    const { value } = event.target;
    this.setState(() => {
      return {
        value
      };
    });
  }

  render() {
    return (
      <form>
        <input
          type="text"
          value={this.state.value}
          onChange={this.handleChange}
        />
      </form>
    );
  }
}

export default Form;
~~~

4. Create the file src/index.js and place an import directive into it for requiring your React component:

~~~
import Form from "./js/components/Form";
~~~

5. With this in place we're ready to create our bundle by running:
   
    `npm run build`

Give webpack a second and see the bundle come to life in dist/main.js.


***
## Set up React, webpack, and Babel: the HTML webpack plugin

1. Webpack needs two additional components for processing HTML: html-webpack-plugin and html-loader. Add the dependencies with:

    `npm i html-webpack-plugin html-loader --save-dev`

2. Update webpack's configuration:
   
~~~
const HtmlWebPackPlugin = require("html-webpack-plugin");

module.exports = {
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      },
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader"
          }
        ]
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: "./src/index.html",
      filename: "./index.html"
    })
  ]
};
~~~

3. Create an HTML file in src/index.html:
   
~~~
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <title>How to set up React, Webpack, and Babel</title>
    </head>
    <body>
        <div id="container"></div>
    </body>
</html>
~~~

4. Open up src/js/components/Form.js and add the following code at the bottom of the file:

~~~
const wrapper = document.getElementById("container");
wrapper ? ReactDOM.render(<Form />, wrapper) : false;
~~~

5. Now run the build again with:

    `npm run build`

Open up dist/index.html in your browser: you should see the React form!


***
## Configuring the webpack dev server

You don't want to type npm run build every time you change a file. It takes only 3 lines of configuration to have a development server up and running.

1. To set up webpack dev server install the package with:

    `npm i webpack-dev-server --save-dev`

2. Open up package.json and add the start script:

~~~
"scripts": {
  "start": "webpack-dev-server --open --mode development",
  "build": "webpack --mode production"
}
~~~

3. Now, by running:
   
    `npm start`

You should see webpack launching your application inside the browser.


***
## How to set up React, webpack, and Babel: wrapping up.

**create-react-app** is the way to go for starting off a new React project. Almost everything is configured out of the box. But sooner or later you may want to extend or tweak webpack a bit.