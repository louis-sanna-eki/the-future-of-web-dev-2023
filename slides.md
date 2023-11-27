---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: images/green-ai.png
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Latest Web Development Trends.
# persist drawings in exports and build
drawings:
  persist: false
# page transition
transition: fade-out
# use UnoCSS
css: unocss
download: https://louis-sanna-eki.github.io/the-future-of-web-dev-2023/export/slides-export.pdf
favicon: images/eki-favicon.png
---

# Latest Web Development Innovations

Exploring the forefront of web development with technologies like Vercel, Next.js, Tailwind CSS, React Server Components, and Shadcn UI.

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Dive into Next.js, Tailwind CSS, and more <carbon:arrow-right class="inline"/>
  </span>
</div>

---
layout: image-right
image: images/web-dev-toc-image.png
---

# Table of Contents

1. **Vercel**: Frontend platform for performance and scaling.
2. **Next.js**: Server rendering and static site generation.
3. **Tailwind CSS**: Utility-first CSS framework.
5. **Shadcn UI**: Modern UI library for design and functionality.
4. **React Server Components**: Enhanced performance in React apps.
6. **Next.js Todo App Demo**: Practical application of these technologies.

---
layout: two-cols
---

# Vercel

- **Description**: A platform for frontend frameworks and static sites, optimized for performance and scaling.
- **Key Features**: 
  - Serverless functions
  - Enhanced performance with Edge Network
  - Seamless integration with various frameworks

- [Link to Vercel](https://vercel.com)
- [Link to Vercel Dashboard](https://vercel.com/dashboard)

::right::

<img src="/images/vercel-dashboard.png" class="mt-20 rounded shadow" />

---
layout: two-cols
---

# Next.js

- **Description**: A React framework providing server rendering and static website generation.
- **Key Features**:
  - Hybrid pages with Static Generation and Server-Side Rendering
  - Image optimization
  - API routes

::right::

<img src="/images/next-js.png" class="mt-40 ml-8 rounded shadow" />


---
layout: image-right
image: images/tailwind.png
---

# Tailwind CSS

- **Description**: A utility-first CSS framework for rapid UI development.
- **Key Features**:
  - Responsive design
  - Customizable through configuration
  - Wide range of utilities

---
---

# Tailwind example

```html
<div class="container mx-auto px-4">
  <div class="bg-blue-500 text-white text-center p-6 rounded-lg shadow-lg">
    <h1 class="text-3xl font-bold mb-2">Welcome to Tailwind CSS</h1>
    <p class="text-lg">Rapidly build modern websites without ever leaving your HTML.</p>
    <button class="mt-4 bg-blue-700 hover:bg-blue-800 text-white font-bold py-2 px-4 rounded">
      Get Started
    </button>
  </div>
</div>
```

<div class="container mx-auto px-4">
  <div class="bg-blue-500 text-white text-center p-6 rounded-lg shadow-lg">
    <h1 class="text-3xl font-bold mb-2">Welcome to Tailwind CSS</h1>
    <p class="text-lg">Rapidly build modern websites without ever leaving your HTML.</p>
    <button class="mt-4 bg-blue-700 hover:bg-blue-800 text-white font-bold py-2 px-4 rounded">
      Get Started
    </button>
  </div>
</div>

---
layout: two-cols
---

# Shadcn UI

- **Description**: A modern UI library with a focus on design and functionality.
- **Key Features**:
  - Comprehensive UI components
  - Customizable themes
  - Built for performance and usability

[Link to library](https://ui.shadcn.com/)

::right::

<img src="/images/shadcn.png" class="mt-20 rounded shadow" />

---
layout: image-right
image: images/server-component.png
---

# React Server Components

- **Description**: Enhance React apps with server-rendered components for better performance.
- **Key Features**:
  - Reduced bundle size
  - Server-side rendering capabilities
  - Seamless integration with React applications

- https://www.joshwcomeau.com/react/server-components/
- https://nextjs.org/docs/app/building-your-application/rendering/composition-patterns

---
---

<div class="slide two-cols-header">
  <h1>Server vs Client Components in React</h1>
  <div>
    <h2>Server Component</h2>
    <ul>
      <li>Fetch data</li>
      <li>Access backend resources (directly)</li>
      <li>Keep sensitive information on the server (access tokens, API keys, etc)</li>
      <li>Keep large dependencies on the server / Reduce client-side JavaScript</li>
    </ul>
  </div>
  <br/>
  <div>
    <h2>Client Component</h2>
    <ul>
      <li>Add interactivity and event listeners (onClick(), onChange(), etc)</li>
      <li>Use State and Lifecycle Effects (useState(), useReducer(), useEffect(), etc)</li>
      <li>Use browser-only APIs</li>
      <li>Use custom hooks that depend on state, effects, or browser-only APIs</li>
      <li>Use React Class components</li>
    </ul>
  </div>
</div>


---
layout: two-cols
---

# Composition patterns

```javascript
import React from 'react';

// Server Component
function App() {
  return <><Header /><Article /></>;
}

// Server Component
function Header() {
  return <header>Header Content</header>;
}

// Client Component
function Article() {
  "use client" //directive
  return <div><HitCounter /><Discussion /></div>;
}

// Client Component
function HitCounter() {
  return <div>Hit Counter</div>;
}

// rest of the app...
```

::right::

<img src="/images/client-server-boundary.png" class="mt-20 rounded shadow" />

---
layout: two-cols
---

# Next.js Todo App Demo

- **Overview**: A practical demonstration using the latest web technologies.
- **Features**:
  - Serverless backend with Vercel
  - Frontend using Next.js and Tailwind CSS
  - React Server Components for dynamic rendering

- [Link to demo](https://nextjs-todo-app-theta-flame.vercel.app/)
- [Link to code](https://github.com/louis-sanna-eki/nextjs-todo-app)

::right::

<img src="/images/nextjs-todo-app-screenshot.png" class="mt-20 rounded shadow" />

---
layout: center
---

# Understanding Server Actions in React

Server actions in React are operations that are performed on the server rather than in the user's browser. This approach can significantly enhance the performance and security of an application.

### How Does It Work?

Server actions are typically written in a Node.js environment and can be triggered by client components through API calls or form submissions. They can perform tasks such as:

- Fetching data from a database
- Writing data to a database
- Performing computations
- Accessing third-party APIs with secret credentials

By intelligently dividing responsibilities between client and server, React applications can become more scalable and maintainable.

---
---

# Server action: example code

```javascript
function CardWithForm() {

  async function add(formData: FormData) {
    "use server"
    const description = formData.get("description")?.toString() ?? "";
    addTodo({ description });
    revalidatePath('/')
  }

  return (
    <Card className="w-[700px]">
      <CardHeader>
        <CardTitle>Todo App</CardTitle>
        <CardDescription>A demonstrator using the lastest web technologies.</CardDescription>
      </CardHeader>
      <CardContent>
        <TableDemo />
      </CardContent>
      <form action={add}><CardFooter className="flex gap-4">
        <Input type="text" name="description" />
        <Button type="submit" className="ml-auto">Add</Button>
      </CardFooter>
      </form>
    </Card>
  )
}
```