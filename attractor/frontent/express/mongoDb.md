```
npm install --save mongoose
```

```
import mongoose from "mongoose";

const run = async () => {
  await mongoose.connect("mongodb://localhost/link_data");

  app.listen(port, () => {
    console.log(`Server started on ${port} port!`);
  });

  process.on("exit", () => {
    mongoose.disconnect();
  });
};

run().catch((err) => console.error(err));
```