rustup target add wasm32-unknown-unknown
```bash
cargo init --lib hello-wasm
or
cargo new --lib hello-wasm
```
```bash
cargo install wasm-pack
```

```bash
cargo add wasm-bindgen@0.2.91
cargo add js-sys@0.3.68
cargo add image@0.24.9
```

[lib]
crate-type = ["cdylib"]

#사용되지 않은 종속성 제거
cargo install cargo-machete
cargo machete

```js
[package]
name = "hello-wasm"
version = "0.1.0"
authors = ["Your Name <you@example.com>"]
description = "A sample project with wasm-pack"
license = "MIT/Apache-2.0"
repository = "https://github.com/yourgithubusername/hello-wasm"
edition = "2018"

[lib]
crate-type = ["cdylib"]

[dependencies]
wasm-bindgen = "0.2"
```

# wasm npm deploy example
```bash
npm adduser

wasm-pack build --scope mynpmusername

cd pkg
npm publish --access=public
```

touch package.json
```json
{
  "scripts": {
    "serve": "webpack-dev-server"
  },
  "dependencies": {
    "@mynpmusername/hello-wasm": "^0.1.0"
  },
  "devDependencies": {
    "webpack": "^4.25.1",
    "webpack-cli": "^3.1.2",
    "webpack-dev-server": "^3.1.10"
  }
}
```

touch webpack.config.js
```js
const path = require("path");
module.exports = {
  entry: "./index.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "index.js",
  },
  mode: "development",
};
```

touch index.html
```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>hello-wasm example</title>
  </head>
  <body>
    <script src="./index.js"></script>
  </body>
</html>
```

touch index.js
```js
const js = import("./node_modules/@yournpmusername/hello-wasm/hello_wasm.js");
js.then((js) => {
  js.greet("WebAssembly");
});
```

```bash
npm install
npm run serve
```

# wasm npm cra example
```bash
npx create-react-app wasm_blockchain
cd wasm_blockchain
npm install ../hello-wasm/pkg
```

app.js
```js
import logo from './logo.svg';
import init, {hello} from "hello-wasm";
import React, {useEffect, useState} from "react";
import './App.css';

function App() {
  const [isWasmLoaded, setWasmLoaded] = useState(false);
  useEffect(() => {
    // Wasm 모듈 초기화
    init().then(() => {
      setWasmLoaded(true);
      // Rust 함수 호출
      hello("ssssss");
    });
  }, []);

  return (
    <div className="App">
      {isWasmLoaded ?
      <div>hello wasm</div>
    :
    <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
        </a>
      </header> }
    </div>
  );
}

export default App;

```