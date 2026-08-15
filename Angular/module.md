# Angular  Day 2 — Modules & Communication Between Components

---

# 1. Understanding Angular Modules ("NgModule")

An Angular module is a traditional mechanism for organizing components, directives, pipes, and their dependencies into a single unit.

You can think of a module as a container that defines which features belong together and which features can be shared with other parts of the application.

For example, an e-commerce application could have:

- "UserModule"
- "ProductModule"
- "OrderModule"
- "CartModule"

## Main Parts of an NgModule

Property| Purpose
|---|---|
|"declarations"| Lists components, directives, and pipes owned by the module|
|"imports"| Adds other modules whose features are required|
|"exports"| Makes selected features available to other modules|
|"providers"| Registers services for the module|

---

# 2. Why Angular Modules Are Used

Modules were introduced to keep Angular applications structured and easier to manage.

## Their main benefits include:

- Better organization – related functionality stays together.
- Code reuse – common features can be shared between different areas.
- Separation of responsibilities – each module can focus on a particular feature.
- Lazy loading – large features can be loaded only when required.
- Dependency management – modules define the features they depend on.

This becomes especially useful when working with older or large Angular applications.

---

# 3. Basic "NgModule" Example

An Angular module is created using the "@NgModule" decorator.
```
// app.module.ts

import { NgModule } from "@angular/core";
import { BrowserModule } from "@angular/platform-browser";
import { AppComponent } from "./app.component";

@NgModule({
  declarations: [
    AppComponent
  ],

  imports: [
    BrowserModule
  ],

  bootstrap: [
    AppComponent
  ]
})
export class AppModule {}
```
Here:

- "declarations" → tells Angular which components belong to the module.
- "imports" → provides additional Angular features.
- "bootstrap" → identifies the starting component.

In traditional Angular applications, "AppModule" acts as the root module.

---

# 4. Modules vs Standalone Components

Modern Angular does not require every component to belong to an "NgModule".

Standalone components allow a component to declare its own dependencies directly.

Since newer Angular versions, standalone components are the default approach for newly created applications.

Feature| NgModule| Standalone|
|---|---|
|Required for every component| No| No|
|Modern default| No| Yes|
|Common in older applications| Yes| Less common|
|Can organize dependencies| Yes| Yes|
|Useful for existing projects| Yes| Yes|

## Important Point

Learning "NgModule" is still valuable because many existing Angular applications use modules. For new Angular development, standalone components are generally preferred.

---

# 5. Component Communication

Components often need to exchange information.

When one component is placed inside another component's template:

- The outer component is the Parent.
- The nested component is the Child.

Angular mainly provides two decorators for parent-child communication:

Communication| Angular Feature
Parent → Child| "@Input()"
Child → Parent| "@Output()"

---

# 6. Passing Data from Parent to Child — "@Input()"

The child component uses "@Input()" when it needs to receive a value from its parent.

Child Component
```
import { Component, Input } from "@angular/core";

@Component({
  selector: "app-child",
  template: <h3>Welcome {{ name }}</h3>,
})
export class ChildComponent {

  @Input() name: string = "";

}
```
## Parent Component

The parent sends the value using property binding:
```
template: `
  <app-child [name]="userName"></app-child>
`

userName: string = "Priya";
```
The child receives the value and displays:

Welcome Priya

The "[name]" syntax means the parent is passing a value into the child's "@Input()" property.

---

# 7. Sending Information Back — "@Output()"

For communication in the opposite direction, the child can create and emit an event using "@Output()" and "EventEmitter".

Child Component
```
import {
  Component,
  Output,
  EventEmitter
} from "@angular/core";

@Component({
  selector: "app-child",
  template: `
    <button (click)="sendMessage()">
      Send Message
    </button>
  `,
})
export class ChildComponent {

  @Output() messageEvent =
    new EventEmitter<string>();

  sendMessage() {
    this.messageEvent.emit(
      "Message from child!"
    );
  }
}
```
# Parent Component

The parent listens for the event:
```
template: `
  <app-child
    (messageEvent)="receiveMessage($event)">
  </app-child>
`

receiveMessage(message: string) {
  this.message = message;
}
```
"$event" contains the value sent by the child through "emit()".

---

# 8. Binding Symbols — Quick Reference

|Syntax| Flow| What It Does|
|---|---|
|"[property]="value""| Parent → Child| Sends data to an "@Input()"|
|"(event)="method()""| Child → Parent| Receives an "@Output()" event|

Easy Way to Remember

"[ ]" → Data goes in

"( )" → Event comes out

So:
```
Parent
   │
   │  @Input
   ↓
Child
   │
   │  @Output
   ↓
Parent
```
---

# 9. Key Points to Remember

- "NgModule" groups related Angular features and manages their dependencies.
- Modules are still supported and are commonly found in older Angular applications.
- Standalone components are the modern default approach for new Angular applications.
- "@Input()" sends information from Parent → Child.
- "@Output()" sends events from Child → Parent.
- "EventEmitter" is commonly used with "@Output()" to emit values.
- "[ ]" represents property/data binding.
- "( )" represents event binding.
- "$event" contains the data emitted by a child component.
