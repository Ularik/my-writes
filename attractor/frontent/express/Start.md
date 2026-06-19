
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


**Для httponly coockeis**
```
npm install cookie-parser
npm install --save-dev @types/cookie-parser
npm i dotenv
npm install --save mongoose
npm install --save cors
npm install --save -D @types/cors
```


```
import dotenv from "dotenv";
dotenv.config();


import express from "express";
import cors from "cors";
import mongoose from "mongoose";
import usersRouter from "./routes/users";
import config from "./config";
import cookieParser from "cookie-parser";

  
const app = express();

app.use(
  cors({
    origin: "http://localhost:5173",
    credentials: true,
  }),
);

  
app.use(cookieParser());
app.use(express.json());
app.use(express.static(config.publicPath));
const port = 8001;
  
app.use("/users", usersRouter);
const run = async () => {
  await mongoose.connect(config.db);
  app.listen(port, () => {
    console.log(`Server started on ${port} port!`);
  });

  process.on("exit", () => {
    mongoose.disconnect();
  });
};

run().catch((err) => console.error(err));
```