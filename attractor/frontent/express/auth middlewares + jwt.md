```
npm i jsonwebtoken
npm i dotenv
```
**Для httponly coockeis**
```
npm install cookie-parser
npm install --save-dev @types/cookie-parser
```

#### 1. generete secret key
https://jwtsecrets.com/#generator

#### 2. create file .env

```
JWT_SECRET=secret_key
```

#### 3. create file .env.template
```
JWT_SECRET=    # empty
```

#### 4. add in index.ts
```
import dotenv from "dotenv";
...
dotenv.config();
```


#### 5. config.ts
```
const config = {
  rootPath,
  publicPath: path.join(rootPath, "public"),
  db: "mongodb://localhost/kr_11",
  jwtSecret: process.env.JWT_SECRET || "default_fallback"   # add this
};
```

#### 6. middleware/auth.ts
```
import { NextFunction, Request, Response, RequestHandler } from "express";
import { UserFields } from "../types";
import { HydratedDocument } from "mongoose";
import UsersOrm from "../models/User";
import jwt, { TokenExpiredError } from 'jsonwebtoken';
import config from "../config";

  
export interface RequestWithUser extends Request {  
  user: HydratedDocument<UserFields>;
}
  
export const authOrNot: RequestHandler = async (
  expressReq: Request,
  res: Response,
  next: NextFunction,
) => {
  const req = expressReq as RequestWithUser;
  const jwtToken = req.cookies.accessToken;

  if (!jwtToken) {
    return next();
  }
  
  try {
    const decoded = jwt.verify(jwtToken, config.jwtSecret) as { _id: string };
    const user = await UsersOrm.findOne({ _id: decoded._id });
  
    if (!user) {
      return res.status(401).send({ error: "Invalid or expired access token" });
    }

    req.user = user;
    next();
  } catch (e) {
    console.log(e);
    if (e instanceof TokenExpiredError) {
      return res.status(401).send({ error: "Your token expired" });
    } else {
      return res
        .status(401)
        .send({ error: "Please authenticate. Invalid access token" });
    }
  }
};

  

const auth: RequestHandler = async (
  expressReq: Request,
  res: Response,
  next: NextFunction,
) => {
  await authOrNot(expressReq, res, () => {
    const req = expressReq as RequestWithUser;
    if (!req.user) {
      return res.status(401).send({ error: "Token not sent" });
    }
    next();
  });
};

export default auth;
```

#### 7. use in route
```
import auth, { RequestWithUser } from "../middlewares/auth";

usersRouter.get("/me", auth, async (req, res) => {
  const user = (req as RequestWithUser).user;
  return res.send(user);
});
```


