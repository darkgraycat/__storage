1. install needed packages
```
mkdir MyProject
cd MyProject
npm init
npm i -D typescript webpack webpack-cli ts-loader
```

2. in `package.json` add scripts:
```
"start": "webpack --mode development --watch",
"build": "webpack --mode production"
```

3. create in root directory `tsconfig.json` file with content:
```
{
  "compilerOptions": {
    "target": "es5",
    "module": "es6",
    "strict": true,
    "outDir": "dist",
    "sourceMap": true
  }
}
```

4. create in root directory `webpack.config.js` file with content:
```
const path = require('path')

module.exports = {
  entry: './src/index.ts',
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: 'ts-loader',
        exclude: /node_modules/,
      },
    ],
  },
  resolve: {
    extensions: ['.tsx', '.ts', '.js'],
  },
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
}
```

5. create `.src/index.ts` with your TypeScript code

6. create `./index.html` with script tag:
```
<script src='./dist/bundle.js'></script>
```

7. start development with single command:
```
npm start
```
