# bootcamp-shopping-cart

![](images/store.png)
![](images/cart.png)

## Preqrequities

- NodeJS v20+
- Visual Studio Code
- Git

To get started, fork this repo
```
https://github.com/txwilliams-godaddy/bootcamp-shopping-cart
```

Then, and then clone your copy locally:
```bash
git clone https://github.com/txwilliams-godaddy/bootcamp-shopping-cart.git
cd bootcamp-shopping-cart
code . #this opens up the visual studio code with the project as your working directory
```

Then, let's make sure we have the BootCampCart-API you worked on [previously](https://github.com/thoag-godaddy/BootCampCart-API).

Install with:

```
npm install
```

Start the app with:

```
npm run local
```

Visit the index at:

http://localhost:8080/

## Getting Started

So how do you build a web application? At the very simplest level, a webapp responds to a request from a client with some data. If you want the content to be viewable in a web browser, it will probably be some combination of HTML/CSS/Javascript files. One way to do this is to simply write HTML, put it on a server, and configure that server to return that file - simple! As you begin to build bigger websites with more complex functionality, though, you will likely find that the simple approach is limiting - that's where frameworks like Gasket and NextJS come in!

### What is a framework?
Web frameworks are collections of code that automate and provide common solutions to many problems that you will need to solve to build a web application. Their features typically include but are certainly not limited to:
- Templating and code reuse: Sharing layouts and inserting dynamic content into pages
- Authentication and session management: Creating accounts and logging in securely.
- Routing: Mapping URLs to code handlers
- File bundling and optimization
- Configuration management

> Aside: What is the difference between a library and a framework? Both are collections of code written bundled separately and included in your application. Put simply: Your code invokes library code, and framework code invokes your code!

