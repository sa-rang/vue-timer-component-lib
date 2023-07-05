[![Netlify Status](https://api.netlify.com/api/v1/badges/b1b84831-789e-4629-a9e3-55a36e136653/deploy-status)](https://app.netlify.com/sites/sharp-babbage-154f0a/deploys)

# Vue Timer Component Library 

> The Vue Timer Component Library contains ready to use timer components [Example/Demo](https://v3.vuejs.org/) .

## Setup



```bash
# install dependencies
npm install

# start the doc app with hot reload, great for testing components
npm run docs:dev

# build the library, available under dist
npm run build

# build the doc app, available under docs/.vitepress/dist
npm run docs:build

# preview the doc app locally from docs/.vitepress/dist
npm run docs:serve
```

You may use [Netlify](https://www.netlify.com/) to auto build and deploy the doc app like this project does.


## How it works



#### TypeScript

In [tsconfig.json](tsconfig.js), set the following as recommended by Vite (since esbuild is used). However, enableing this option leads to https://github.com/vitejs/vite/issues/5814. The workaround is to also enable `compilerOptions.skipLibCheck`.

```json
"compilerOptions": {
  "isolatedModules": true
}
```

In [tsconfig.json](tsconfig.js), set the following to address [Issue #32](https://github.com/wuruoyun/vue-component-lib-starter/issues/32). The solution is from https://github.com/johnsoncodehk/volar/discussions/592.

```json
"compilerOptions": {
  "types": [
    "vite/client"
  ]
}
```
