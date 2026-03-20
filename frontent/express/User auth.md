```
npm install --save bcrypt
npm i -D @types/bcrypt
```

### Users.ts
```
import mongoose from "mongoose";


interface UsersFields {
    username: string;
    password: string;
    token: string
}

const UsersSchema = new mongoose.Schema<UsersFields>({

  username: {
    type: String,
    required: true,
    unique: true,
  },

  password: {
    type: String,
    required: true,
  },

  token: {
    type: String,
    required: true,
  },

});

const UsersOrm = mongoose.model("Users", UsersSchema);
export default UsersOrm;
```

```
import bcrypt from "bcrypt";

  
const SALT_WORK_FACTOR = 10;

...

UsersSchema.pre("save", async function () {   # 1
    if (!this.isModified("password")) return;

    const salt = await bcrypt.genSalt(SALT_WORK_FACTOR);
    const hash = await bcrypt.hash(this.password, salt);
    this.password = hash;
});

UsersSchema.set("toJSON", {    # 2
  transform: (doc, ret, options) => {
    const { password, ...rets } = ret;
    return rets;
  },
});
```

1. Перед сохранением пароль кэшируется
2. при сериализации в json будет выполянться удаление пароля из тела

### Далее добавляем метод проверки пароля при логине
```


UsersSchema.methods.checkPassword = function (password) {
  return bcrypt.compare(password, this.password);
};

```

типизируем этот метод
```
import mongoose, { Model } from "mongoose";

interface UsersMethods {
  checkPassword(password: string): Promise<boolean>;
}

type UsersModel = Model<UsersFields, {}, UsersMethods> 

const UsersSchema = new mongoose.Schema<UsersFields, UsersModel, UsersMethods>({
...

const UsersOrm = mongoose.model<UsersFields, UsersModel>("Users", UsersSchema); 
```