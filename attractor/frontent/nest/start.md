```
npm i -g @nestjs/cli
nest new treck-api
cd treck-api
```

```
npm run start:dev
```

#### Создать новый контроллер

```
nest g controller trecks --no-spec
```

#### Обработка файлов
```
npm i -D @types/multer
```

main.ts
```
import { NestExpressApplication } from '@nestjs/platform-express';
import { join } from 'path';


async function bootstrap() {
  const app = await NestFactory.create<NestExpressApplication>(AppModule);   # add 
  app.useStaticAssets(join(__dirname, '..', 'public'));   # add this
  await app.listen(process.env.PORT ?? 8001);
}
bootstrap();
```