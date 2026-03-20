
```
npm i multer
npm i -D @types/multer
```

#### Создаем файл multer.ts
```
import multer from "multer";
import { promises as fs } from "fs";
import path from "path";
import { randomUUID } from "crypto";
import config from "./config";

const imageStorage = multer.diskStorage({
  destination: async (_req, _file, cb) => {
    const destDir = path.join(config.publicPath, "images");
    await fs.mkdir(destDir, { recursive: true });
    cb(null, destDir);
  },

  filename: (_req, file, cb) => {
    const extension = path.extname(file.originalname);
    cb(null, randomUUID() + extension);
  },
});

export const imagesUpload = multer({ storage: imageStorage });
```


### Создаем файл config.ts
```
import path from 'path';

  
const rootPath = __dirname;

  
const config = {
  rootPath,
  publicPath: path.join(rootPath, 'public'),
};

export default config;
```

```
import { imagesUpload } from "../multer";


messagesRouter.post("/", imagesUpload.single("image"), async (req, res) => {
  const message: MessageMutation = {
    author: req.body.author,
    text: req.body.text,
    image: req.file ? req.file.filename : null,
  };
  await filedb.addMessage(message);
  res.status(201).send("Success create new Product");
});
```


## Сохранение нескольких файлов изображений

```
productsRouter.post(
  "/",
  imagesUpload.single("image"),
  async (req: Request, res: Response, next) => {
  const files = req.files as Express.Multer.File[];

  const product: ProductWithoutId = {
    category: req.body.category_id, // 69b8c5fb1a08c2d6b96398e7
    title: req.body.title,
    description: req.body.description || null,
    price: Number(req.body.price),
    image: files ? files.map(file => "images/" + file.filename) : null,
  };
});
```

меняем типизацию для объекта ProductOrm:

```
const Schema = mongoose.Schema;

  

const ProductSchema = new Schema({
  title: {
    type: String,
    required: true,
  },
  ...
  image: [String],   # this

});
```

```
# types.d.ts

export interface ProductWithoutId {
    category: string;
    title: string;
    price: number;
    description: string;
    image: string[] | null;
}
```