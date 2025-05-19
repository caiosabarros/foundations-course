# foundations-course
NextJS Foundations Course for BYUI WDD430.

# Foundational Course NextJs Chapter 1-5
1. Run:
```shell
$ cd nextjs-dashboard # get into the app folder
$ npm run dev # run the app
```

# Class 1
DOM: it is like the AST for Solidity smart contracts. It is the Document Object Model.

DOM, in a practical scenario, is what we see in the developer tools code. That's not exactly the HTML that comes to be rendered in the page.
The HTML that is rendered in the page includes many script tags, and other kind of elements that get rendered in the DOM.
This is because the HTML represents the initial page content, whereas the DOM represents the updated page content which was changed by the JavaScript code you wrote.

Manipulating the DOM usually involves manipulating its elements, in content or style, using JS.

DOM methods: these are the methods in the document. For example, document.getElementById is a method from the DOM - the Object Model.

In other words, imperative programming is like giving a chef step-by-step instructions on how to make a pizza. 
Declarative programming is like ordering a pizza without being concerned about the steps it takes to make the pizza. 🍕
React is a declarative library.

Babel: Used to transform JSX code into JS code. Basically, the React code will live in the script tag of an HTML file.
However, React is written in JSX, which gets translated into JS by Babel, since the script tag of an HTML file 
understands only JS.

REACT:
- React components should be capitalized to distinguish them from plain HTML and JavaScript.
- Use React components the same way you'd use regular HTML tags (< Component />)
- React requires a top-level component. That's why we embrace components in divs in the return statements.
The top-level component and the child components form what they call the `component tree`.
- In React, data flows down the component tree. This is referred to as one-way data flow.
- React needs something to uniquely identify items in an array so it knows which elements to update in the DOM.
- Unlike props which are passed to components as the first function parameter, the state is initiated and stored within a component. You can pass the state information to children components as props, but the logic for updating the state _should be kept_ within the component where state was initially created. That is, the functions for updating state should be passed down.
- Props are read-only information that's passed to components. State is information that can change over time, usually triggered by user interaction.

NEXTJS:
- What does Next.js do when a <Link> component appears in the browser’s viewport in a production environment? Next.js automatically prefetches the code for the linked route in the background. By the time the user clicks the link, the code for the destination page will already be loaded in the background, and this is what makes the page transition near-instant!

API layer
APIs are an intermediary layer between your application code and database. There are a few cases where you might use an API:

If you're using third-party services that provide an API.
If you're fetching data from the client, you want to have an API layer that runs on the server to avoid exposing your database secrets to the client.
In Next.js, you can create API endpoints using Route Handlers.

You don't want to fetch data on the client as this would expose your database secrets (remember, you're not using an API layer). Instead, you can fetch the data on the server, and pass it to the component as a prop.

There are a few cases where you have to write database queries:

When creating your API endpoints, you need to write logic to interact with your database.
If you are using React Server Components (fetching data on the server), you can skip the API layer, and query your database directly without risking exposing your database secrets to the client.

Using Server Components to fetch data
By default, Next.js applications use React Server Components. Fetching data with Server Components is a relatively new approach and there are a few benefits of using them:

Server Components support JavaScript Promises, providing a solution for asynchronous tasks like data fetching natively. You can use async/await syntax without needing useEffect, useState or other data fetching libraries.
Server Components run on the server, so you can keep expensive data fetches and logic on the server, only sending the result to the client.
Since Server Components run on the server, you can query the database directly without an additional API layer. This saves you from writing and maintaining additional code.

@caiosapy
Concerning the API Layer, the way NextJs does it differently is that there is no need for me to create the logic to call on the database and make these functions available in the server through endpoints. Here, we can create the logic to query the database and import those functions directly in the client code. It avoids the API layer stuff.

A "waterfall" refers to a sequence of network requests that depend on the completion of previous requests. In the case of data fetching, each request can only begin once the previous request has returned data. This pattern is not necessarily bad. There may be cases where you want waterfalls because you want a condition to be satisfied before you make the next request. For example, you might want to fetch a user's ID and profile information first. Once you have the ID, you might then proceed to fetch their list of friends. In this case, each request is contingent on the data returned from the previous request. However, this behavior can also be unintentional and impact performance. A common way to avoid waterfalls is to initiate all data requests at the same time - in parallel.

However, there is one disadvantage of relying only on this JavaScript pattern: what happens if one data request is slower than all the others? Let's find out more in the next chapter.

RENDERING
With static rendering, data fetching and rendering happens on the server at build time (when you deploy) or when revalidating data. Static rendering is useful for UI with no data or data that is shared across users, such as a static blog post or a product page. It might not be a good fit for a dashboard that has personalized data which is regularly updated. When your data updates, you want to show the latest changes in your dashboard. Static Rendering is not a good fit for this use case.

