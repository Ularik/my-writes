
```
npm init -y
npm install --save express
```

Теперь установим ts-node, а также сам typescript, типы для работы и nodemon:

  ```
  npm i -D @types/node @types/express typescript ts-node nodemon
  ```
- @types/node - библиотека, содержащая типизацию для стандартных библиотек Node.js
- @types/express - типизация для express
- typescript - библиотека для работы с TS
- ts-node - инструмент для запуска TS файлов напрямую, без предварительной транспиляции
- nodemon - инструмент для запуска приложения с возможностью перезапуска при изменении каких-либо файлов приложения (чтобы не перезапускать сервер вручную при разработке).

Инициализируем TypeScript:
```
npm install typescript
npx tsc --init
```


Также как и на прошлом занятии отредактируем файл package.json, чтобы работать через nodemon. 
```
  "scripts": {
    "dev": "nodemon index.ts"
  },
```


```
import express from 'express';

import productsRouter from "./routers/products";

  

const app = express();
app.use(express.json());   # возможность парсить json запросы
const port = 8000;

  

app.use('/products', productsRouter);

  

app.listen(port, () => {

  console.log(`Server started on ${port} port!`);

});
```