### Next.js and Gasket
[Next.js](https://nextjs.org/) is a Javascript web application framework designed to help you create performant React applications. It has a ton of powerful features, but don't be overwhelmed (what the heck is `Client-side routing with optimized prefetching`??) - you don't have to know much to get up and running!

[Gasket](https://gasket.dev/#/) takes frameworks to another level: it's a framework for frameworks!

Created and open-sourced at GoDaddy, Gasket lets us create standard templates for and bundle features for different types of applications. Want to build an API? There's a Gasket preset for that, with support for GoDaddy authentication and API standards already built in. Building a full-stack app? There's a preset for that, with all of our standard UX goodness built in.

To use Gasket, we would normally to install it into our project via NPM (Node Package Manager) via [these instructions](https://github.com/godaddy/gasket/blob/master/docs/quick-start.md). However, this repo already comes with gasket installed so no need to worry!

And speaking of libraries, let's install one called [Material UI](https://v5.mui.com/material-ui/). This is a Google-inspired opensource library that contains some basic react components we'll use to make our web app pretty. We'll go more in-depth into Material UI a little later on. For now though, let's just run this command:
```
npm install @mui/material
```

### Framework skeleton
Alright, enough talk, let's look at some code. After creating a new Gasket app with the Next.js preset, we get a few key things:
- Some config files that we'll ignore for now (`gasket.config.js`, `package.json`, `jest.config.js`)
- The components directory, with one component: [`head.js`](./components/head.js)
- The pages directory, with 3 files:
  - [`_app.js`](./pages/_app.js)
  - [`_document.js`](./pages/_document.js)
  - [`index.js`](./pages/index.js)

We'll talk more about pages and components in a moment, but I want to make a couple of brief notes on these:
- `head.js` is a simple React component that encapsulates what we'd put in the `<head>` element on every page. This is a great example of how web frameworks are a big boost over writing out pages by hand - we can just re-use that component everywhere and inject the variable values that we need!
- In Nextjs, files in the `pages/` directory map to route names, with `index.js` being a special case that routes to `/`. Visit `http://localhost:8080` to view the page, and edit the text and save the file to see it reload in real time!
- If you want global css styles, import them into [`_app.js`](./pages/_app.js).

One last thing to remember - it's ok to not understand everything that's going on in these generated files! One of the most important skills to learn as a web developer is the ability to accept some "magic" and focus your attention where it really matters. There will always be time to learn about features in-depth later when you need to use them.

## Pages
You may remember having to map URL routes to code handlers when writing your APIs in the Falcon framework with code like this:
```python
api.add_route('/v1/products', Products())
api.add_route('/v1/products/{product_id:int}', Product())
```

For UI pages in Next.js, the framework is even more opinionated: each file in the `pages/` that exports a React component automatically gets its own route. We discussed the special case of `pages/index.js` already, which is mapped to the root route `/`. New files will get pages that correspond to their name, and you can create folder structures to nest pages. For example:
- `pages/about.js` will map to `/about`
- `/pages/blog/posts.js` will map to `/blog/posts`
- `/pages/blog/posts/[id].js` will create a dynamic route for `/blog/posts/{id}`. We won't be discussing dynamic Next.js routing today, but you can check out the docs for more information: https://nextjs.org/docs/app/api-reference/file-conventions/dynamic-routes

Remember the products API you created? Let's create a page to browse products at the `/shop` route. We don't have any components to include yet, so for now we'll just add a title (`"My shop"`) and a link to another page, the cart page. The Next.js [Link component](https://nextjs.org/docs/13/app/api-reference/components/link) works a lot like an `<a>` tag, but with some framework magic sprinkled in.

Here's the full page code that you should add to `pages/shop.js`:

```javascript
import React from 'react';
import Head from '../components/head';
import Link from 'next/link';

import { Container, Typography } from '@mui/material'

export const ShopPage = () => (
  <Container>
    <Head title='Home'/>
    <div>
      <Typography variant="h3">My Shop</Typography>
    </div>
    <div>
      <Link href="/cart">View cart</Link>
    </div>
  </Container>
);

export default ShopPage;
```

Check it out at `http://localhost:8080/shop`. You'll notice that clicking on the "View cart" link leads to an error page - let's fix that! Create a `cart.js` page similar to this one with a different title and a link back to the shop page.

## Material UI

Ok, now that we have the basic pages setup lets circle back to back to Material UI. You might have noticed a few lines of code in ShopPage that [import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) the "Container" and "Typography" components from Material UI

```js
// This is called a named import
import { Container, Typography } from '@mui/material'
```

Both `Container` and `Typography` are examples of reusable react components that the gracious and mysterious and opensource community have built. In react, reusability is core tenet and the main way we will encapsulate reusable frontend logic and HTML is via components. In fact, when building a large scale web-app, one of the first things you will need to identify is what components you will need to build, and which components you can reuse.

Here's a couple candidates for re-use (via Material UI), but feel free to explore the docs to see the full list of generic components are available

Card
- https://v5.mui.com/components/cards/
- https://v5.mui.com/api/card/

Typography
- https://v5.mui.com/components/typography/
- https://v5.mui.com/api/typography/

Button
- https://v5.mui.com/components/buttons/
- https://v5.mui.com/api/buttons/

## Components

However, no web-app is able to rely purely on generic components. We'll also need to build some custom components specific to our use case. Since we're modeling a shopping cart, you could imagine the we need the following components:

1. ShopItem
2. ShopItemList
3. CartItem
4. Cart

Let's walk through an example and create a skeleton for `ShopItem.js` in the `/components` folder.

```js
import React from 'react';
import { Card, CardContent, CardActions } from '@mui/material'

function ShopItem({ /* insert props here */}) {

  const addToCart = () => {
     /* Insert logic here  */
  }

  return (
    <Card style={{height: "400px"}}>
      /* Insert Image Here */
    <CardContent>
      /* Insert Name, Description, Price Here */
    </CardContent>
      <CardActions>
        /* Insert AddToCart Button here */
        /* What should happen when you click the button? */
      </CardActions>
    </Card>
  );
}

export default ShopItem;
```

Now that we've got a ShopItem, we need to retrieve the list of products from your api and render the full list using your newly built component. To do this, let's create a `ShopItemList.js` in the `/components` folder. This component will reference `ShopItem`.


```js
import React, { useState, useEffect } from 'react';
import { Grid } from '@mui/material'
import { useRouter } from 'next/router'

function ShoppingItemList() {

  // this is the state we will use to hold the response from the api
  const [products, setProducts] = useState([]);
  const router = useRouter()

  useEffect(async () => {
    /* fetch list of products here */
    /* update product state with response */
  }, [])

  const handleAddToCart = async (product) => {
    /* add product to cart via api */
    /* redirect to the cart page */
  }

  return (
    <Grid container direction="row" spacing={1}>
       /* Add your ShoppingItem components here! */
       /* How should you iterate over the list of products?  */
       /* Hint: map() function for Arrays */
    </Grid>
  )
}
```


Now that we got you started, the rest is up to you! Your next steps should be finishing up the CartItem and Cart components. Reach out to a bootcamp trainer or take a peek at a working solution [here](https://github.com/txwilliams-godaddy/bootcamp-shopping-cart/tree/solution).

## Tips
### Async/await
Managing asynchronous code can be tricky to wrap your head around, but once you master it you will unlock tons of the cool stuff that client-side javascript can do.

First: what does async/asynchronous mean? Well, it's actually a negation of "synchronous" ("a-synchronous" == "not synchronous") so let's start there: Synchronous means that your code instructions are fully executed in the order that you write them. This is probably how you've written most programs! Something like:

```js
function square(n) {
  return n * n;
}

const x = 5;
const result = square(x);
console.log(result)
// 25
```

Each line will be executed in full and in order. First `x` is assigned to 5, then the function `square` is called with `x` as a parameter, `square` finishes and the result is stored in the `result` variable, and finally we print `result` to the console.

Asynchronous code (or async for short) lets you start running some code, continue doing other useful work, and come back and do something when that bit of code you started completes. Why would you want to do this? The most common case is that you want to do something that involves a lot of waiting around, like making an HTTP request to some API. These requests can take seconds, an eternity of waiting for a computer. Async code style lets you just define what you want to do when the operation completes, and the execution envioronment handles all the details of invoking the your handling code when the background portion is done.

In javascript, how you write async code has evolved over the years so you may see some different styles. One way to do it is to make use of the `async` and `await` keywords. When defining a function, you can specify that it is an async function by adding `async` to the declaration:

```js
async function async_square(n) {
  // I need to think for a while...
  // got it!
  return n * n;
}
```

This tells the javascript interpreter that the function is async and restricts how you can use it. Most importantly, it changes what the function returns! If you try to call `square` directly, it no longer returns a number:

```js
let result = async_square(5);
console.log(result)
// Promise { <state>: "fulfilled", <value>: 25 }
```

Instead of `25` we got a `Promise` object with some properties. We won't go too deeply into the details of what a `Promise` is here, but here are the key things to know:
- Promises are wrappers around some result value, which could be anything - a number, a string, an object.
- Promises have internal state managed by the javascript engine, and you can hook functions into these lifecycle changes - for example, when it is fulfilled.

The easiest way to obtain the result of an `Promise` (and by extention, `async` functions), is to use the `await` keyword:

```js
let result = await async_square(5);
console.log(result)
// 5
```

This is some syntactic sugar that automatically handles the promise resolution and assigns the value to your `result` variable. How would you do this manually? We have to get a bit deeper into the weeds of Promises for that.

### Promises
We saw that `async` functions return `Promise` objects. A `Promise` is, well, a promise that something will be computed asynchronously and eventually returned back to the caller through a callback method. To specify these callback methods, we use the `Promise.then` function ([docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then#syntax)). Let's re-write our async square function above with raw promises instead of async/await:

```js
function async_square(n) {
  return new Promise((resolve, reject) => {
    resolve(n * n);
  });
}

let result = async_square(5);
console.log(result);
// Promise { <state>: "fulfilled", <value>: 25 }
result.then(value => {
  console.log(value);
  // 5
})
```

Cool! You may notice that when creating the promise, we actually passed in a function definition that took 2 arguements: `resolve` and `reject`. These are how we invoke different callback methods for either success or failure. It's common that something can go wrong with an async operation like an HTTP request, so we need to support error handling with the `reject` method. If all goes well, we call `resolve` with the outcome. If not, we call `reject` with details on what went wrong. To handle rejections, we can pass a second callback method into the `.then` function. Here's a full example, with explicitly defined handler functions instead of arrow functions:

```js
function async_square(n) {
  return new Promise((resolve, reject) => {
    if (n <= 12) {
      resolve(n*n);
    } else {
      reject(`I'm still working on my times tables, ${n} is too large! Try a smaller number`);
    }
  });
}

function handle_fulfilled(result) {
  console.log(`Fulfilled with result: ${result}`);
}

function handle_rejected(reason) {
  console.log(`Oh no! Promise rejected for reason: ${reason})`);
}

async_square(5).then(handle_fulfilled, handle_rejected);
// Fulfilled with result: 25

async_square(15).then(handle_fulfilled, handle_rejected);
// Oh no! Promise rejected for reason: I'm still working on my times tables, 15 is too large! Try a smaller number
```

### Fetch
The most common async code you'll write is when making HTTP requests. The way you do that is through the fetch API ([docs](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)). At it's simplest, you can make a GET request and parse json like so:

```js
fetch('http://example.com/movies.json')
  .then(response => response.json())
  .then(data => console.log(data));
```

Ooh, two `.then` calls! Did I mention that you can chain promises? Let's walk through what's happening here:

1. `fetch(url)` takes in a URL and returns a promise that resolves with a [response](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch#response_objects) object on success.
2. We add a handler that takes that response object and calls the [`.json()`](https://developer.mozilla.org/en-US/docs/Web/API/Request/json) method which _also_ returns a promise. This parses the body JSON and resolves the promise with an object representing the JSON.
3. We add another handler to that promise, which takes the `data` result and logs it to the console.

Cool! Notoriously missing here is error handling - I won't get into that, but you can check out the [docs](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch#checking_that_the_fetch_was_successful) for some examples.

To POST data to an API, we need to make use of the 2nd parameter to the `fetch` function, the request options:

```js

const postData = { answer: 42 };
const options = {
  method: 'POST',
  headers: {
      'Content-Type': 'application/json'
  },
  body: JSON.stringify(data)
}
const result = await fetch('https://example.com/answer', options).then(response => response.json());
```

Now go forth and fetch!

## Links

Gasket: https://github.com/godaddy/gasket
- Gasket Plugins:  https://gasket.dev/#/README

Training URLS:
- Pluralsight
  - React Track: https://www.pluralsight.com/browse/software-development/react
  - React Fundamentals: https://www.pluralsight.com/courses/react-fundamentals-update
  - React Component App: https://www.pluralsight.com/projects/build-a-quiz-component-with-react
