##### Для токена авторизации нужны эти библиотеки
```
npm install jsonwebtoken
npm install --save-dev @types/jsonwebtoken
```

##### Для хэширования пароля
```
npm i argon2
```


**Для google авторизации**
```
npm i google-auth-library
```

### Users.ts
```
import mongoose, { Document, HydratedDocument, Model } from "mongoose";
import argon2 from "argon2";
import type { UserFields } from "../types";
import config from "../config";
import jwt from "jsonwebtoken";


interface UsersMethods {
  checkPassword(password: string): Promise<boolean>;
  generateToken(): void;
}

interface UserVirtuals {
  confirmPassword: string;
}

type UsersModel = Model<UserFields, {}, UsersMethods>;

const UserSchema = new mongoose.Schema<
  HydratedDocument<UserFields>,
  UsersModel,
  UsersMethods,
  {},
  UserVirtuals
>(
  {
    username: {
      type: String,
      required: true,
      unique: true,
    },
    password: {
      type: String,
      required: true,
    },
    role: {
      type: String,
      required: true,
      default: "user",
      enum: ["user", "admin"],
    },
    token: {
      type: String,
    },

  
    displayName: {
      type: String,
      required: true,
    },
    googleID: String,
    avatar: String,
  },
  {
    virtuals: {
      confirmPassword: {
        get() {
          return this.__confirmPassword;
        },
        set(confirmPassword: string) {
          this.__confirmPassword = confirmPassword;
        },
      },
    },
  },
);

  

UserSchema.path("username").validate({
  validator: async function (this: Document, value: string) {
    if (!this.isModified("username")) return true;
    const user = await UserOrm.findOne({ username: value });
    return !user;
  },
  
  message: "This user is already registered",
});

  

UserSchema.path("password").validate(function (currentPassword: string) {
  if (!this.isModified("password")) return;
  
  return currentPassword === this.confirmPassword;
}, "Passwords do not match");


UserSchema.pre("save", async function () {
  if (!this.isModified("password")) return;
  
  try {
    this.password = await argon2.hash(this.password);
  } catch (e) {
    throw new Error("Error hash password");
  }
});

  

UserSchema.methods.generateToken = function () {
  this.token = jwt.sign({ _id: this._id }, config.refreshSecret, {
    expiresIn: "7d",
  });
};

  

UserSchema.set("toJSON", {
  transform: (doc, ret, options) => {
    const { password, token, ...rets } = ret;
    return rets;
  },
});

  

UserSchema.methods.checkPassword = function (password) {
  return argon2.verify(this.password, password);
};

  
const UserOrm = mongoose.model("users", UserSchema);
  

export default UserOrm;
```

#### Нужно сгенерировать секретный ключ для jwt на сайте [jwt](https://jwtsecrets.com/#generator), создать файлы .env и .env.template а также установить зависимость 
```
npm i dotenv


# index.ts
import dotenv

dotenv.config();
```

.env:
```
JWT_SECRET=c6a9ae17ebbbf324085aaca488b236e01e4f4d05e7c799a8b12110754386d66e
```
.env.template:
```
JWT_SECRET=
```
```
config.ts

const config = {
  rootPath,
  publicPath: path.join(rootPath, "public"),
  db: "mongodb://localhost/trecks",
  jwtSecret: process.env.JWT_SECRET || "default_fallback",
};
```
