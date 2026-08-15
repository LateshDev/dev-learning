# Angular · Day 5 — Route Guards

Route Guards are used to control access to Angular routes. They check a condition before a route is opened and decide whether the user can continue.

---

# 1. What is a Route Guard?

A route guard is a piece of logic that runs before Angular loads a route.

It can allow the navigation, stop it, or redirect the user to another page.

Think of it as a security checkpoint:
```
User requests a page
        ↓
Route Guard checks
        ↓
   Allowed?
   ↙️       ↘️
 Yes       No
 ↓          ↓
Page      Redirect
opens     to Login
```
For example, a profile page should normally be available only after login. If someone directly enters "/profile" without being authenticated, the guard can send them back to "/login".

---

# 2. Creating an Authentication Guard

One traditional way of creating a guard is by implementing "CanActivate".

## CLI command:

ng generate guard auth

When Angular asks which guard type to create, select CanActivate.

## "auth.guard.ts"
```
import { Injectable } from "@angular/core";
import { CanActivate, Router } from "@angular/router";
import { AuthService } from "./auth.service";

@Injectable({
  providedIn: "root"
})
export class AuthGuard implements CanActivate {

  constructor(
    private auth: AuthService,
    private router: Router
  ) {}

  canActivate(): boolean {

    if (this.auth.getToken()) {
      return true;
    }

    this.router.navigate(["/login"]);
    return false;
  }
}
```
The important part is the return value:

- "true" → navigation is allowed.
- "false" → navigation is stopped.

Here, the guard checks whether a token exists. If it does, the user can continue. Otherwise, the user is redirected to the login page.

---

# 3. Applying the Guard to a Route

After creating the guard, attach it to the route that should be protected.
```
// app-routing.module.ts

const routes: Routes = [

  {
    path: "login",
    component: LoginComponent
  },

  {
    path: "profile",
    component: ProfileComponent,
    canActivate: [AuthGuard]
  },

  {
    path: "",
    redirectTo: "login",
    pathMatch: "full"
  }

];
```
Now Angular checks "AuthGuard" before opening "/profile".
```
/profile requested
       ↓
   AuthGuard
       ↓
Token exists?
   ↙️       ↘️
 Yes       No
 ↓          ↓
Profile   Login
```
The same guard can be assigned to multiple protected routes.

---

# 4. NoAuthGuard — The Opposite Check

Sometimes we need the reverse behavior.

A user who is already logged in should not normally see the login or signup screen again. For this situation, we can create a NoAuthGuard.

Its rule is simple:

«Allow the route only when there is no authentication token.»
```
// no-auth.guard.ts

import { Injectable } from "@angular/core";
import { CanActivate, Router } from "@angular/router";
import { AuthService } from "./auth.service";

@Injectable({
  providedIn: "root"
})
export class NoAuthGuard implements CanActivate {

  constructor(
    private auth: AuthService,
    private router: Router
  ) {}

  canActivate(): boolean {

    if (!this.auth.getToken()) {
      return true;
    }

    this.router.navigate(["/profile"]);
    return false;
  }
}
```
Applying Both Guards
```
const routes: Routes = [

  {
    path: "login",
    component: LoginComponent,
    canActivate: [NoAuthGuard]
  },

  {
    path: "signup",
    component: SignupComponent,
    canActivate: [NoAuthGuard]
  },

  {
    path: "profile",
    component: ProfileComponent,
    canActivate: [AuthGuard]
  }

];
```
The two guards work in opposite ways:

|Guard| Used For| Condition|
|---|---|---|
|"AuthGuard"| Private pages| Token must exist|
|"NoAuthGuard"| Login/Signup| Token must not exist|

So:

AuthGuard: Logged in → allow private page.

NoAuthGuard: Logged out → allow login/signup.

---

# 5. Complete Authentication System

The guard is only one part of the complete authentication process.

The three important pieces work together:

## Login

The user provides credentials and receives a token.

## Interceptor

The interceptor attaches the saved token to outgoing HTTP requests.

## Route Guard

The guard controls which pages the user can open.
```
Login
  ↓
Token Received
  ↓
Token Stored
  ↓
Interceptor → Adds Token to API Requests
  ↓
Route Guard → Controls Page Access
  ↓
Protected Profile
```
Together, these features create a basic authentication flow for an Angular application.

---

# 6. Main Types of Angular Guards

Angular provides several types of guards for different navigation situations.

|Guard| When It Runs| Common Use|
|---|---|---|
|"CanActivate"| Before entering a route| Protect private pages|
|"CanDeactivate"| Before leaving a route| Prevent losing unsaved work|
|"CanActivateChild"| Before entering child routes| Protect nested routes|
|"Resolve"| Before route data is ready| Load required data first|

Example: "CanDeactivate"

Imagine a resume editor containing unsaved information.

If the user tries to leave the page, "CanDeactivate" can check for unsaved changes and ask whether they really want to leave.
```
User edits resume
      ↓
Tries to leave
      ↓
CanDeactivate checks
      ↓
Unsaved changes?
   ↙️          ↘️
 Yes          No
 ↓             ↓
Ask user     Leave
```
This prevents users from accidentally losing their work.

---

# 7. Key Takeaways

- A Route Guard checks navigation before a route is opened.
- "CanActivate" is commonly used to protect authenticated pages.
- Returning "true" allows navigation.
- Returning "false" prevents navigation.
- "AuthGuard" checks for a token before allowing access to private pages.
- "canActivate: [AuthGuard]" attaches the guard to a route.
- "NoAuthGuard" performs the opposite check and keeps authenticated users away from login/signup pages.
- "CanDeactivate" is useful for warning about unsaved changes.
- "CanActivateChild" can protect a group of child routes.
- Login + Interceptor + Route Guard together form the basic authentication flow.
