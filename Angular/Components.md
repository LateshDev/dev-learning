#  🧩 Angular Day 2 — Components

Day 2 of my Angular learning journey, focused on understanding components—the core building blocks used to create reusable, organized, and maintainable user interfaces.

# 📚 Table of Contents

1. What is a Component?
2. Why Components are Used
3. Component Structure
4. Component Class
5. @Component Decorator
6. Template
7. Interpolation
8. Selector
9. Creating a Component with Angular CLI
10. Key Takeaways
    
# 1. 📦 What is a Component?

A component is a fundamental building block of an Angular application.
It combines the UI and its related logic into a reusable unit.
A component generally contains:

Component
```
├── TypeScript → Logic
├── HTML       → UI
└── CSS        → Styling
```
# 2. ♻️ Why Components are Used

Components help divide a large application into smaller and manageable sections.
For example:
```
Website
├── Header
├── Navbar
├── Product List
├── Login Form
└── Footer
```
## Benefits

Code reusability
Better organization
Easier maintenance
Easier testing
Less duplicate code
Better scalability

# 3. 🏗️ Component Structure

An Angular component can be understood through three main parts:
```
Angular Component
│
├── Class
├── Decorator
└── Template
```
These parts work together to create a functional UI section.

# 4. 💻 Component Class

The class contains the component's data and logic.
```
export class HelloComponent {
  name: string = "Angular";

  greet() {
    console.log("Hello Angular");
  }
}
```
Here:
```
name → Property
string → Data type
greet() → Method
````

# 5. 🏷️ @Component Decorator

The @Component decorator tells Angular that a TypeScript class should be treated as a component.
import { Component } from '@angular/core';
```
@Component({
  selector: 'app-hello',
  template: <h1>Hello Angular</h1>
})
export class HelloComponent {}
```
It can define information such as:
Selector
Template
Styles
Component configuration

# 6. 🖼️ Template

The template defines the HTML displayed to the user.
Example:
HTML
```
<h1>Welcome to Angular</h1>
<p>This is my first component.</p>

```
A template can contain:

HTML
Angular syntax
Data binding
Event handling
Conditional and repeated content

# 7. 🔗 Interpolation

Interpolation is used to display values from the component class inside the template.

## Syntax:

{{ value }}

## Example:

name = "amit";

HTML
<h1>Hello {{ name }}</h1>

## Output:

Hello amit

# 8. 🏷️ Selector

A selector defines the custom name through which a component can be used.

## Example:

selector: 'app-hello'
It can then be placed in another template as:

HTML
<app-hello></app-hello>
The selector connects the component with its location in the UI.

# 9. ⚙️ Creating a Component with Angular CLI

Angular CLI can generate a component automatically.

## Command
ng generate component hello

## Short form:

ng g c hello

Angular may generate files such as:
```
hello/
├── hello.component.ts
├── hello.component.html
├── hello.component.scss
└── hello.component.spec.ts
```
This saves time and creates the required structure automatically.

# 10. 🧠 Key Takeaways

- Components are the basic building blocks of Angular applications.
- They make UI sections reusable.
- A component connects TypeScript logic with an HTML template.
- @Component provides component metadata.
- The class contains properties and methods.
- The template defines the visible UI.
- Selectors provide a way to use components.
- {{ }} is used for interpolation.
- ng g c can quickly generate a component.
- Components make applications easier to organize, maintain, test, and scale
