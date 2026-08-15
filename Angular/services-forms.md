# Angular · Day 3 (3 of 3) — Services, Data Sharing & Forms

Angular applications often need a place for reusable logic, communication between components, and handling user input. This section covers Services, Dependency Injection, Shared Data, and Forms.

---

# 1. What is an Service?

A service is a reusable TypeScript class that contains logic or data needed by multiple parts of an application.

Instead of writing the same API call, calculation, or shared state inside several components, we can place it inside a service.

## Simple idea:

«Components mainly manage the UI, while services handle reusable application logic.»

Common examples include:

- API communication
- Authentication
- User information
- Shared application data
- Reusable calculations

---

# 2. Creating a Service

A service can be generated using Angular CLI:

ng generate service data

## Example:
```
// data.service.ts

import { Injectable } from "@angular/core";

@Injectable({
  providedIn: "root"
})
export class DataService {

  private message: string = "Hello from DataService";

  getMessage(): string {
    return this.message;
  }

  setMessage(value: string): void {
    this.message = value;
  }
}
```
The "@Injectable()" decorator tells Angular that the class can participate in Dependency Injection.

"providedIn: "root"" makes the service available throughout the application.

---

# 3. Dependency Injection — Getting a Service

Angular components normally do not create services manually.

Instead, the component asks Angular for the required service through its constructor.
```
import { Component, OnInit } from "@angular/core";
import { DataService } from "./data.service";

@Component({
  selector: "app-hello",
  template: <p>{{ text }}</p>
})
export class HelloComponent implements OnInit {

  text: string = "";

  constructor(
    private dataService: DataService
  ) {}

  ngOnInit(): void {
    this.text = this.dataService.getMessage();
  }
}
```
This:

constructor(private dataService: DataService) {}

means:

«"This component needs "DataService"."»

Angular creates or provides the required service instance automatically.

You generally should not use:

new DataService();

because Angular's Dependency Injection system should manage the service.

---

# 4. Services for Sharing Data

A root-provided service normally has one shared instance throughout the application.

This is commonly called a singleton service.

For example, one component can update a value:
```
// Component A
this.dataService.setMessage("Message updated by Component A");
```
Another component can retrieve the same value:
```
// Component B
this.text = this.dataService.getMessage();
```
Because both components use the same service instance, the second component can access the updated data.

Which method should you use?

|Situation| Recommended Approach|
|---|---|
|Parent → Child| "@Input()"|
|Child → Parent| "@Output()"|
|Unrelated components| Shared Service|

A service is especially useful when components are far apart in the application structure.

---

# 5. Angular Forms — Two Main Approaches

Forms are used in almost every application for tasks such as login, registration, search, and profile updates.

Angular provides two major form approaches:

|Feature| Template-Driven| Reactive|
|---|---|---|
|Main logic| HTML| TypeScript|
|Required module| "FormsModule"| "ReactiveFormsModule"|
|Complexity| Simple| More advanced|
|Validation| Easier for basic forms| Powerful and flexible|
|Suitable for| Small forms| Large/complex forms|

Both approaches are supported. Template-driven forms are generally easier to start with, while reactive forms provide more control over form structure and validation.

---

# 6. Template-Driven Forms

Template-driven forms are mainly created inside the HTML template.

They commonly use "[(ngModel)]" for two-way data binding and require "FormsModule".

### HTML
```
<form (ngSubmit)="submitForm()">

  <input
    name="username"
    [(ngModel)]="username"
    placeholder="Username"
  />

  <input
    name="email"
    [(ngModel)]="email"
    placeholder="Email"
  />

  <button type="submit">
    Submit
  </button>

</form>
```
### TypeScript
```
username: string = "";
email: string = "";

submitForm(): void {
  console.log(this.username);
  console.log(this.email);
}
```
Here, the input values are automatically connected to the component properties.

When the form is submitted, "submitForm()" receives the latest values.

This approach is convenient for small and straightforward forms.

---

# 7. Reactive Forms

Reactive forms define the form structure inside the TypeScript class.

They use classes such as:

- "FormGroup"
- "FormControl"
- "Validators"

The application needs "ReactiveFormsModule".

### TypeScript
```
import {
  FormGroup,
  FormControl,
  Validators
} from "@angular/forms";

export class SignupComponent {

  signupForm = new FormGroup({

    username: new FormControl(
      "",
      Validators.required
    ),

    email: new FormControl(
      "",
      [
        Validators.required,
        Validators.email
      ]
    )

  });
```
  submitForm(): void {
    console.log(this.signupForm.value);
  }
}

### HTML
```
<form
  [formGroup]="signupForm"
  (ngSubmit)="submitForm()"
>

  <input
    formControlName="username"
    placeholder="Username"
  />

  <input
    formControlName="email"
    placeholder="Email"
  />

  <button
    type="submit"
    [disabled]="signupForm.invalid"
  >
    Submit
  </button>

</form>
```
Here, the form is created in TypeScript and the HTML connects to it using "[formGroup]" and "formControlName".

Reactive forms are especially useful when a form contains multiple fields, complex rules, or detailed validation.

---

# 8. Form Validation

Angular provides built-in validators for checking user input.

|Validator| Purpose|
|---|---|
|"Validators.required"| Value must be provided|
|"Validators.email"| Checks email format|
|"Validators.minLength(6)"| Requires at least 6 characters|
|"Validators.maxLength(20)"| Allows up to 20 characters|

You can check the complete form:

signupForm.valid
signupForm.invalid

You can also check an individual control:

signupForm.get("email")?.invalid

This allows the application to show validation messages or disable the submit button when the entered information is not valid.

---

# 9. Key Takeaways

- Service: A reusable class for common application logic and shared data.
- "@Injectable()": Makes a class available to Angular's Dependency Injection system.
- Dependency Injection: Angular provides services through the constructor instead of manually creating them.
- Singleton Service: A root-provided service can be shared across components.
- Data Sharing: Use "@Input()" / "@Output()" for direct parent-child communication and services for unrelated components.
- Template-Driven Forms: Use HTML and "ngModel"; suitable for simple forms.
- Reactive Forms: Build forms with "FormGroup" and "FormControl"; useful for complex forms.
- Validators: Provide rules such as required fields, email format, and length restrictions.
- Main Idea: Services handle reusable logic, while Angular Forms provide structured ways to collect and validate user input.