Dynamic rendering allows you to access information that can only be known at request time, such as cookies or the URL search parameters. The dashboard application we're building is dynamic.

However, there is still one problem mentioned in the previous chapter. What happens if one data request is slower than all the others? **With dynamic rendering, your application is only as fast as your slowest data fetch.**

What's the solution to the above problem? Streaming

STREAMING
Streaming is a data transfer technique that allows you to break down a route into smaller "chunks" and progressively stream them from the server to the client as they become ready. Streaming works well with React's component model, as each component can be considered a chunk.

There are two ways you implement streaming in Next.js:

At the page level, with the loading.tsx file (which creates <Suspense> for you).
At the component level, with <Suspense> for more granular control.

loading.tsx is a special Next.js file built on top of React Suspense. It allows you to create fallback UI to show as a replacement while page content loads.
Since <SideNav> is static, it's shown immediately. The user can interact with <SideNav> while the dynamic content is loading.
The user doesn't have to wait for the page to finish loading before navigating away (this is called interruptable navigation). 

But we can do more to improve the user experience. Let's show a loading skeleton instead of the Loading… text.

A loading skeleton is a simplified version of the UI. Many websites use them as a placeholder (or fallback) to indicate to users that the content is loading. Any UI you add in loading.tsx will be embedded as part of the static file, and sent first. Then, the rest of the dynamic content will be streamed from the server to the client.

@caiosapy
My understanding is the NextJS is the most of what we could call a fron/back-end-mix. It doesn't separate one from the other. In one code you can call the DB and in the same code repository and frameework and you can display it nicely.

Route Groups
Route groups allow you to organize files into logical groups without affecting the URL path structure. When you create a new folder using parentheses (), the name won't be included in the URL path. So /dashboard/(overview)/page.tsx becomes /dashboard.

Here, you're using a route group to ensure loading.tsx only applies to your dashboard overview page. However, you can also use route groups to separate your application into sections (e.g. (marketing) routes and (shop) routes) or by teams for larger applications.

Great! You're almost there, now you need to wrap the <Card> components in Suspense. You can fetch data for each individual card, but this could lead to a popping effect as the cards load in, this can be visually jarring for the user.

So, how would you tackle this problem?

To create more of a staggered effect, you can group the cards using a wrapper component. This means the static <SideNav/> will be shown first, followed by the cards, etc.

**By moving data fetching down to the components that need it, you can create more granular Suspense boundaries. This allows you to stream specific components and prevent the UI from blocking.**

To recap, you've done a few things to optimize data fetching in your application so far:

Created a database in the same region as your application code to reduce latency between your server and database.
Fetched data on the server with React Server Components. This allows you to keep expensive data fetches and logic on the server, reduces the client-side JavaScript bundle, and prevents your database secrets from being exposed to the client.
Used SQL to only fetch the data you needed, reducing the amount of data transferred for each request and the amount of JavaScript needed to transform the data in-memory.
Parallelize data fetching with JavaScript - where it made sense to do so.
Implemented Streaming to prevent slow data requests from blocking your whole page, and to allow the user to start interacting with the UI without waiting for everything to load.
Move data fetching down to the components that need it, thus isolating which parts of your routes should be dynamic.

SEARCH
Your search functionality will span the client and the server. When a user searches for an invoice on the client, the URL params will be updated, data will be fetched on the server, and the table will re-render on the server with the new data.

useSearchParams- Allows you to access the parameters of the current URL. For example, the search params for this URL /dashboard/invoices?page=1&query=pending would look like this: {page: '1', query: 'pending'}.
usePathname - Lets you read the current URL's pathname. For example, for the route /dashboard/invoices, usePathname would return '/dashboard/invoices'.
useRouter - Enables navigation between routes within client components programmatically. There are multiple methods you can use.

When to use the useSearchParams() hook vs. the searchParams prop?

You might have noticed you used two different ways to extract search params. Whether you use one or the other depends on whether you're working on the client or the server.

<Search> is a Client Component, so you used the useSearchParams() hook to access the params from the client.
<Table> is a Server Component that fetches its own data, so you can pass the searchParams prop from the page to the component.
As a general rule, if you want to read the params from the client, use the useSearchParams() hook as this avoids having to go back to the server.

DEBOUCING
Debouncing is a programming practice that limits the rate at which a function can fire. In our case, you only want to query the database when the user has stopped typing.

How Debouncing Works:

Trigger Event: When an event that should be debounced (like a keystroke in the search box) occurs, a timer starts.
Wait: If a new event occurs before the timer expires, the timer is reset.
Execution: If the timer reaches the end of its countdown, the debounced function is executed.

Debouncing prevents a new database query on every keystroke, thus saving resources.

The `error.tsx` file serves as a catch-all for unexpected errors and allows you to display a fallback UI to your users.