# Daily Journal

I've decided to start a journey to reinvigorate my technical skills. 

Day 1.

I think its time to learn React. Increasingly I find myself in need of things that could be really great simple web apps. Also, Esri does a lot of work with web, so this could be a great opportunity to explore more of the stack. 

## Getting Started

In a few of my past attempts to learn a new framework or technology, I've skipped over the doc and started with blogs. Having worked on doc and samples for almost two years, I think I owe it to the authors to give the traditional approach a try. I'm starting with the React tutorial tonight. 

### Thoughts, Discoveries, Notes

* I've used NPM before but had never heard of `npx`. It can execute as well as retrieve packages, so you can type `npx create-react-app app` to create a react app without installing - all one line. 
* `npm start` looks at scripts in package.json. Use of `start` appears to be by convention.
* React components get passed a `this.props` object with any properties passed as parameters in html.
    * Components return a hierarchy of views via `render`.
* Use `this.state` to store component state
    * Use setState to modify state (why not just a plain dictionary?)
* 'Controlled components' don't maintain state - fully owned by another component
* `Slice` creates a copy of an array for editing
  * **Immutability** seems to be pretty big. Better to copy->change->replace than directly edit. Makes it easier to implement undo/redo and easier to detect changes/when to re-render
* **Function components** only have a render method, no state; main benefit is easier to write
* Arrow functions appear to pass `this` to their contents
* React uses keys to know how to update lists (otherwise it has to re-render)