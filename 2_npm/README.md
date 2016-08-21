# React with npm
参照:http://mae.chab.in/archives/2765

 - npm 2.15.8
 - node v4.4.7

## 事前準備
```
mkdir react-project
cd react-project
npm init
touch app.js index.html build.js
```
※build.jsの中身はコンパイルされた結果  

index.html
```html
<!doctype html>
<html>
<head>
  <meta charset="utf-8">
  <title>react 0.14 sample</title>
</head>
<body>
  <div id="content"></div>
  <script src="build.js"></script>
</body>
</html>
```
app.js
```javascript
var React = require("react");
var ReactDOM = require("react-dom");

ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('content')
);
```
## 開発環境
```
npm install --save react react-dom
npm install --save-dev watchify
npm install --save-dev babelify
npm install --save-dev exorcist //for source map
npm install --save-dev babel-preset-react
//npm install --save-dev babel-preset-es2015 //want to use ES2015（ES6）
```

```
touch .babelrc
```
.babelrc
```
{
  "presets": ["react"]
}
```
or (if 'babel-preset-es2015' is installed)
```
{
  "presets": ["react", "es2015"]
}
```

package.json
```
{
  "name": "2_npm",
  "version": "1.0.0",
	"private": true,
  "scripts": {
		"watch": "watchify -t babelify ./app.js -o 'exorcist ./build.js.map > ./build.js' -d",
		"build": "NODE_ENV=production browserify ./app.js -t babelify -t envify | uglifyjs -c warnings=false > ./build.js",
		"build-win": "set NODE_ENV=production && browserify ./app.js -t babelify -t envify | uglifyjs -c warnings=false > ./build.js"
  }
}
```

開発時には以下を実行
```
npm run watch
```

## 製品ビルド環境
```
npm install --save-dev envify
npm install --save-dev uglify-js
npm install --save-dev browserify
```

### ビルド
```
npm run build
```
### デバッグ用のコード
only development code
```
if (process.env.NODE_ENV !== 'production') {
  console.log('development only')
}
```
envify change the code when development
```
if ('production' !== 'production') {
  console.log('development only')
}
```
envify delese the code when release.

~~if ('production' !== 'production') {
  console.log('development only')
}~~
