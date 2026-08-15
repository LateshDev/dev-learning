# 🚀 Angular — Day 1: Getting Started

A beginner's introduction to Angular, TypeScript, ECMAScript, Angular CLI, installation, and the basic tools used to create modern web applications.

# 📚 Table of Contents

1. Introduction to Angular
2. Why Choose Angular?
3. Angular Performance
4. Mobile & Responsive Applications
5. Languages Used with Angular
6. Understanding ECMAScript
7. Understanding TypeScript
8. Setting Up Angular
9. Angular CLI
10. Useful CLI Commands
11. Key Takeaways

---

# 📖 1. Introduction to Angular

Angular is a frontend framework maintained by Google and used to develop modern web applications.

It is especially useful for applications that contain many screens, components, services, and user interactions.

## Angular can be used to create:

- Dynamic websites
- Single Page Applications (SPAs)
- Business applications
- Large-scale enterprise applications
- Interactive dashboards

## 🎯 Main Goals of Angular

Angular provides tools and features that help developers build applications that are:

- Organized
- Reusable
- Maintainable
- Scalable
- Easier to test

---

# ❓ 2. Why Choose Angular?

Angular provides a complete development environment instead of requiring developers to build everything from scratch.

## ⚡ Performance

Angular includes features that help applications run efficiently.

## Some important areas include:

- Efficient rendering
- Change detection
- Lazy loading
- Code splitting
- Production builds
- Component-based architecture

Angular's tooling can also optimize applications before they are deployed.

---

# 📱 3. Mobile & Responsive Applications

Angular applications can be designed to work across different screen sizes.

A single application can provide interfaces for:

- Desktop
- Laptop
- Tablet
- Mobile

Responsive behavior is generally achieved using web technologies such as:

HTML
CSS
JavaScript / TypeScript

Angular manages the application logic and structure while CSS handles much of the visual responsiveness.

---

# 💻 4. Language Choices

Angular applications are primarily developed using:

- JavaScript
- TypeScript
- Modern ECMAScript features

## ⭐ TypeScript

TypeScript is the language most commonly associated with Angular development because it provides static typing and additional development features.

---

# 📘 5. What is ECMAScript?

ECMAScript is the standardized specification on which JavaScript is based.

JavaScript implementations follow ECMAScript specifications.

For example:

ES6 → ES2015

ES2015 introduced several important JavaScript features, including:

- Classes
- Modules
- Arrow functions
- Template literals
- "let" and "const"

---

# 📗 6. What is TypeScript?

TypeScript is an open-source programming language developed by Microsoft.

It extends JavaScript by adding features such as:

- Static type checking
- Interfaces
- Type annotations
- Classes
- Generics
- Better tooling

## Example:

let age: number = 20;
let username: string = "Latesh";

The TypeScript compiler converts TypeScript into JavaScript that can be executed by the browser or another JavaScript runtime.

## 🔄 Basic Process
```
TypeScript Code
      ↓
TypeScript Compiler
      ↓
JavaScript
      ↓
Browser / Runtime
```
## ⭐ Benefits

- Detects many errors during development
- Improves code readability
- Makes large projects easier to manage
- Provides better editor support
- Makes refactoring easier

---

# ⚙️ 7. Installing Angular

Before creating an Angular project, you need the required development tools.

The basic setup can be divided into three stages.

## Step 1 — Install Node.js

Install a supported LTS version of Node.js.

After installation, check the versions:

node -v
npm -v

"npm" is normally installed along with Node.js.

«Always check the current Angular documentation for the Node.js versions supported by the Angular release you are using.»

---

## Step 2 — Install Angular CLI

Angular CLI can be installed globally with:

npm install -g @angular/cli

After installation, verify it:

ng version

If Angular CLI information appears, the CLI is available in your terminal.

---

## Step 3 — Create an Angular Application

Create a new project:

ng new my-first-app

Enter the project directory:

cd my-first-app

Start the development server:

ng serve

Angular will provide a local development address, commonly:

http://localhost:4200

Open that address in your browser to view the application.

---

# 🛠️ 8. What is Angular CLI?

CLI means Command Line Interface.

Angular CLI is the official command-line tool used to create and manage Angular projects.

It can help developers with:

- Creating projects
- Generating components
- Running the development server
- Building applications
- Running tests
- Managing Angular configuration

Without CLI tools, many project files and configurations would need to be created manually.

---

# 📌 9. Useful Angular CLI Commands

Create a Project

ng new project-name

Start Development Server

ng serve

Generate a Component

ng generate component component-name

## Short form:

ng g c component-name

## Build the Application

ng build

## Run Tests

ng test

## Display Angular Information

ng version

---

# ⭐ 10. Why is Angular CLI Important?

Angular CLI makes development more systematic.

It helps by:

- Reducing repetitive work
- Generating standard project files
- Creating components quickly
- Maintaining a consistent project structure
- Preparing applications for production
- Improving developer productivity

Simple Idea

## Without CLI
```
Manual Setup
     ↓
More Configuration
     ↓
More Work
```
## With CLI

```
Command
   ↓
Generated Structure
   ↓
Start Development

```
# 11. Key Takeaways

Angular is a framework for building modern frontend applications.

Google maintains the Angular framework.

TypeScript is the primary language used in Angular development.

ECMAScript defines the standard features of JavaScript.

Node.js provides the runtime environment required by Angular's development tools.

npm manages project dependencies and packages.

Angular CLI simplifies project creation and development.

Commands such as ng new, ng serve, ng generate, and ng build are fundamental Angular CLI commands.

Angular's component-based structure helps organize large applications.


