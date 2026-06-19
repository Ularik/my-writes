#### Создаем файл fixtures.ts
```
import crypto from "crypto";
import mongoose from "mongoose";
import config from "./config";
import Category from "./models/Category";
import ProductsOrm from "./models/Products";
import UsersOrm from "./models/Users";

  
  

const run = async () => {
  await mongoose.connect(config.db);
  const db = mongoose.connection;

  try {
    await db.dropCollection("categories");
    await db.dropCollection("products");
    await db.dropCollection("users");
  } catch (e) {
    console.log("Collections were not present, skipping drop...");
  }
    

  const [cpuCategory, ssdCategory] = await Category.create(

    {
      title: "CPUs",
      description: "Central Processor Units",
    },

    {
      title: "SSDs",
      description: "Solid State Drives",
    },
  );

  await ProductsOrm.create(

    {
      title: "Intel Core i7 12700K",
      price: 350,
      category: cpuCategory!._id,
      image: "fixtures/cpu.jpg",
    },
    {
      title: "Samsung 990 Pro 1TB",
      price: 170,
      category: ssdCategory!._id,
      image: "fixtures/ssd.jpg",
    },
  );

  await UsersOrm.create({
    name: "user",
    password: "1@345qWert",
    token: crypto.randomUUID(),
  });

  await db.close();
}

run().catch(console.error);
```

#### добавляем в package.json
```
  "fixtures": "ts-node fixtures.ts"
```
запуск 
```
npm run fixtures
```