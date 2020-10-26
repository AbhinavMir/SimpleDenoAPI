# SimpleDenoAPI
🐱‍🐉 Making a simple API in the new JavaScript Framework.

A few days ago, I wrote an article on an introduction to Deno, the new JavaScript/TypeScript runtime. [Here is the link](https://dev.to/abhinavmir/node-is-dead-welcome-deno-4l96) to it.
.Today we are going to develop a simple API in Deno.land, without a Database right now. That will be a discussion for later. This will be a simple GET and POST API.
Create a file named app.ts (Or anything that you want).
First, we need Oak, a middleware to help us create web servers. We import it via deno.land.

```typescript
import { Application, Router } from "https://deno.land/x/oak/mod.ts";
```

Now we declare the models. In Deno, we use Interfaces.

```typescript
interface Students
{
    name: String;
    roll: number;
}
```

Let's add data to your models and we are doing this from an array. Usually you would call your database here.

```typescript
let students: Array<Students> =
[
    {
        name: "Abhinav",
        roll: 1
    },
{
        name: "Puja",
        roll: 2
    }
]
```

Next we need to export router actions.

```typescript
export const getStudents = ({response}:{response: any}) =>
{
    response.body = students;
}
export const addStudents = async (
    {request, response}: {
        request: any;
        response: any;
    },
) => {
    const body = await request.body();
    const student: Students = body.value;
students.push(student);
    response.body = {studentAdded: "Success"};
    response.status = 200;
}
```

Let's instantiate router and application.

```typescript
const router = new Router();
const app = new Application();
const PORT = 4300;
// describe routers
router
    .get("/allstudents", getStudents)
    .post("/addStudent", addStudents);
app.use(router.routes());
app.use(router.allowedMethods());
```

Let's now make the script listen on the port.

```typescript
await app.listen({port: PORT});
```

And now we can run the script! Open command line in the directory and type this -

```shell
deno run --allow-net --allow-env app.ts
```

And there you go!
