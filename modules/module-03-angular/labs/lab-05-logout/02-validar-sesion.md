# Lab 05 — Logout

## Paso 02 — Validar estado de sesión dinámicamente

---

## 🎯 Objetivo

Comprobar que:

* La sesión se elimina correctamente tras logout.
* Angular detecta el estado autenticado en tiempo real.
* La UI puede reaccionar dinámicamente al estado del usuario.

---

# 🧠 Contexto

Hasta ahora:

* Forzamos login con `login-required`.
* Mostramos claims del token.
* Implementamos refresh automático.
* Implementamos logout centralizado.

Ahora validaremos que el estado autenticado cambia correctamente tras cerrar sesión.

---

# 🛠 Paso 1 — Mostrar estado autenticado en la UI

Abrir:

```text
src/app/app.component.ts
```

Actualizar el componente:

```typescript
import { Component, inject, OnInit } from '@angular/core';
import { KeycloakService } from 'keycloak-angular';
import { RouterOutlet } from '@angular/router';
import { CommonModule } from '@angular/common';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [RouterOutlet, CommonModule],
  template: `
    <h1>Keycloak Training App</h1>

    <nav>
      <a routerLink="/">Home</a>
      <a routerLink="/protected" *ngIf="authenticated">Protected</a>

      <button *ngIf="authenticated" (click)="logout()">Logout</button>
      <button *ngIf="!authenticated" (click)="login()">Login</button>
    </nav>

    <p>
      Estado de sesión:
      <strong>{{ authenticated ? 'Autenticado' : 'No autenticado' }}</strong>
    </p>

    <hr>

    <router-outlet></router-outlet>
  `
})
export class AppComponent implements OnInit {

  private keycloak = inject(KeycloakService);

  authenticated = false;

  async ngOnInit() {
    this.authenticated = await this.keycloak.isLoggedIn();
  }

  login() {
    this.keycloak.login();
  }

  logout() {
    this.keycloak.logout(window.location.origin);
  }
}
```

---

# ▶️ Reiniciar aplicación

```bash
npm start
```

---

# 🧪 Escenario 1 — Usuario autenticado

* El enlace **Protected** debe mostrarse.
* El botón **Logout** debe estar visible.
* El estado indica: `Autenticado`.

---

# 🧪 Escenario 2 — Tras logout

* El botón **Logout** desaparece.
* Aparece botón **Login**.
* El enlace **Protected** desaparece.
* El estado indica: `No autenticado`.

---

# 🧠 Qué estamos validando

✔ `isLoggedIn()` refleja correctamente el estado actual.
✔ Logout invalida sesión en servidor.
✔ Angular actualiza la UI según el estado.
✔ La autorización en cliente puede basarse en el estado autenticado.
