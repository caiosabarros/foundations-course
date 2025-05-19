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
