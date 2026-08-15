# Angular Day 4 — Login, Profile & SCSS

# 1. Complete Login Flow

The basic authentication flow is:
```
User Login
    ↓
Send Email + Password
    ↓
Server Verification
    ↓
Receive Token
    ↓
Save Token
    ↓
Navigate to Profile
```
After login, the profile page requests user information from the server.

---

# 2. Auth Service — Login & Token

The authentication service handles login requests and token storage.
```
login(email: string, password: string) {
  return this.http.post(this.api + "/login", {
    email,
    password
  });
}

saveToken(token: string) {
  localStorage.setItem("token", token);
}

getToken() {
  return localStorage.getItem("token");
}

logout() {
  localStorage.removeItem("token");
}
```
The service keeps each task separate: login, save token, get token, and logout.

---

# 3. Login Component

The login component calls the authentication service when the user submits the form.
```
onLogin() {
  this.auth.login(this.email, this.password).subscribe(
    (response: any) => {
      this.auth.saveToken(response.token);
      this.router.navigate(["/profile"]);
    },
    () => {
      this.error = "Invalid email or password";
    }
  );
}

<input [(ngModel)]="email" placeholder="Email">

<input
  [(ngModel)]="password"
  type="password"
  placeholder="Password"
>

<button (click)="onLogin()">Login</button>

<p *ngIf="error">{{ error }}</p>
```
The token is saved inside "subscribe()" because it is available only after the server responds.

---

# 4. Profile Service

The profile service requests the current user's information.
```
getProfile(): Observable<any> {
  return this.http.get(this.api + "/me");
}
```
The service does not manually add the token. The HTTP interceptor takes care of it automatically.

---

# 5. HTTP Interceptor

An interceptor can attach the saved token to every outgoing HTTP request.
```
intercept(req: HttpRequest<any>, next: HttpHandler) {

  const token = this.auth.getToken();

  if (token) {
    const updatedRequest = req.clone({
      setHeaders: {
        Authorization: "Bearer " + token
      }
    });

    return next.handle(updatedRequest);
  }

  return next.handle(req);
}
```
This means protected API requests automatically contain the authentication token.

---

# 6. Profile Component

The profile component loads the user information when the page starts.
```
ngOnInit() {
  this.profile.getProfile().subscribe((data) => {
    this.user = data;
  });
}

<div *ngIf="user">
  <h2>{{ user.name }}</h2>
  <p>{{ user.email }}</p>
</div>
```
The profile is displayed only after the user data has been received.

---

# 7. Routing & Route Guard

Routes connect URLs with Angular components.
```
const routes: Routes = [
  { path: "login", component: LoginComponent },
  { path: "profile", component: ProfileComponent },
  { path: "", redirectTo: "login", pathMatch: "full" }
];
```
Use the router outlet to display the selected component:

<router-outlet></router-outlet>

A Route Guard can later protect "/profile" by checking whether the user has a valid token.

---

# 8. SCSS — Variables, Nesting & Mixins

SCSS provides additional features over normal CSS.

## Variables
```
$primary: #c0272d;
$radius: 6px;

Nesting

.login-box {

  input {
    padding: 10px;
  }

  button {
    background: $primary;

    &:hover {
      background: darken($primary, 10%);
    }
  }
}
```
## Mixins

```
@mixin flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
}

.login-box {
  @include flex-center;
}
```
Variables reduce repeated values, nesting keeps related styles organized, and mixins provide reusable style blocks.

---

# 9. Key Takeaways

- Auth Service handles login and token management.
- Token is saved after a successful login response.
- Interceptor automatically adds the token to API requests.
- Profile Service retrieves user information.
- Profile Component displays the received data.
- Routing connects URLs with Angular components.
- Route Guard can protect restricted pages.
- SCSS Variables, Nesting & Mixins make styles easier to reuse and maintain.
- Overall Flow: Login → Token → Profile Request → Interceptor → User Data.
