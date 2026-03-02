# Lab 05 — Logout

## Paso 01 — Implementar Logout Centralizado

---

## 🎯 Objetivo

Implementar un logout que:

* Cierre sesión en Angular.
* Invalide la sesión en Keycloak.
* Redirija correctamente tras el cierre.
* Elimine los tokens del navegador.

En este laboratorio usamos exclusivamente el wrapper `keycloak-angular`.

---

# 🧠 Concepto clave

Cerrar sesión en una SPA con OIDC no significa solo borrar el token local.

Un logout correcto debe:

1️⃣ Invalidar la sesión en Keycloak.
2️⃣ Eliminar cookies del realm.
3️⃣ Eliminar tokens del navegador.
4️⃣ Redirigir al usuario a una URL segura.

Eso se denomina **Logout Centralizado**.

---

# 🛠 Paso 1 — Añadir botón de logout

Abrir:

```text
src/app/app.component.ts
```

Modificar el template para incluir botón:

```typescript
import { Component, inject } from '@angular/core';
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
      <a routerLink="/protected">Protected</a>
      <button (click)="logout()">Logout</button>
    </nav>

    <hr>

    <router-outlet></router-outlet>
  `
})
export class AppComponent {

  private keycloak = inject(KeycloakService);

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

# 🧪 Validar comportamiento

1. Inicia sesión.
2. Navega a `/protected`.
3. Pulsa **Logout**.

---

## 🔎 Resultado esperado

* El navegador redirige a Keycloak.
* La sesión del realm se elimina.
* Se redirige de nuevo a `http://localhost:4200`.
* Si intentas acceder a `/protected`, se vuelve a forzar login.

---

# 🧠 Qué está ocurriendo internamente

```typescript
this.keycloak.logout(window.location.origin);
```

Hace:

* Llamada al endpoint de logout de Keycloak.
* Invalidación de sesión.
* Eliminación de tokens.
* Redirección segura.

---

# 🔐 Por qué es importante el logout centralizado

Si solo borráramos el token local:

* La sesión en Keycloak seguiría activa.
* Un refresh automático podría reautenticar al usuario.
* No habría verdadero cierre de sesión.

Con logout centralizado:

✔ Sesión invalidada en el servidor
✔ Tokens eliminados
✔ Seguridad real

---

# 📌 Estado actual

Con este paso ya hemos cubierto:

* Bootstrap
* Guards
* Token claims
* Refresh automático
* Logout centralizado

El ciclo completo de autenticación está implementado.