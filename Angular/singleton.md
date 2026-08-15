# Angular  Day 3 — Services, Singletons & Pipes

Angular applications often need to reuse the same data and logic in different components. Services provide a clean way to handle shared work, while Singletons allow that shared data to remain consistent. Pipes help format values before displaying them.

---
# 1. What is an Angular Service?

A service is a reusable TypeScript class that contains common logic or data that multiple components may need.

For example, API requests, user information, application settings, or a company name used in the header and footer can be managed inside a service instead of being repeated in different components.

## Simple rule:

«Components mainly handle the UI, while services take care of reusable application logic.»

A service is commonly created with the "@Injectable" decorator.

Generate a Service

ng generate service logo

Example
```
// logo.service.ts

import { Injectable } from "@angular/core";

@Injectable({
  providedIn: "root"
})
export class LogoService {

  companyName = "Resume Loop";

  getCompanyName(): string {
    return this.companyName;
  }

  setCompanyName(name: string): void {
    this.companyName = name;
  }
}
```
"@Injectable()" makes the class available to Angular's Dependency Injection system.

"providedIn: "root"" makes the service available throughout the application.

---

# 2. Dependency Injection — Getting a Service

A component normally does not create a service manually. Instead, it requests the service through its constructor and Angular supplies the required instance.
```
// header.component.ts

import { Component } from "@angular/core";
import { LogoService } from "./logo.service";

@Component({
  selector: "app-header",
  template: `
    <h1>{{ logoService.getCompanyName() }}</h1>

    <button
      (click)="logoService.setCompanyName('Snapied')">
      Change
    </button>
  `
})
export class HeaderComponent {

  constructor(
    public logoService: LogoService
  ) {}

}
```
### This:
```
constructor(public logoService: LogoService) {}
```
means the component is asking Angular for a "LogoService".

You should not normally create it manually with:

new LogoService();

## Simple Analogy

Think about an electricity connection. You don't build a power generator every time you need electricity. You connect to the existing supply.

Dependency Injection works similarly:
```
Component
    ↓
Requests Service
    ↓
Angular DI
    ↓
Provides Service
```
---

# 3. Singleton — One Shared Service Instance

When a service uses:

providedIn: "root"

Angular normally provides a single shared instance for the entire application.

This is known as a singleton service.

Because components receive the same instance, changing a value through one component can affect what another component reads.

##Some Easy Examples

###1. Shared TV Remote

Imagine a family using one remote. If one person changes the channel, everyone sees the new channel.

### 2. Shared Washing Machine

A family uses one washing machine. One person starts a cycle and another person later finds the machine in its current state.

### 3. Shopping Cart

Imagine one shopping cart containing products collected from different stores. Everyone adds items to the same cart.

These examples represent the basic idea of a singleton: one shared instance instead of separate copies.

Without a shared instance, each component would have its own independent data and changes would not automatically be visible elsewhere.

---

# 4. Sharing Data Between Components

Two components do not have to be directly connected as parent and child to exchange information.

They can use a common service.

## Component A — Update the Value

this.logoService.setCompanyName("Snapied");

## Component B — Read the Value
```
<p>
  {{ logoService.getCompanyName() }}
</p>
```
Because both components use the same singleton service, Component B can access the value updated by Component A.

### Common Mistake

Avoid reading the value once and storing a separate copy when you need the latest service value.

## For example:

this.companyName =
  this.logoService.getCompanyName();

If the service value changes later, "companyName" may still contain the old value.

It is similar to taking a picture of a notice board once instead of checking the actual board when you need the latest information.

---

# 5. Why Learn Angular 13?

Even though Angular has newer versions, learning an older version can still be useful for understanding the fundamentals.

|Reason| Explanation|
|---|---|
|Core concepts| Components, services, DI, pipes, and modules remain important|
|Existing projects| Many applications are built on older Angular versions|
|Learning resources| Older versions have plenty of tutorials and examples|
|Easier foundation| Fewer newer concepts can make the basics easier to understand|

The important point is to first build a strong understanding of Angular fundamentals.

Once the basics are clear, moving to a newer Angular version becomes much easier.

---

# 6. Pipes — Formatting Template Values

A pipe changes the display format of a value without modifying the original data.

Pipes are applied using the "|" symbol.
```
<p>{{ name | uppercase }}</p>
<!-- RESUME LOOP -->

<p>{{ price | currency:'INR' }}</p>
<!-- ₹500.00 -->

<p>{{ today | date:'longDate' }}</p>
<!-- August 7, 2026 -->
```
Common Built-in Pipes

Angular provides several useful pipes:

uppercase
lowercase
titlecase
date
currency
number
percent
json
slice

## Combining Pipes

Multiple pipes can be applied in sequence:

{{ name | slice:0:5 | uppercase }}

The value is first shortened using "slice" and then converted to uppercase.

---

# 7. Creating a Custom Pipe

When the built-in pipes are not suitable for a particular requirement, you can create your own pipe.

A custom pipe uses:

- "@Pipe"
- "PipeTransform"
- "transform()"

## Example
```
// double.pipe.ts

import {
  Pipe,
  PipeTransform
} from "@angular/core";

@Pipe({
  name: "double"
})
export class DoublePipe
  implements PipeTransform {

  transform(value: number): number {
    return value * 2;
  }
}
```
Use it in the template:

<p>{{ 10 | double }}</p>

## Result:

20

The "transform()" method receives the value placed before the "|", performs the required operation, and returns the new value.

## Generate Using CLI

ng generate pipe double

For Angular 13 module-based applications, remember to include the custom pipe in the appropriate module's "declarations".

## Pipe with an Argument

A pipe can also accept additional parameters.
```
@Pipe({
  name: "multiply"
})
export class MultiplyPipe
  implements PipeTransform {

  transform(
    value: number,
    times: number
  ): number {

    return value * times;
  }
}
```
Use it like this:

<p>{{ 10 | multiply:3 }}</p>

## Result:

30

Here "10" is the input value and "3" is passed as an argument to the pipe.

---

# 8. Angular 13 Concepts — Quick Summary

|Concept| Main Purpose|
|---|---|
|Service| Store reusable logic and shared data|
|"@Injectable()"| Enables Dependency Injection|
|Dependency Injection| Lets Angular provide required services|
|Singleton| Keeps one shared service instance|
|Pipe| Formats values for template display|
|Custom Pipe| Creates application-specific transformations|

A useful flow to remember is:
```
Component
   ↓
Requests Service
   ↓
Angular DI
   ↓
Shared Service Instance
   ↓
Data Available to Components
```
---

# 9. Key Takeaways

- Service: A reusable class for common application logic and shared data.
- "@Injectable()": Makes a service work with Angular's Dependency Injection system.
- Dependency Injection: Components request services through their constructors.
- Singleton: A root-provided service can provide one shared instance throughout the application.
- Data Sharing: Unrelated components can communicate through a shared service.
- Angular 13: Provides the same important fundamentals needed to understand Angular development.
- Pipes: Format values in templates using the "|" operator.
- Built-in Pipes: Include "uppercase", "date", "currency", "number", "slice", and more.
- Custom Pipes: Use "@Pipe" and "transform()" to create your own transformation logic.
