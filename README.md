hi.. this is a beginner redux tutotial for creating a counter app. Follow the steps and make sure you have node installed.

1.	Create a new project. Open an empty folder in vscode
mkdir redux-webpack-app
cd redux-webpack-app
2.	Initialize a new project with a package.json file.
npm init -y
3.	Install Required Dependencies
Install Webpack & Babel
npm install --save-dev webpack webpack-cli webpack-dev-server babel-loader @babel/core @babel/preset-env @babel/preset-react html-webpack-plugin css-loader style-loader

Install React & Redux
npm install react react-dom react-redux redux @reduxjs/toolkit
4.	Set Up Webpack Configuration
Create a file called webpack.config.js in the root of your project and add the following:
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  mode: "development",
  entry: "./src/index.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "bundle.js",
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
        },
      },
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"],
      },
    ],
  },
  resolve: {
    extensions: [".js", ".jsx"],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./public/index.html",
    }),
  ],
  devServer: {
    static: {
      directory: path.join(__dirname, "dist"),
    },
    port: 3000,
    historyApiFallback: true,
  },
};




5.	Set Up Babel Configuration
Create a file called .babelrc and add:
	{
    "presets": ["@babel/preset-env", "@babel/preset-react"]
  }
  


6.	Set Up React + Redux Files
Create Folder Structure
src public
src/index.js
 src/App.js 
src/store.js 
public/index.html
7.	In public/index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Redux Webpack App</title>
</head>
<body>
    <div id="root"></div>
</body>
</html>

8.	In src/index.js
import React from "react";
import { createRoot } from "react-dom/client";
import { Provider } from "react-redux";
import store from "./store";
import App from "./App";

const root = createRoot(document.getElementById("root"));
root.render(
  <Provider store={store}>
    <App />
  </Provider>
);


9.	In src/App.js
import React from "react";
import { useSelector, useDispatch } from "react-redux";
import { increment, decrement } from "./store";

function App() {
  const count = useSelector((state) => state.counter.value);
  const dispatch = useDispatch();

  return (
    <div>
      <h1>Redux + Webpack</h1>
      <p>Count: {count}</p>
      <button onClick={() => dispatch(increment())}>+</button>
      <button onClick={() => dispatch(decrement())}>-</button>
    </div>
  );
}

export default App;


10.	in src/store.js
import { configureStore, createSlice } from "@reduxjs/toolkit";

const counterSlice = createSlice({
  name: "counter",
  initialState: { value: 0 },
  reducers: {
    increment: (state) => { state.value += 1; },
    decrement: (state) => { state.value -= 1; },
  },
});

export const { increment, decrement } = counterSlice.actions;

const store = configureStore({
  reducer: { counter: counterSlice.reducer },
});

export default store;



11.	Finally, Add Scripts to package.json
Open package.json and update the scripts section:
 "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack serve --open --mode development",
    "build": "webpack --mode production"
  },

Then run the code
  npm start
