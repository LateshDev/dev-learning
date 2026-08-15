# Angular Day 3 — Binding, Directives & Pipes

Angular provides several features to connect component data with the HTML template. In this day, we learn Data Binding, Directives, and Pipes.

# 1. Data Binding

Data Binding creates a connection between the TypeScript component and the HTML template. Angular provides four main types of binding.

## 1.1 Interpolation — "{{ }}"

Interpolation is used to display a component value inside HTML.

Direction: Component → View

<h1>Hello {{ name }}</h1>

If "name = "Angular"", the output will be:

Hello Angular

## 1.2 Property Binding — "[ ]"

Property Binding is used to set an HTML element property using a component value.

<img [src]="imageUrl">

<button [disabled]="isBusy">
  Save
</button>

Direction: Component → View

## 1.3 Event Binding — "( )"

Event Binding allows the template to call a component method when an event occurs.
```
<button (click)="save()">
  Save
</button>

save() {
  console.log("Saved!");
}
````
Direction: View → Component

## 1.4 Two-Way Binding — "[( )]"

Two-way binding keeps the component property and input field synchronized.

It is commonly known as "Banana in a Box."

<input [(ngModel)]="username">

<p>You entered: {{ username }}</p>

"ngModel" requires "FormsModule".

## Data Binding Summary
```
 Syntax|        Direction| Purpose
"{{ }}"| Component → View| Display data
  "[ ]"| Component → View| Set properties
  "( )"| View → Component| Handle events
  "[( )]"|      Both ways| Two-way binding
```
«Easy Trick: "[ ]" = Data In · "( )" = Event Out · "[( )]" = Both»

---

# 2. Directives

Directives are special Angular instructions that change the behavior, structure, or appearance of HTML elements.

The commonly used directives are:

- "*ngIf"
- "*ngFor"
- "ngSwitch"

## 2.1 "*ngIf" — Conditional Rendering

"*ngIf" displays an element only when the given condition is true.
```
<p *ngIf="isLoggedIn">
  Welcome back!
</p>

<p *ngIf="!isLoggedIn">
  Please login.
</p>
```
If the condition is false, Angular removes the element from the DOM.

## 2.2 "*ngFor" — Loop Through Data

"*ngFor" is used to repeat an HTML element for every item in an array.
```
<ul>
  <li *ngFor="let fruit of fruits">
    {{ fruit }}
  </li>
</ul>

fruits = ["Apple", "Mango", "Banana"];
```
The above array creates three "<li>" elements.

## 2.3 "ngSwitch" — Multiple Conditions

"ngSwitch" is useful when different content needs to be displayed according to a value.
```
<div [ngSwitch]="role">

  <p *ngSwitchCase="'admin'">
    You are an Admin
  </p>

  <p *ngSwitchCase="'user'">
    You are a User
  </p>

  <p *ngSwitchDefault>
    Unknown Role
  </p>

</div>
```
If "role" is ""admin"", only the admin message is displayed.

---

# 3. Pipes

Pipes are used to transform and format data for display without changing the original value.

Pipes use the "|" symbol.

Example
```
 <p>{{ name | uppercase }}</p>

<p>{{ price | currency:'INR' }}</p>

<p>{{ today | date:'longDate' }}</p>
```
Some commonly used built-in pipes are:

- "uppercase"
- "lowercase"
- "titlecase"
- "date"
- "currency"
- "number"
- "percent"
- "json"
- "slice"

### Pipe Chaining

Multiple pipes can be combined together.

{{ name | slice:0:5 | uppercase }}

Here, the value is first sliced and then converted into uppercase.

---

# 4. Custom Pipes

Angular allows developers to create their own pipes when the built-in pipes are not enough.

A custom pipe generally contains:

- "@Pipe" decorator
- "PipeTransform"
- "transform()" method

### Example
```
import { Pipe, PipeTransform } from "@angular/core";

@Pipe({
  name: "double"
})
export class DoublePipe implements PipeTransform {

  transform(value: number): number {
    return value * 2;
  }

}
```
Use the custom pipe in HTML:

<p>{{ 10 | double }}</p>

### Output:

20

The "transform()" method receives the value and returns the transformed result.

## Generate Pipe Using CLI

ng generate pipe double

### Short form:

ng g pipe double

---

# 5. Pipes with Arguments

A pipe can also receive additional arguments.
```
@Pipe({
  name: "multiply"
})
export class MultiplyPipe implements PipeTransform {

  transform(value: number, times: number): number {
    return value * times;
  }

}
```
Use it like this:

<p>{{ 10 | multiply:3 }}</p>

### Output:

30

Here:

- "10" → Input value
- "multiply" → Pipe name
- "3" → Argument

---

# 6. Key Takeaways

- Interpolation "{{ }}" → Displays component data.
- Property Binding "[ ]" → Sets HTML properties.
- Event Binding "( )" → Handles user events.
- Two-Way Binding "[( )]" → Synchronizes data in both directions.
- "*ngIf" → Displays content conditionally.
- "*ngFor" → Repeats elements for array items.
- "ngSwitch" → Selects content based on a value.
- Pipes "|" → Format and transform values for display.
- Custom Pipes → Create your own data transformations.

---

# Conclusion

In Angular, Binding connects data with the UI, Directives control how elements behave, and Pipes format data for presentation. These concepts are important for building dynamic and interactive Angular applications.
